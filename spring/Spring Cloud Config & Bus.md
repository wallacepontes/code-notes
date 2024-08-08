# Spring Cloud Config & Bus

In a microservices architecture developed in Java with Spring Boot, I would recommend using a combination of Spring Cloud Config and Spring Cloud Bus for externalizing and managing configuration, including business rules, across microservices.

Spring Cloud Config provides a centralized configuration server that can be used to store and manage the configuration of all microservices. This way, any changes made to the configuration are immediately available to all microservices without the need for a code change or recompilation.

Spring Cloud Bus provides a mechanism for broadcasting configuration changes across microservices. This ensures that all microservices are aware of configuration changes and can update their configuration accordingly.

By using Spring Cloud Config and Spring Cloud Bus, you can externalize your business rules into a centralized configuration server and manage them separately from the code. This allows for easier configuration management and the ability to change business rules without needing to recompile the code. Additionally, it provides a centralized location for configuration, making it easier to manage and maintain a large number of microservices.

## Externalize the rules into a separate configuration

One common approach to remove business rules from compiled code is to externalize the rules into a separate configuration or data file. This way, the rules can be modified without changing the source code, and the changes can be applied without the need to recompile the code.

One way to achieve this is to use a rules engine or a business rules management system (BRMS), which allows you to define and manage rules separately from the code. The rules engine can evaluate the rules at runtime, based on the data provided, and return the appropriate result.

Another approach is to use a feature toggle system, where the business rules are encapsulated behind feature toggles. This means that the rules can be turned on or off at runtime without the need to recompile the code. This approach is particularly useful for A/B testing and experimentation.

Finally, you can use a configuration management system that allows you to store the business rules in a separate configuration file that can be read by the code at runtime. This way, the rules can be updated without changing the source code, and the changes can be applied without the need to recompile the code.

## Feature toggle system

Sure, here's a simple example using a feature toggle system in Java to a business rule like validating data fields:

Let's say you have a Java class called `UserData` that represents user data and has several fields such as `name`, `email`, `age`, etc. You want to implement a feature toggle system to validate the data fields based on a business rule.

First, you can define an interface called `Validator` that has a method `validate(UserData userData)` which takes a `UserData` object as a parameter and returns a boolean value indicating whether the data is valid or not. 

```java
public interface Validator {
    boolean validate(UserData userData);
}
```

Then, you can implement the `Validator` interface with a concrete validator class called `DataFieldValidator` that performs the validation based on your business rule.

```java
public class DataFieldValidator implements Validator {
    public boolean validate(UserData userData) {
        // Implement your business rule to validate the data fields here
        // Return true if the data is valid, false otherwise
    }
}
```

Next, you can use a feature toggle system to turn on or off the validation feature. For example, you can use a boolean flag called `dataFieldValidationEnabled` to control whether the validation feature is turned on or off. 

```java
public class FeatureToggle {
    private static boolean dataFieldValidationEnabled = true;

    public static boolean isDataFieldValidationEnabled() {
        return dataFieldValidationEnabled;
    }

    public static void setDataFieldValidationEnabled(boolean enabled) {
        dataFieldValidationEnabled = enabled;
    }
}
```

Finally, you can add the feature toggle check in your `UserData` class to determine whether to perform the validation or not.

```java
public class UserData {
    private String name;
    private String email;
    private int age;

    public UserData(String name, String email, int age) {
        this.name = name;
        this.email = email;
        this.age = age;
    }

    public boolean isValid() {
        if (FeatureToggle.isDataFieldValidationEnabled()) {
            Validator validator = new DataFieldValidator();
            return validator.validate(this);
        } else {
            return true; // If the validation feature is turned off, always return true
        }
    }
}
```

With this setup, you can turn on or off the data field validation feature by calling `FeatureToggle.setDataFieldValidationEnabled(true/false)` and the `isValid()` method of the `UserData` class will perform the validation or skip it based on the value of the `dataFieldValidationEnabled` flag.


## Create a project in github
1. http://localhost:8888/application/default

## Videos

 * [Spring Cloud Config for Shared Microservice Configuration](https://www.youtube.com/watch?v=uFPbUqrbanc)
	> [<img src="https://img.youtube.com/vi/uFPbUqrbanc/0.jpg" width="200">](https://www.youtube.com/watch?v=uFPbUqrbanc "This video shows how to use Spring Cloud Config to share your Spring Boot configuration to microservices. by OktaDev 7,800 views 14 minutes, 26 seconds")

 * [Set up spring cloud config server from scratch - Microservice configuration with Spring Boot [11]](https://www.youtube.com/watch?v=gb1i4WyWNK4)
	> [<img src="https://img.youtube.com/vi/gb1i4WyWNK4/0.jpg" width="200">](https://www.youtube.com/watch?v=gb1i4WyWNK4 "Spring Boot Microservices Level 3: Microservice configuration. by Java Brains 142,000 views 11 minutes, 04 seconds")

 * [How to use Spring Cloud Config Server with Git and Vault | Microservices 4](https://www.youtube.com/watch?v=p65u4t26BBc)
	> [<img src="https://img.youtube.com/vi/p65u4t26BBc/0.jpg" width="200">](https://www.youtube.com/watch?v=p65u4t26BBc "how to use Spring Cloud Config Server with two sources: Git and Vault. by Sergio Lema 10,000 views 10 minutes, 49 seconds")

 * [Spring Cloud Config Server with Example in a Spring Boot App | Tech Primers](https://www.youtube.com/watch?v=b2ih5RCuxTM)
	> [<img src="https://img.youtube.com/vi/b2ih5RCuxTM/0.jpg" width="200">](https://www.youtube.com/watch?v=b2ih5RCuxTM "This video covers the Spring Cloud Config Server with an Example using Spring Boot app. by Tech Primers 74,000 views 19 minutes, 04 seconds")



## References
1. https://docs.spring.io/spring-cloud-config/docs/current/reference/html/#_spring_cloud_config_server
2. https://spring.io/projects/spring-cloud-bus
3. https://cloud.spring.io/spring-cloud-bus/reference/html/#quick-start
4. https://github.com/spring-cloud/spring-cloud-config
5. https://github.com/serlesen/microservices-bookstore
6. https://martinfowler.com/articles/feature-toggles.html
