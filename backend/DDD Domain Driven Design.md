# DDD

## The focus is on modeling the business domain

That's a great observation! In DDD, the focus is on modeling the business domain, and not just on data persistence or database tables. The entities represent the real-world objects in the domain, and they should contain not only data but also behavior.

In your example, the Car entity should contain not only properties such as the number of doors and color but also methods such as start() and accelerate(). Similarly, the Cliente entity should contain behavior related to its credit, such as methods for making payments and checking the credit limit.

It's also important to note that DDD encourages a rich domain model, where the behavior of the entities is encapsulated within them. This means that the business rules related to the credit calculation should be contained within the Cliente entity itself, instead of being in a separate BLCliente class. This makes the domain model more expressive and easier to understand, as the behavior of the entities is more closely related to the business rules of the domain.

Overall, DDD can help you create a more robust and expressive domain model, where the entities represent real-world objects with behavior and not just data. By focusing on the business domain, you can create more maintainable and understandable code that better reflects the needs of the business.

## bounded context

In Domain-Driven Design (DDD), a "bounded context" is a concept that refers to a specific domain or area of the business with its own language, rules and models. It helps to delimit the boundaries of the problem and the solution space, and to ensure that the domain model is consistent and does not become too complex. Each Bounded Context has its own model, and concepts in one Bounded Context might have different meanings or representations in another one.

The goal of using bounded contexts is to minimize the complexity of the overall system by breaking it down into smaller, more manageable parts. By defining clear boundaries between different areas of the business, it becomes easier to understand the relationships and dependencies between different parts of the system, and to make changes and improvements without affecting other areas.

In practice, different teams or sub-teams can be responsible for different bounded contexts, and they can use different tools or technologies to implement their models. Communication between the teams is important to ensure consistency and alignment and avoid confusion or misunderstandings.

Overall, Bounded Contexts can help to improve the maintainability and scalability of the system and make it more adaptable to changing business requirements.


## Repository

In DDD, the responsibility of persisting the entities lies with a Repository, which acts as an abstraction layer between the domain model and the data access layer. The repository provides a way for the domain model to save and retrieve entities without exposing details about how the data is persisted.

The repository interface should be defined in the domain layer, and its implementation should be provided by the data access layer. The repository should be responsible for mapping the entities to the database schema and managing the transactional boundaries.

Regarding the question of whether the entity itself should be responsible for persisting itself in the database, the answer is no. Entities should not have any knowledge of the persistence mechanism, as it would break the separation of concerns principle. Entities should focus on their own behavior and the business rules of the domain, and the persistence logic should be handled by the repository.

It's worth noting that DDD does not prescribe a specific persistence mechanism or technology. The repository interface can be implemented using any data access technology, such as an ORM or a plain SQL database access layer.

In summary, the responsibility of persisting entities in DDD lies with a repository, which acts as an abstraction layer between the domain model and the data access layer. Entities should not have any knowledge of the persistence mechanism, and their focus should be on their own behavior and the business rules of the domain.

### The Repository pattern is a key pattern in DDD

The Repository pattern is a key pattern in DDD, which helps to abstract the persistence mechanism and provide a clean separation between the domain model and the data access layer.

In the Repository pattern, the Repository interface defines a set of methods for querying and persisting entities, and its implementation is responsible for handling the persistence details. The repository acts as a bridge between the domain model and the data access layer, shielding the domain model from the persistence details and providing a simple and uniform interface for accessing the data.

The repository interface should be defined in the domain layer, while its implementation should be provided by the data access layer. The implementation can use any persistence technology, such as an ORM, a plain SQL database access layer, or a document database, without affecting the domain model.

By using the Repository pattern, the domain model can focus on the business logic and the behavior of the entities, without being coupled to the persistence details. This makes the code more maintainable, testable, and adaptable, as changes in the persistence mechanism can be easily handled by changing the implementation of the repository, without affecting the domain model.

Overall, the Repository pattern is a powerful tool for implementing persistence in DDD, and it helps to maintain a clean and modular architecture, with a clear separation of concerns between the domain model and the data access layer.

## Code example

Sure, here's an example of how the Repository pattern can be used in a simple e-commerce domain model:

Let's say we have a domain model for an e-commerce application that includes a Customer entity. The Customer entity has properties such as name, email, and address, as well as methods for placing orders, checking the order history, and updating the customer information.

To persist the Customer entity, we can define a CustomerRepository interface in the domain layer, which includes methods for saving, retrieving, and deleting Customer entities:

```java
public interface CustomerRepository {
  void save(Customer customer);
  Customer getById(long id);
  void delete(Customer customer);
}
```

The implementation of the CustomerRepository interface can be provided by the data access layer, using any persistence technology such as JDBC, JPA, or MongoDB. Here's an example implementation using JPA:

```java
public class JpaCustomerRepository implements CustomerRepository {
  private EntityManager entityManager;

  public JpaCustomerRepository(EntityManager entityManager) {
    this.entityManager = entityManager;
  }

  @Override
  public void save(Customer customer) {
    entityManager.persist(customer);
  }

  @Override
  public Customer getById(long id) {
    return entityManager.find(Customer.class, id);
  }

  @Override
  public void delete(Customer customer) {
    entityManager.remove(customer);
  }
}
```

The Customer entity itself does not have any knowledge of the persistence mechanism, and its focus is on its own behavior and the business rules of the domain. Here's an example Customer entity class:

```java
public class Customer {
  private long id;
  private String name;
  private String email;
  private String address;

  public Customer(String name, String email, String address) {
    this.name = name;
    this.email = email;
    this.address = address;
  }

  public void placeOrder(Order order) {
    // business logic for placing an order
  }

  public List<Order> getOrderHistory() {
    // business logic for retrieving the order history
  }

  public void updateCustomerInfo(String name, String email, String address) {
    // business logic for updating the customer information
  }

  // getters and setters
}
```

In this example, the Customer entity can be created and manipulated in the domain layer, using its public methods, without being coupled to the persistence details. The persistence mechanism is abstracted by the CustomerRepository interface, which is implemented by the JpaCustomerRepository class in the data access layer.

By using the Repository pattern, we achieve a clean separation of concerns between the domain model and the data access layer, and a clear and modular architecture that is easy to maintain, test, and adapt to changing requirements.

## DTOs

In DDD, the primary focus is on modeling the domain, which includes entities, value objects, aggregates, and domain services. The domain model should be decoupled from the infrastructure, including the presentation layer and data access layer.

To serve a frontend application, it's common to use DTOs (Data Transfer Objects) to transfer data between the backend and the frontend. DTOs are simple data structures that contain only the data needed for a specific use case, and they are designed to be easily serializable and deserializable.

In DDD, DTOs can be used to map the domain objects to simpler data structures that can be used by the frontend. This mapping can be done using an adapter or mapping layer that converts the domain objects to DTOs and vice versa.

Here's an example of how DTOs can be used to serve a frontend application in a DDD context:

Let's say we have a domain model for an e-commerce application that includes a Customer entity, as shown in the previous example. To serve a frontend application, we can define a CustomerDTO class that contains only the data needed by the frontend:

```java
public class CustomerDTO {
  private long id;
  private String name;
  private String email;

  public CustomerDTO(long id, String name, String email) {
    this.id = id;
    this.name = name;
    this.email = email;
  }

  // getters and setters
}
```

To map the Customer entity to a CustomerDTO, we can define a CustomerMapper class that converts the Customer entity to a CustomerDTO:

```java
public class CustomerMapper {
  public static CustomerDTO toDTO(Customer customer) {
    return new CustomerDTO(customer.getId(), customer.getName(), customer.getEmail());
  }

  public static Customer toEntity(CustomerDTO customerDTO) {
    return new Customer(customerDTO.getName(), customerDTO.getEmail());
  }
}
```

The CustomerMapper class can be used in the controller layer of the backend to convert the Customer entity to a CustomerDTO before sending it to the frontend, and to convert the CustomerDTO received from the frontend to a Customer entity before persisting it in the database.

```java
@RestController
@RequestMapping("/customers")
public class CustomerController {
  private CustomerRepository customerRepository;

  @GetMapping("/{id}")
  public CustomerDTO getCustomer(@PathVariable long id) {
    Customer customer = customerRepository.getById(id);
    return CustomerMapper.toDTO(customer);
  }

  @PostMapping("/")
  public void createCustomer(@RequestBody CustomerDTO customerDTO) {
    Customer customer = CustomerMapper.toEntity(customerDTO);
    customerRepository.save(customer);
  }
}
```

By using DTOs and a mapping layer, we achieve a clear separation between the domain model and the presentation layer, and a modular architecture that is easy to test, maintain, and evolve.

## Videos
 * [DDD &amp; REST - Domain Driven APIs for the web - Oliver Gierke](https://www.youtube.com/watch?v=NdZqeAAIHzc)
	> [<img src="https://img.youtube.com/vi/NdZqeAAIHzc/0.jpg" width="200">](https://www.youtube.com/watch?v=NdZqeAAIHzc "DDD &amp; REST - Domain Driven APIs for the web - Oliver Gierke 36,208 views 1 hour, 50 minutes")
 * [Domain Driven Design do Jeito Certo](https://www.youtube.com/watch?v=cz6EU7Z_BhE)
	> [<img src="https://img.youtube.com/vi/cz6EU7Z_BhE/0.jpg" width="200">](https://www.youtube.com/watch?v=cz6EU7Z_BhE "Domain Driven Design do Jeito Certo by Full Cycle 36,208 views 1 hour, 50 minutes")
 * [Domain Driven Design nos dias atuais](https://www.youtube.com/watch?v=lQCKJvpNCoQ)
	> [<img src="https://img.youtube.com/vi/lQCKJvpNCoQ/0.jpg" width="200">](https://www.youtube.com/watch?v=lQCKJvpNCoQ "Domain Driven Design nos dias atuais by balta.io 18,643 views 1 hour, 6 minutes")
 * [Domain-Driven Refactoring - Jimmy Bogard - NDC London 2022](https://www.youtube.com/watch?v=f64tZ90Dntg)
	> [<img src="https://img.youtube.com/vi/f64tZ90Dntg/0.jpg" width="200">](https://www.youtube.com/watch?v=f64tZ90Dntg "Domain-Driven Refactoring - Jimmy Bogard - NDC London 2022 by NDC Conferences 41,342 views 1 hour")
 * [Domain Driven Design: What You Need To Know](https://www.youtube.com/watch?v=4rhzdZIDX_k)
	> [<img src="https://img.youtube.com/vi/4rhzdZIDX_k/0.jpg" width="200">](https://www.youtube.com/watch?v=4rhzdZIDX_k "Domain Driven Design: What You Need To Know by Alex Hyett 70,898 views 8 minutes, 42 seconds")
 * [Czym jest &quot;model&quot; i jak go projektować? | Domain-Driven Design](https://www.youtube.com/watch?v=HJHmSH4X_dk)
	> [<img src="https://img.youtube.com/vi/HJHmSH4X_dk/0.jpg" width="200">](https://www.youtube.com/watch?v=HJHmSH4X_dk "Czym jest &quot;model&quot; i jak go projektować? | Domain-Driven Design by DevMentors 9,276 views 1 hour, 35 minutes")
 * [Taktyczne Domain-Driven Design | Masterclass](https://www.youtube.com/watch?v=18sjru-OyhE)
	> [<img src="https://img.youtube.com/vi/18sjru-OyhE/0.jpg" width="200">](https://www.youtube.com/watch?v=18sjru-OyhE "Taktyczne Domain-Driven Design | Masterclass by DevMentors 14,705 views 6 hours, 9 minutes")
 * [IMPLEMENTANDO UM CRUD COM DOMAIN-DRIVEN DESIGN](https://www.youtube.com/watch?v=1Chr58HqrrM)
	> [<img src="https://img.youtube.com/vi/1Chr58HqrrM/0.jpg" width="200">](https://www.youtube.com/watch?v=1Chr58HqrrM "IMPLEMENTANDO UM CRUD COM DOMAIN-DRIVEN DESIGN by Road to Agility 2,309 views 1 hour, 4 minutes")
* [Domain Logic: Where does it go?](https://www.youtube.com/watch?v=PrJIMTZsbDw)
	> [<img src="https://img.youtube.com/vi/PrJIMTZsbDw/0.jpg" width="200">](https://www.youtube.com/watch?v=PrJIMTZsbDw "Domain Logic: Where does it go? by CodeOpinion 19,400 views 12 minutes, 1 second")
 * [Arquitetura Hexagonal na prática | 📎 Zup Clipes ✂️](https://www.youtube.com/watch?v=QnCFbYjT644)
	> [<img src="https://img.youtube.com/vi/QnCFbYjT644/0.jpg" width="200">](https://www.youtube.com/watch?v=QnCFbYjT644 "Arquitetura Hexagonal na prática | 📎 Zup Clipes ✂️ by Zup 2,002 views 10 minutes, 6 seconds")
 * [DDD Explained in 9 MINUTES | What is Domain Driven Design?](https://www.youtube.com/watch?v=kbGYy49fCz4)
	> [<img src="https://img.youtube.com/vi/kbGYy49fCz4/0.jpg" width="200">](https://www.youtube.com/watch?v=kbGYy49fCz4 "DDD Explained in 9 MINUTES | What is Domain Driven Design? by Marco Lenzo 29,792 views 9 minutes, 44 seconds")
 * [Esta é a regra MAIS IMPORTANTE do Clean Code! | #dev #code #programação  #balta](https://www.youtube.com/watch?v=CpfbAfidVeA)
	> [<img src="https://img.youtube.com/vi/CpfbAfidVeA/0.jpg" width="200">](https://www.youtube.com/watch?v=CpfbAfidVeA "Esta é a regra MAIS IMPORTANTE do Clean Code! | #dev #code #programação  #balta by balta.io 44,314 views 7 minutes, 12 seconds")
 * [Hexagonal Architecture: What You Need To Know - Simple Explanation](https://www.youtube.com/watch?v=bDWApqAUjEI)
	> [<img src="https://img.youtube.com/vi/bDWApqAUjEI/0.jpg" width="200">](https://www.youtube.com/watch?v=bDWApqAUjEI "Hexagonal Architecture: What You Need To Know - Simple Explanation by Alex Hyett 51,671 views 8 minutes, 16 seconds")
 * [DDD is just giving a $h!t about your Domain](https://www.youtube.com/watch?v=i0aGAdgbG7A)
	> [<img src="https://img.youtube.com/vi/i0aGAdgbG7A/0.jpg" width="200">](https://www.youtube.com/watch?v=i0aGAdgbG7A "DDD is just giving a $h!t about your Domain by CodeOpinion 14,942 views 8 minutes, 35 seconds")
 * [What makes an Aggregate (DDD)? Hint: it&#39;s NOT hierarchy &amp; relationships](https://www.youtube.com/watch?v=djq0293b2bA)
	> [<img src="https://img.youtube.com/vi/djq0293b2bA/0.jpg" width="200">](https://www.youtube.com/watch?v=djq0293b2bA "What makes an Aggregate (DDD)? Hint: it&#39;s NOT hierarchy &amp; relationships by CodeOpinion 24,856 views 9 minutes, 32 seconds")
 * [Modeling a Domain With Domain-Driven Design From Scratch | DDD](https://www.youtube.com/watch?v=fO2T5tRu3DE)
	> [<img src="https://img.youtube.com/vi/fO2T5tRu3DE/0.jpg" width="200">](https://www.youtube.com/watch?v=fO2T5tRu3DE "Modeling a Domain With Domain-Driven Design From Scratch | DDD by Milan Jovanović 69,758 views 19 minutes")
 * [Czym jest AGREGAT w DDD?](https://www.youtube.com/watch?v=g6OVgndzYpQ)
	> [<img src="https://img.youtube.com/vi/g6OVgndzYpQ/0.jpg" width="200">](https://www.youtube.com/watch?v=g6OVgndzYpQ "Czym jest AGREGAT w DDD? by DevMentors 6,860 views 1 hour, 5 minutes")
 * [O que é Domain-Driven Design [DDD]? | 📎 Zup Clipes ✂️](https://www.youtube.com/watch?v=24asxVs56wA)
	> [<img src="https://img.youtube.com/vi/24asxVs56wA/0.jpg" width="200">](https://www.youtube.com/watch?v=24asxVs56wA "O que é Domain-Driven Design [DDD]? | 📎 Zup Clipes ✂️ by Zup 263 views 1 minute, 44 seconds")
 * [Modeling a Domain With Domain-Driven Design From Scratch | DDD](https://www.youtube.com/watch?v=fO2T5tRu3DE)
	> [<img src="https://img.youtube.com/vi/fO2T5tRu3DE/0.jpg" width="200">](https://www.youtube.com/watch?v=fO2T5tRu3DE "Modeling a Domain With Domain-Driven Design From Scratch | DDD by Milan Jovanović 69,758 views 19 minutes")
 * [Agregados | DDD do jeito certo | Parte 06](https://www.youtube.com/watch?v=1AEOcQWQR2o)
	> [<img src="https://img.youtube.com/vi/1AEOcQWQR2o/0.jpg" width="200">](https://www.youtube.com/watch?v=1AEOcQWQR2o "Agregados | DDD do jeito certo | Parte 06 by EximiaCo - Excelência Tecnológica  18,000 views 17 minutes")
 * [Descomplicando &quot;Arquitetura Hexagonal&quot;](https://www.youtube.com/watch?v=V7JnDDQY1m0)
	> [<img src="https://img.youtube.com/vi/V7JnDDQY1m0/0.jpg" width="200">](https://www.youtube.com/watch?v=V7JnDDQY1m0 "Descomplicando &quot;Arquitetura Hexagonal&quot; by EximiaCo - Excelência Tecnológica  26,362 views 14 minutes, 10 seconds")
 * [Arquitetura Hexagonal: O que você precisa saber](https://www.youtube.com/watch?v=or5zAOASPjU)
	> [<img src="https://img.youtube.com/vi/or5zAOASPjU/0.jpg" width="200">](https://www.youtube.com/watch?v=or5zAOASPjU "Arquitetura Hexagonal: O que você precisa saber by Full Cycle 51,188 views 46 minutes")
 * [&quot;Clean Code Envelheceu como Leite!&quot; Será?](https://www.youtube.com/watch?v=6x9vo-VKm1g)
	> [<img src="https://img.youtube.com/vi/6x9vo-VKm1g/0.jpg" width="200">](https://www.youtube.com/watch?v=6x9vo-VKm1g "&quot;Clean Code Envelheceu como Leite!&quot; Será? by Código Fonte TV 60,195 views 19 minutes")
 * [Clean Code // Dicionário do Programador](https://www.youtube.com/watch?v=ln6t3uyTveQ)
	> [<img src="https://img.youtube.com/vi/ln6t3uyTveQ/0.jpg" width="200">](https://www.youtube.com/watch?v=ln6t3uyTveQ "Clean Code // Dicionário do Programador by Código Fonte TV 164,931 views 14 minutes, 22 seconds")
 * [DIFERENCIANDO UM CRUD DE UM CONTEXTO DDD](https://www.youtube.com/watch?v=U_4bRv3fVy8)
	> [<img src="https://img.youtube.com/vi/U_4bRv3fVy8/0.jpg" width="200">](https://www.youtube.com/watch?v=U_4bRv3fVy8 "DIFERENCIANDO UM CRUD DE UM CONTEXTO DDD by Road to Agility 1,941 views 50 minutes")
 * [Clean Architecture + DDD: Você pensa que sabe. Só que não!](https://www.youtube.com/watch?v=jbQF7LR5jvY)
	> [<img src="https://img.youtube.com/vi/jbQF7LR5jvY/0.jpg" width="200">](https://www.youtube.com/watch?v=jbQF7LR5jvY "Clean Architecture + DDD: Você pensa que sabe. Só que não! by Full Cycle 10,102 views 22 minutes")
 * [Solidity Tutorial - A Full Course on Ethereum, Blockchain Development, Smart Contracts, and the EVM](https://www.youtube.com/watch?v=ipwxYa-F1uY)
	> [<img src="https://img.youtube.com/vi/ipwxYa-F1uY/0.jpg" width="200">](https://www.youtube.com/watch?v=ipwxYa-F1uY "Solidity Tutorial - A Full Course on Ethereum, Blockchain Development, Smart Contracts, and the EVM by freeCodeCamp.org 1,490,695 views 1 hour, 30 minutes")
 * [Comparativo entre Clean Architecture e DDD (Domain Driven Design)](https://www.youtube.com/watch?v=3qgnmkLTXVs)
	> [<img src="https://img.youtube.com/vi/3qgnmkLTXVs/0.jpg" width="200">](https://www.youtube.com/watch?v=3qgnmkLTXVs "Comparativo entre Clean Architecture e DDD (Domain Driven Design) by Full Cycle 18,993 views 39 minutes")
 * [Programadores Profissionais Devem Saber dar Nomes // Entendendo o Livro Clean Code (Capítulo 2)](https://www.youtube.com/watch?v=aEPn7VV45kU)
	> [<img src="https://img.youtube.com/vi/aEPn7VV45kU/0.jpg" width="200">](https://www.youtube.com/watch?v=aEPn7VV45kU "Programadores Profissionais Devem Saber dar Nomes // Entendendo o Livro Clean Code (Capítulo 2) by Código Fonte TV 43,091 views 20 minutes")
 * [DDD, Domain-Driven Design com Elemar Jr. // Live #52](https://www.youtube.com/watch?v=NsQnmmIykoE)
	> [<img src="https://img.youtube.com/vi/NsQnmmIykoE/0.jpg" width="200">](https://www.youtube.com/watch?v=NsQnmmIykoE "DDD, Domain-Driven Design com Elemar Jr. // Live #52 by Rodrigo Branas 34,358 views 3 hours, 26 minutes")
 * [O Design Pattern que virou Vilão! Singleton na Prática + Alternativa Clean Code // Mão no Código #43](https://www.youtube.com/watch?v=yimeXZ1twWs)
	> [<img src="https://img.youtube.com/vi/yimeXZ1twWs/0.jpg" width="200">](https://www.youtube.com/watch?v=yimeXZ1twWs "O Design Pattern que virou Vilão! Singleton na Prática + Alternativa Clean Code // Mão no Código #43 by Código Fonte TV 31,106 views 12 minutes, 36 seconds")
 * [DDD (Domain-Driven Design) // Dicionário do Programador](https://www.youtube.com/watch?v=GE6asEjTFv8)
	> [<img src="https://img.youtube.com/vi/GE6asEjTFv8/0.jpg" width="200">](https://www.youtube.com/watch?v=GE6asEjTFv8 "DDD (Domain-Driven Design) // Dicionário do Programador by Código Fonte TV 68,453 views 11 minutes")
 * [Subdomínios e contextos delimitados | DDD do jeito certo | Parte 03](https://www.youtube.com/watch?v=9hlRHZ4Pfyo)
	> [<img src="https://img.youtube.com/vi/9hlRHZ4Pfyo/0.jpg" width="200">](https://www.youtube.com/watch?v=9hlRHZ4Pfyo "Subdomínios e contextos delimitados | DDD do jeito certo | Parte 03 by EximiaCo - Excelência Tecnológica  23,928 views 18 minutes")
 * [Linguagem Onipresente (Ubiquitous Language) | DDD do jeito certo | Parte 02](https://www.youtube.com/watch?v=HnvmpyUAITs)
	> [<img src="https://img.youtube.com/vi/HnvmpyUAITs/0.jpg" width="200">](https://www.youtube.com/watch?v=HnvmpyUAITs "Linguagem Onipresente (Ubiquitous Language) | DDD do jeito certo | Parte 02 by EximiaCo - Excelência Tecnológica  22,859 views 17 minutes")
 * [Atacando a complexidade | DDD do jeito certo | Parte 01](https://www.youtube.com/watch?v=2X9Q97u4tUg)
	> [<img src="https://img.youtube.com/vi/2X9Q97u4tUg/0.jpg" width="200">](https://www.youtube.com/watch?v=2X9Q97u4tUg "Atacando a complexidade | DDD do jeito certo | Parte 01 by EximiaCo - Excelência Tecnológica  50,026 views 7 minutes, 55 seconds")
 * [Agregados | DDD do jeito certo | Parte 06](https://www.youtube.com/watch?v=1AEOcQWQR2o)
	> [<img src="https://img.youtube.com/vi/1AEOcQWQR2o/0.jpg" width="200">](https://www.youtube.com/watch?v=1AEOcQWQR2o "Agregados | DDD do jeito certo | Parte 06 by EximiaCo - Excelência Tecnológica  18,000 views 17 minutes")
 * [Entidades e Objetos de Valor | DDD do jeito certo | Parte 05](https://www.youtube.com/watch?v=6Pjod34OCnE)
	> [<img src="https://img.youtube.com/vi/6Pjod34OCnE/0.jpg" width="200">](https://www.youtube.com/watch?v=6Pjod34OCnE "Entidades e Objetos de Valor | DDD do jeito certo | Parte 05 by EximiaCo - Excelência Tecnológica  16,526 views 14 minutes, 56 seconds")
 * [Serviços de Domínio (Domain Services) | DDD do jeito certo | Parte 11](https://www.youtube.com/watch?v=bpETAJ_8y6Y)
	> [<img src="https://img.youtube.com/vi/bpETAJ_8y6Y/0.jpg" width="200">](https://www.youtube.com/watch?v=bpETAJ_8y6Y "Serviços de Domínio (Domain Services) | DDD do jeito certo | Parte 11 by EximiaCo - Excelência Tecnológica  12,500 views 14 minutes, 25 seconds")
 * [Atacando a complexidade | DDD do jeito certo | Parte 01](https://www.youtube.com/watch?v=2X9Q97u4tUg)
	> [<img src="https://img.youtube.com/vi/2X9Q97u4tUg/0.jpg" width="200">](https://www.youtube.com/watch?v=2X9Q97u4tUg "Atacando a complexidade | DDD do jeito certo | Parte 01 by EximiaCo - Excelência Tecnológica  50,026 views 7 minutes, 55 seconds")
 * [Módulos - Organizando o código de maneira eficiente | DDD do jeito certo | Parte 13](https://www.youtube.com/watch?v=TVdAyLhR1uw)
	> [<img src="https://img.youtube.com/vi/TVdAyLhR1uw/0.jpg" width="200">](https://www.youtube.com/watch?v=TVdAyLhR1uw "Módulos - Organizando o código de maneira eficiente | DDD do jeito certo | Parte 13 by EximiaCo - Excelência Tecnológica  9,947 views 12 minutes, 14 seconds")
 * [Factories - O que são e quando utilizar? | DDD do jeito certo | Parte 12](https://www.youtube.com/watch?v=vGhtcdP5gsI)
	> [<img src="https://img.youtube.com/vi/vGhtcdP5gsI/0.jpg" width="200">](https://www.youtube.com/watch?v=vGhtcdP5gsI "Factories - O que são e quando utilizar? | DDD do jeito certo | Parte 12 by EximiaCo - Excelência Tecnológica  8,812 views 9 minutes, 37 seconds")
 * [Serviços de Domínio (Domain Services) | DDD do jeito certo | Parte 11](https://www.youtube.com/watch?v=bpETAJ_8y6Y)
	> [<img src="https://img.youtube.com/vi/bpETAJ_8y6Y/0.jpg" width="200">](https://www.youtube.com/watch?v=bpETAJ_8y6Y "Serviços de Domínio (Domain Services) | DDD do jeito certo | Parte 11 by EximiaCo - Excelência Tecnológica  12,500 views 14 minutes, 25 seconds")
 * [Que idioma utilizar no código que expressa o modelo de domínio? | DDD do jeito certo | Parte 10](https://www.youtube.com/watch?v=uh_iLnsjKyg)
	> [<img src="https://img.youtube.com/vi/uh_iLnsjKyg/0.jpg" width="200">](https://www.youtube.com/watch?v=uh_iLnsjKyg "Que idioma utilizar no código que expressa o modelo de domínio? | DDD do jeito certo | Parte 10 by EximiaCo - Excelência Tecnológica  7,414 views 11 minutes, 24 seconds")
 * [Repositórios | DDD do jeito certo | Parte 09](https://www.youtube.com/watch?v=FeqpZ9w-CRA)
	> [<img src="https://img.youtube.com/vi/FeqpZ9w-CRA/0.jpg" width="200">](https://www.youtube.com/watch?v=FeqpZ9w-CRA "Repositórios | DDD do jeito certo | Parte 09 by EximiaCo - Excelência Tecnológica  14,844 views 17 minutes")
 * [Specifications | DDD do jeito certo | Parte 08](https://www.youtube.com/watch?v=dCdOG_RZpxU)
	> [<img src="https://img.youtube.com/vi/dCdOG_RZpxU/0.jpg" width="200">](https://www.youtube.com/watch?v=dCdOG_RZpxU "Specifications | DDD do jeito certo | Parte 08 by EximiaCo - Excelência Tecnológica  10,347 views 9 minutes, 43 seconds")
 * [Agregados | DDD do jeito certo | Parte 06](https://www.youtube.com/watch?v=1AEOcQWQR2o)
	> [<img src="https://img.youtube.com/vi/1AEOcQWQR2o/0.jpg" width="200">](https://www.youtube.com/watch?v=1AEOcQWQR2o "Agregados | DDD do jeito certo | Parte 06 by EximiaCo - Excelência Tecnológica  18,000 views 17 minutes")
 * [Entidades e Objetos de Valor | DDD do jeito certo | Parte 05](https://www.youtube.com/watch?v=6Pjod34OCnE)
	> [<img src="https://img.youtube.com/vi/6Pjod34OCnE/0.jpg" width="200">](https://www.youtube.com/watch?v=6Pjod34OCnE "Entidades e Objetos de Valor | DDD do jeito certo | Parte 05 by EximiaCo - Excelência Tecnológica  16,526 views 14 minutes, 56 seconds")
 * [Subdomínios e contextos delimitados | DDD do jeito certo | Parte 03](https://www.youtube.com/watch?v=9hlRHZ4Pfyo)
	> [<img src="https://img.youtube.com/vi/9hlRHZ4Pfyo/0.jpg" width="200">](https://www.youtube.com/watch?v=9hlRHZ4Pfyo "Subdomínios e contextos delimitados | DDD do jeito certo | Parte 03 by EximiaCo - Excelência Tecnológica  23,928 views 18 minutes")
 * [Linguagem Onipresente (Ubiquitous Language) | DDD do jeito certo | Parte 02](https://www.youtube.com/watch?v=HnvmpyUAITs)
	> [<img src="https://img.youtube.com/vi/HnvmpyUAITs/0.jpg" width="200">](https://www.youtube.com/watch?v=HnvmpyUAITs "Linguagem Onipresente (Ubiquitous Language) | DDD do jeito certo | Parte 02 by EximiaCo - Excelência Tecnológica  22,859 views 17 minutes")
 * [Atacando a complexidade | DDD do jeito certo | Parte 01](https://www.youtube.com/watch?v=2X9Q97u4tUg)
	> [<img src="https://img.youtube.com/vi/2X9Q97u4tUg/0.jpg" width="200">](https://www.youtube.com/watch?v=2X9Q97u4tUg "Atacando a complexidade | DDD do jeito certo | Parte 01 by EximiaCo - Excelência Tecnológica  50,026 views 7 minutes, 55 seconds")
* [Battle Of The Software Architectures: Which One Reigns Supreme?](https://www.youtube.com/watch?v=VbuJaH7mKIc)
	> [<img src="https://img.youtube.com/vi/VbuJaH7mKIc/0.jpg" width="200">](https://www.youtube.com/watch?v=VbuJaH7mKIc "Battle Of The Software Architectures: Which One Reigns Supreme? by CodeOpinion 13,195 views 8 minutes, 6 seconds")
* [Explorando Domain Driven Design (DDD) ft. Alvaro Valle?](https://www.youtube.com/watch?v=JNCokDBQeF0&list=PLUn2w8hGMK1_Ases16h1ya1qDiFHmqbW_)
	> [<img src="https://img.youtube.com/vi/JNCokDBQeF0/0.jpg" width="200">](https://www.youtube.com/watch?v=JNCokDBQeF0&list=PLUn2w8hGMK1_Ases16h1ya1qDiFHmqbW_ "O Domain Driven Design (DDD) permite aproximar as áreas de tecnologia de negócio colaborando também para delimitar a separação de responsabilidade dos contextos de negócio. by Ramon Durães 17,000 views 47 minutes, 46 seconds")
* [EVENT MODELING: IMPLEMENTANDO DDD COM SPRING BOOT E AXON](https://www.youtube.com/watch?v=PkWfi_Y3adE)
	> [<img src="https://img.youtube.com/vi/PkWfi_Y3adE/0.jpg" width="200">](https://www.youtube.com/watch?v=PkWfi_Y3adE "Este vídeo mostra como implementar um domínio de negócio que foi elaborado utilizando Event Modeling. Vamos utilizar Kotlin, Spring Boot e Axon. by Lucas Gertel 4,000 views 2 hours, 7 minutes, 27 seconds")

## References

1. https://refactoring.guru/design-patterns/template-method
2. http://eduardodotnet.blogspot.com/2009/04/ddd-dto-vs-entity.html
3. https://docs.abp.io/pt-BR/abp/4.1/Domain-Driven-Design-Implementation-Guide
4. http://eduardodotnet.blogspot.com/2009/05/vo-value-object-vs-dto-data-transfer.html
5. https://stackoverflow.com/questions/31438286/ddd-which-layer-dto-should-be-implemented
6. https://khalilstemmler.com/articles/categories/domain-driven-design/
7. https://www.udemy.com/course/java-certification-ii-ocp-practice-for-1z0-809-exam-m/
8. https://speakerdeck.com/camilacampos/qcon-2019-event-driven-architecture-na-creditas
9. https://speakerdeck.com/camilacampos/2022-como-entender-o-seu-dominio-por-meio-de-historias-27354c56-98a8-473b-b6f6-1cb5521bd0c6?slide=63
10. https://martinfowler.com/articles/microservices.html#SmartEndpointsAndDumbPipes
11. https://vaadin.com/blog/ddd-part-3-domain-driven-design-and-the-hexagonal-architecture
12. https://ppbruna.medium.com/quero-aplicar-ddd-por-onde-come%C3%A7o-73c9390ca678
13. https://www.avanscoperta.it/en/training/domain-models-in-practice-workshop-ddd-cqrs-event-sourcing/
14. https://github.com/ddd-crew/aggregate-design-canvas
15. https://github.com/ddd-crew/bounded-context-canvas
16. https://github.com/ddd-crew/context-mapping
17. https://codeopinion.com/clean-up-your-domain-model-with-event-sourcing/
18. https://codeopinion.com/aggregate-root-design-behavior-data/
19. https://codeopinion.com/avoid-entity-services-by-focusing-on-capabilities/
20. https://codeopinion.com/decomposing-crud-to-a-task-based-ui/
21. https://www.baeldung.com/spring-data-repositories
22. https://dev.to/kirekov/rich-domain-model-with-hibernate-445k
23. https://www.baeldung.com/java-anemic-vs-rich-domain-objects
24. 
