# Object-Oriented Programming Implementation

## 1. Abstraction

### Definition
Hiding complex implementation details and showing only necessary features of an object.

### Implementation in HMS
```java
// Abstract base class - cannot be instantiated directly
public abstract class Person {
    private final String id;
    private String firstName;
    private String lastName;
    private String email;
    private String phoneNumber;
    private final UserRole role;

    // Protected constructor - only subclasses can use
    protected Person(String firstName, String lastName, String email, 
                    String phoneNumber, UserRole role) {
        this.id = IdGenerator.getInstance().generateId(role);
        this.firstName = firstName;
        this.lastName = lastName;
        this.email = email;
        this.phoneNumber = phoneNumber;
        this.role = role;
    }
}
```

### Where It's Used
1. `Person` class is abstract:
   - Cannot create generic "Person" objects
   - Forces creation of specific types (Doctor, Nurse, etc.)
   - Hides ID generation complexity

2. Service interfaces:
```java
public interface Id {
    String generateId(UserRole role);
}
```

## 2. Encapsulation

### Definition
Bundling data and methods that operate on that data within a single unit, restricting direct access to object's components.

### Implementation in HMS
```java
public class Doctor extends Person {
    // Private fields - data hiding
    private String specialization;
    private List<String> qualifications;
    private boolean isAvailable;
    private String schedule;

    // Public methods - controlled access
    public String getSpecialization() { 
        return specialization; 
    }

    public void setSpecialization(String specialization) { 
        this.specialization = specialization; 
    }

    // Defensive copying for collections
    public List<String> getQualifications() { 
        return new ArrayList<>(qualifications); 
    }

    public void setQualifications(List<String> qualifications) { 
        this.qualifications = new ArrayList<>(qualifications); 
    }
}
```

### Where It's Used
1. All entity classes use private fields
2. Access through public methods only
3. Immutable fields where needed
4. Defensive copying for collections

## 3. Inheritance

### Definition
Mechanism that allows a class to inherit properties and methods from another class.

### Implementation in HMS
```java
// Base class
public abstract class Person { ... }

// Doctor inherits from Person
public class Doctor extends Person {
    public Doctor(String firstName, String lastName, String email, 
                 String phoneNumber, String specialization, 
                 List<String> qualifications) {
        // Call parent constructor
        super(firstName, lastName, email, phoneNumber, UserRole.DOCTOR);
        this.specialization = specialization;
        this.qualifications = new ArrayList<>(qualifications);
    }
}

// Nurse inherits from Person
public class Nurse extends Person {
    public Nurse(String firstName, String lastName, String email, 
                String phoneNumber, String department, 
                List<String> certifications) {
        super(firstName, lastName, email, phoneNumber, UserRole.NURSE);
        this.department = department;
        this.certifications = new ArrayList<>(certifications);
    }
}
```

### Class Hierarchy
```
Person (abstract)
├── Doctor
│   └── Properties: specialization, qualifications
├── Nurse
│   └── Properties: department, certifications
├── Receptionist
│   └── Properties: desk, shift
└── Admin
    └── Properties: (none)
```

## 4. Polymorphism

### Definition
Ability of objects to take on multiple forms, specifically the ability to use a base class reference to handle derived class objects.

### Implementation in HMS

#### 1. Method Overriding
```java
// Base class method
public class Person {
    @Override
    public String toString() {
        return String.format("ID: %s, Name: %s %s, Role: %s",
            id, firstName, lastName, role);
    }
}

// Derived class override
public class Doctor extends Person {
    @Override
    public String toString() {
        return String.format("%s, Specialization: %s, Available: %s",
            super.toString(), specialization, isAvailable);
    }
}
```

#### 2. Interface Implementation
```java
// Interface definition
public interface Id {
    String generateId(UserRole role);
}

// Implementation
public class IdGenerator implements Id {
    @Override
    public String generateId(UserRole role) {
        // Implementation details
    }
}
```

#### 3. Polymorphic Collections
```java
// In AuthenticationService
private Map<String, Person> loggedInUsers;  // Can store any Person subtype

// In Main
private static final List<Doctor> doctors = new ArrayList<>();
private static final List<Nurse> nurses = new ArrayList<>();
```

### Real-World Usage Examples

1. **User Authentication**
```java
public Person login(String email, String password) {
    Person user = findUser(email);  // Could be any Person subtype
    if (validateCredentials(user, password)) {
        return user;
    }
    throw new SecurityException("Invalid credentials");
}
```

2. **Staff Management**
```java
public void assignToShift(Person staff, String shift) {
    if (staff instanceof Nurse) {
        ((Nurse) staff).setShift(shift);
    } else if (staff instanceof Doctor) {
        ((Doctor) staff).setSchedule(shift);
    }
}
```

## Benefits of OOP in HMS

1. **Code Organization**
   - Clear structure
   - Logical grouping
   - Easy navigation
   - Maintainable codebase

2. **Code Reuse**
   - Common functionality in base class
   - Shared methods
   - Consistent behavior
   - Reduced duplication

3. **Security**
   - Data protection
   - Access control
   - Validation
   - Error handling

4. **Extensibility**
   - Easy to add new staff types
   - Simple to extend functionality
   - Flexible architecture
   - Modular design 