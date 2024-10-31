# Sprint Batch HandsOn

## Table of Contents
- [Sprint Batch HandsOn](#sprint-batch-handson)
  - [Table of Contents](#table-of-contents)
  - [Job steps in the Spring Batch](#job-steps-in-the-spring-batch)
    - [Maven Dependencies](#maven-dependencies)
    - [Configuration and Job Setup](#configuration-and-job-setup)
    - [Key Points:](#key-points)
  - [Step 3 and 4 in parallel](#step-3-and-4-in-parallel)
    - [1. Modify Job Configuration for Parallel Execution](#1-modify-job-configuration-for-parallel-execution)
    - [2. Explanation of the Modifications](#2-explanation-of-the-modifications)
    - [How it works:](#how-it-works)

## Job steps in the Spring Batch

To build this flow in Spring Batch, we can break it down into steps with corresponding job steps in the Spring Batch framework. Here’s how the flow can be structured:

1. **Drop and Recreate Tables**: You can use `JdbcTemplate` to drop and recreate the tables. This step will execute the DDL SQL commands.
2. **Insert Static Data into `mig_po_published`**: Run a script that inserts 10 records into the `mig_po_published` table.
3. **Extract Data from Sr and Ecm Databases**: Query the `Sr` and `Ecm` databases and insert the results into `mig_sr_pos` and `mig_ecm_pos` tables.
4. **Update `mig_po_published` Table**: Run SQL updates to mark corresponding records with `SR` and `ECM` boolean fields.
5. **Generate a Report**: Fetch the final data from the `mig_po_published` table and output it into a report format (e.g., a CSV file or console output).

Here is an example Spring Batch configuration:

### Maven Dependencies

Ensure your `pom.xml` includes the following dependencies:
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-batch</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
</dependency>
<dependency>
    <groupId>com.oracle.database.jdbc</groupId>
    <artifactId>ojdbc8</artifactId>
    <scope>runtime</scope>
</dependency>
```

### Configuration and Job Setup

```java
@Configuration
@EnableBatchProcessing
public class SanityCheckBatchConfig {

    @Autowired
    private DataSource sanityDataSource;  // DataSource for the sanity schema

    @Autowired
    private DataSource srDataSource;      // DataSource for the Sr database

    @Autowired
    private DataSource ecmDataSource;     // DataSource for the Ecm database

    @Bean
    public Job sanityCheckJob(JobBuilderFactory jobBuilderFactory, StepBuilderFactory stepBuilderFactory) {
        return jobBuilderFactory.get("sanityCheckJob")
                .start(dropAndRecreateTablesStep(stepBuilderFactory))
                .next(insertStaticDataStep(stepBuilderFactory))
                .next(fetchAndInsertSrPosStep(stepBuilderFactory))
                .next(fetchAndInsertEcmPosStep(stepBuilderFactory))
                .next(updateMigPoPublishedStep(stepBuilderFactory))
                .next(generateReportStep(stepBuilderFactory))
                .build();
    }

    // Step 1: Drop and Recreate Tables
    @Bean
    public Step dropAndRecreateTablesStep(StepBuilderFactory stepBuilderFactory) {
        return stepBuilderFactory.get("dropAndRecreateTablesStep")
                .tasklet((contribution, chunkContext) -> {
                    JdbcTemplate jdbcTemplate = new JdbcTemplate(sanityDataSource);
                    jdbcTemplate.execute("DROP TABLE IF EXISTS mig_po_published");
                    jdbcTemplate.execute("CREATE TABLE mig_po_published (PO VARCHAR(100), Buco BOOLEAN, Sr BOOLEAN, ECM BOOLEAN)");
                    jdbcTemplate.execute("DROP TABLE IF EXISTS mig_sr_pos");
                    jdbcTemplate.execute("CREATE TABLE mig_sr_pos (PO VARCHAR(100))");
                    jdbcTemplate.execute("DROP TABLE IF EXISTS mig_ecm_pos");
                    jdbcTemplate.execute("CREATE TABLE mig_ecm_pos (PO VARCHAR(100))");
                    return RepeatStatus.FINISHED;
                }).build();
    }

    // Step 2: Insert Static Data into mig_po_published
    @Bean
    public Step insertStaticDataStep(StepBuilderFactory stepBuilderFactory) {
        return stepBuilderFactory.get("insertStaticDataStep")
                .tasklet((contribution, chunkContext) -> {
                    JdbcTemplate jdbcTemplate = new JdbcTemplate(sanityDataSource);
                    for (int i = 1; i <= 10; i++) {
                        jdbcTemplate.update("INSERT INTO mig_po_published (PO, Buco, Sr, ECM) VALUES (?, TRUE, FALSE, FALSE)", "PO_" + i);
                    }
                    return RepeatStatus.FINISHED;
                }).build();
    }

    // Step 3: Fetch data from Sr database and insert into mig_sr_pos
    @Bean
    public Step fetchAndInsertSrPosStep(StepBuilderFactory stepBuilderFactory) {
        return stepBuilderFactory.get("fetchAndInsertSrPosStep")
                .tasklet((contribution, chunkContext) -> {
                    JdbcTemplate jdbcTemplateSr = new JdbcTemplate(srDataSource);
                    JdbcTemplate jdbcTemplateSanity = new JdbcTemplate(sanityDataSource);

                    List<String> srPos = jdbcTemplateSr.queryForList("SELECT po FROM sr_po_table", String.class);
                    for (String po : srPos) {
                        jdbcTemplateSanity.update("INSERT INTO mig_sr_pos (PO) VALUES (?)", po);
                    }
                    return RepeatStatus.FINISHED;
                }).build();
    }

    // Step 4: Fetch data from Ecm database and insert into mig_ecm_pos
    @Bean
    public Step fetchAndInsertEcmPosStep(StepBuilderFactory stepBuilderFactory) {
        return stepBuilderFactory.get("fetchAndInsertEcmPosStep")
                .tasklet((contribution, chunkContext) -> {
                    JdbcTemplate jdbcTemplateEcm = new JdbcTemplate(ecmDataSource);
                    JdbcTemplate jdbcTemplateSanity = new JdbcTemplate(sanityDataSource);

                    List<String> ecmPos = jdbcTemplateEcm.queryForList("SELECT po FROM ecm_po_table", String.class);
                    for (String po : ecmPos) {
                        jdbcTemplateSanity.update("INSERT INTO mig_ecm_pos (PO) VALUES (?)", po);
                    }
                    return RepeatStatus.FINISHED;
                }).build();
    }

    // Step 5: Update mig_po_published based on Sr and Ecm data
    @Bean
    public Step updateMigPoPublishedStep(StepBuilderFactory stepBuilderFactory) {
        return stepBuilderFactory.get("updateMigPoPublishedStep")
                .tasklet((contribution, chunkContext) -> {
                    JdbcTemplate jdbcTemplate = new JdbcTemplate(sanityDataSource);

                    // Update Sr
                    jdbcTemplate.update("UPDATE mig_po_published SET Sr = TRUE WHERE PO IN (SELECT PO FROM mig_sr_pos)");
                    // Update ECM
                    jdbcTemplate.update("UPDATE mig_po_published SET ECM = TRUE WHERE PO IN (SELECT PO FROM mig_ecm_pos)");

                    return RepeatStatus.FINISHED;
                }).build();
    }

    // Step 6: Generate Report
    @Bean
    public Step generateReportStep(StepBuilderFactory stepBuilderFactory) {
        return stepBuilderFactory.get("generateReportStep")
                .tasklet((contribution, chunkContext) -> {
                    JdbcTemplate jdbcTemplate = new JdbcTemplate(sanityDataSource);
                    List<Map<String, Object>> results = jdbcTemplate.queryForList("SELECT * FROM mig_po_published");

                    System.out.println("Health Check Report - mig_po_published:");
                    for (Map<String, Object> row : results) {
                        System.out.println(row);
                    }

                    return RepeatStatus.FINISHED;
                }).build();
    }
}
```

### Key Points:
1. **DataSources**: You will need three different `DataSource` configurations for the `sanity`, `sr`, and `ecm` databases.
2. **Tasklets**: Each step is implemented as a tasklet, where SQL queries are executed for dropping/recreating tables, inserting data, querying external databases, and updating rows.
3. **Job Flow**: The flow is sequential—each step is executed in order.
4. **Report Generation**: The final step generates a simple report by printing the result of `mig_po_published`.

This is a complete skeleton for your Spring Batch job that implements the desired steps. Let me know if you'd like further customization or adjustments!

## Step 3 and 4 in parallel

To run **Steps 3 (fetch data from Sr database)** and **Step 4 (fetch data from Ecm database)** in parallel in Spring Batch, you can use the `Flow` API and configure the steps to run asynchronously. Spring Batch supports parallel execution by using a `Flow` inside the job configuration, combined with an `AsyncTaskExecutor`.

Here’s how you can modify the job to run **Step 3** and **Step 4** in parallel:

### 1. Modify Job Configuration for Parallel Execution

```java
import org.springframework.core.task.SimpleAsyncTaskExecutor;
import org.springframework.batch.core.job.flow.Flow;

@Bean
public Job sanityCheckJob(JobBuilderFactory jobBuilderFactory, StepBuilderFactory stepBuilderFactory) {
    return jobBuilderFactory.get("sanityCheckJob")
            .start(dropAndRecreateTablesStep(stepBuilderFactory))  // Step 1
            .next(insertStaticDataStep(stepBuilderFactory))         // Step 2
            .split(new SimpleAsyncTaskExecutor())                   // Start parallel execution
            .add(fetchSrAndEcmParallelFlow(stepBuilderFactory))     // Steps 3 and 4 in parallel
            .next(updateMigPoPublishedStep(stepBuilderFactory))     // Step 5
            .next(generateReportStep(stepBuilderFactory))           // Step 6
            .build();
}

@Bean
public Flow fetchSrAndEcmParallelFlow(StepBuilderFactory stepBuilderFactory) {
    return new FlowBuilder<Flow>("fetchSrAndEcmParallelFlow")
            .start(fetchAndInsertSrPosStep(stepBuilderFactory))     // Step 3
            .split(new SimpleAsyncTaskExecutor())                   // Parallel execution using async task executor
            .add(fetchAndInsertEcmPosFlow(stepBuilderFactory))      // Step 4
            .build();
}

@Bean
public Flow fetchAndInsertEcmPosFlow(StepBuilderFactory stepBuilderFactory) {
    return new FlowBuilder<Flow>("fetchAndInsertEcmPosFlow")
            .start(fetchAndInsertEcmPosStep(stepBuilderFactory))
            .build();
}
```

### 2. Explanation of the Modifications

- **`split(new SimpleAsyncTaskExecutor())`**: This line is used to split the flow and allow parallel execution of the steps. The `SimpleAsyncTaskExecutor` provides a basic thread-based asynchronous execution model.
  
- **`Flow` API**: We encapsulate **Step 3** and **Step 4** inside their own `Flow` and use the `split()` method to run them in parallel. Each flow represents a sequence of steps, and by using `split`, Spring Batch will execute them asynchronously.

- **SimpleAsyncTaskExecutor**: This is a simple executor that creates a new thread for each task (in this case, each parallel flow).

### How it works:
- After completing **Step 2** (inserting static data), the job will **split** the execution into two parallel flows: one for fetching data from the **Sr** database (Step 3) and one for fetching data from the **Ecm** database (Step 4).
- Once both flows finish, the job will proceed to **Step 5** (updating `mig_po_published`) and continue to **Step 6** (generating the report).

This setup allows the steps fetching data from `Sr` and `Ecm` databases to execute concurrently, improving efficiency when both data sources are queried at the same time.