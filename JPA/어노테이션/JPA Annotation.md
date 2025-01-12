Here's a summary of key JPA (Java Persistence API) annotations

---

### **Entity Annotations**
- **`@Entity`**
  - Marks a class as a JPA entity, representing a table in the database.
  - Example:
    ```java
    @Entity
    public class User { ... }
    ```

- **`@Table`**
  - Specifies the table name in the database for the entity.
  - Example:
    ```java
    @Entity
    @Table(name = "users")
    public class User { ... }
    ```

---

### **Field Annotations**
- **`@Id`**
  - Marks a field as the primary key of the entity.
  - Example:
    ```java
    @Id
    private Long id;
    ```

- **`@GeneratedValue`**
  - Specifies how the primary key value is generated.
  - Strategies:
    - `GenerationType.IDENTITY`
    - `GenerationType.SEQUENCE`
    - `GenerationType.TABLE`
    - `GenerationType.AUTO`
  - Example:
    ```java
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    ```

- **`@Column`**
  - Maps a field to a specific column in the database.
  - Attributes:
    - `name`: Custom column name.
    - `nullable`: Whether the column can be null.
    - `length`: Maximum length for `String` types.
  - Example:
    ```java
    @Column(name = "email", nullable = false, length = 50)
    private String email;
    ```

- **`@Transient`**
  - Specifies a field that is not persistent and will not be mapped to the database.
  - Example:
    ```java
    @Transient
    private String tempData;
    ```

---

### **Relationship Annotations**
- **`@OneToOne`**
  - Defines a one-to-one relationship.
  - Example:
    ```java
    @OneToOne
    @JoinColumn(name = "profile_id")
    private Profile profile;
    ```

- **`@OneToMany`**
  - Defines a one-to-many relationship.
  - Example:
    ```java
    @OneToMany(mappedBy = "user")
    private List<Post> posts;
    ```

- **`@ManyToOne`**
  - Defines a many-to-one relationship.
  - Example:
    ```java
    @ManyToOne
    @JoinColumn(name = "user_id")
    private User user;
    ```

- **`@ManyToMany`**
  - Defines a many-to-many relationship.
  - Example:
    ```java
    @ManyToMany
    @JoinTable(
        name = "user_roles",
        joinColumns = @JoinColumn(name = "user_id"),
        inverseJoinColumns = @JoinColumn(name = "role_id")
    )
    private Set<Role> roles;
    ```

---

### **Lifecycle Annotations**
- **`@PrePersist`**
  - Method annotated with this runs before an entity is persisted.
  - Example:
    ```java
    @PrePersist
    public void prePersist() {
        this.createdAt = LocalDateTime.now();
    }
    ```

- **`@PostPersist`**
  - Method runs after an entity is persisted.
  - Example:
    ```java
    @PostPersist
    public void postPersist() {
        System.out.println("Entity persisted!");
    }
    ```

- **`@PreUpdate`**
  - Method runs before an entity is updated.
  - Example:
    ```java
    @PreUpdate
    public void preUpdate() {
        this.updatedAt = LocalDateTime.now();
    }
    ```

- **`@PostUpdate`**
  - Method runs after an entity is updated.

- **`@PreRemove`**
  - Method runs before an entity is removed.

- **`@PostRemove`**
  - Method runs after an entity is removed.

---

### **Inheritance Annotations**
- **`@Inheritance`**
  - Specifies inheritance strategy for an entity hierarchy.
  - Strategies:
    - `InheritanceType.SINGLE_TABLE`
    - `InheritanceType.JOINED`
    - `InheritanceType.TABLE_PER_CLASS`
  - Example:
    ```java
    @Entity
    @Inheritance(strategy = InheritanceType.JOINED)
    public class BaseEntity { ... }
    ```

- **`@DiscriminatorColumn`**
  - Defines the column used for distinguishing entity types in a single-table inheritance.

---

### **Query Annotations**
- **`@NamedQuery`**
  - Defines a static, named JPQL query.
  - Example:
    ```java
    @NamedQuery(name = "User.findByEmail", query = "SELECT u FROM User u WHERE u.email = :email")
    ```

- **`@NamedNativeQuery`**
  - Defines a static, named SQL query.
  - Example:
    ```java
    @NamedNativeQuery(
        name = "User.findByEmailNative",
        query = "SELECT * FROM users WHERE email = ?",
        resultClass = User.class
    )
    ```
