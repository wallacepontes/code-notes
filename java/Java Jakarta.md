# Jakarta EE

## Jakarta EE vs J2EE vs Java EE

Jakarta EE, J2EE, and Java EE represent different phases and evolutions of the enterprise Java platform. Here’s a detailed comparison to understand the differences and transitions between them:

### J2EE (Java 2 Platform, Enterprise Edition)

1. **Initial Release**: Late 1990s (starting with J2EE 1.2 in 1999).
2. **Naming**: The platform was originally called J2EE (Java 2 Platform, Enterprise Edition).
3. **Key Features**: 
   - Focus on building enterprise-level applications.
   - Included technologies like Servlets, JSP (JavaServer Pages), EJB (Enterprise JavaBeans), JMS (Java Message Service), and more.
4. **Goal**: To provide a standardized platform for developing multi-tier, web-based applications.
5. **Managed by**: Sun Microsystems.

### Java EE (Java Platform, Enterprise Edition)

1. **Renaming**: The name changed from J2EE to Java EE with the release of Java EE 5 in 2006.
2. **Key Improvements**:
   - Simplification of the development model.
   - Introduction of new APIs like JPA (Java Persistence API) and improvements in existing ones like EJB.
   - More focus on POJO (Plain Old Java Object) programming.
3. **Goal**: Continued focus on enterprise applications but with an emphasis on ease of development and modern practices.
4. **Managed by**: Sun Microsystems initially, and later Oracle after it acquired Sun Microsystems.

### Jakarta EE

1. **Transition**: In 2017, Oracle handed over the development of Java EE to the Eclipse Foundation. The platform was then rebranded as Jakarta EE.
2. **Naming**: The name change to Jakarta EE started with the release of Jakarta EE 8 in 2019.
3. **Key Changes**:
   - The initial Jakarta EE 8 release was essentially Java EE 8 but under the new Jakarta branding.
   - Jakarta EE 9 introduced namespace changes (from `javax.*` to `jakarta.*`) to allow for future enhancements and improvements.
   - Jakarta EE continues to evolve with a community-driven approach under the Eclipse Foundation, allowing for more rapid innovation and alignment with modern cloud-native and microservice architectures.
4. **Managed by**: The Eclipse Foundation.

### Comparison Summary

- **J2EE**: The original platform for enterprise Java development, focused on providing a standard for building enterprise applications.
- **Java EE**: The next evolution, emphasizing simplicity, modern development practices, and improved APIs.
- **Jakarta EE**: The current version, managed by the Eclipse Foundation, with ongoing development and community-driven innovations, including the namespace shift to `jakarta.*`.

### Key Technologies and Concepts

Regardless of the name changes, several core technologies and concepts have been central to all these versions:

- **Servlets and JSP**: For web application development.
- **EJB (Enterprise JavaBeans)**: For building modular, reusable components.
- **JPA (Java Persistence API)**: For object-relational mapping and database interactions.
- **JMS (Java Message Service)**: For messaging and asynchronous communication.
- **JSF (JavaServer Faces)**: For building component-based user interfaces.
- **CDI (Contexts and Dependency Injection)**: For dependency injection and lifecycle management.

### Example of Namespace Change

Under Java EE:
```java
import javax.persistence.Entity;
import javax.persistence.Id;

@Entity
public class MyEntity {
    @Id
    private Long id;
    // Other fields and methods
}
```

Under Jakarta EE:
```java
import jakarta.persistence.Entity;
import jakarta.persistence.Id;

@Entity
public class MyEntity {
    @Id
    private Long id;
    // Other fields and methods
}
```

### Future of Jakarta EE

Jakarta EE continues to evolve with a focus on modern application development, including cloud-native architectures, microservices, and other contemporary trends in software development. The community-driven approach ensures that it remains relevant and adapts to the needs of modern enterprise applications.

## Videos

 * [Jakarta EE vs J2EE vs Java EE | Are you Confused ?](https://www.youtube.com/watch?v=0l0ZtoWgOB0)
	> [<img src="https://img.youtube.com/vi/0l0ZtoWgOB0/0.jpg" width="200">](https://www.youtube.com/watch?v=0l0ZtoWgOB0 "Jakarta EE vs J2EE vs Java EE | Are you Confused ? by in28minutes - Get Cloud Certified 13,010 views 5 minutes, 50 seconds")
 * [Usando Java CDI em projetos Jakarta EE ou Microprofile](https://www.youtube.com/watch?v=iDGgNNLYaO4)
	> [<img src="https://img.youtube.com/vi/iDGgNNLYaO4/0.jpg" width="200">](https://www.youtube.com/watch?v=iDGgNNLYaO4 "Usando Java CDI em projetos Jakarta EE ou Microprofile by SouJava 1,292 views 1 hour, 57 minutes")
 * [25 Anos de Java: Modern Cloud Native Java/Jakarta EE Frameworks: tips, challenges, and trends](https://www.youtube.com/watch?v=jEkmVX9d5CY)
	> [<img src="https://img.youtube.com/vi/jEkmVX9d5CY/0.jpg" width="200">](https://www.youtube.com/watch?v=jEkmVX9d5CY "25 Anos de Java: Modern Cloud Native Java/Jakarta EE Frameworks: tips, challenges, and trends by SouJava 1,262 views 1 hour, 10 minutes")
 * [#boraescovarbit - Especial Jakarta EE Cookbook! | #java #jakartaee](https://www.youtube.com/watch?v=q3Ytmu7i3a0)
	> [<img src="https://img.youtube.com/vi/q3Ytmu7i3a0/0.jpg" width="200">](https://www.youtube.com/watch?v=q3Ytmu7i3a0 "#boraescovarbit - Especial Jakarta EE Cookbook! | #java #jakartaee by Elder Moraes 294 views 23 minutes")
