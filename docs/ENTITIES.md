# Hospital Management System - Entity Classes Analysis

## Base Class

### Person.java
```java
public abstract class Person {
    private final String id;
    private String firstName;
    private String lastName;
    private String email;
    private String phoneNumber;
    private final UserRole role;
}
```

**Purpose:** Abstract base class for all staff members
**Key Features:**
- Abstract class prevents direct instantiation
- Immutable ID and role fields
- Common personal information fields

**Complete Analysis:**
1. **Fields:**
   - `id`: Unique identifier, immutable
   - `firstName`, `lastName`: Personal names
   - `email`: Contact email
   - `phoneNumber`: Contact number
   - `role`: Staff role, immutable

2. **Constructor:**
   ```java
   protected Person(String firstName, String lastName, 
                   String email, String phoneNumber, UserRole role) {
       this.id = IdGenerator.getInstance().generateId(role);
       this.firstName = firstName;
       this.lastName = lastName;
       this.email = email;
       this.phoneNumber = phoneNumber;
       this.role = role;
   }
   ```
   - Protected access
   - Generates unique ID
   - Initializes all fields

3. **Methods:**
   ```java
   // Getters
   public String getId() { return id; }
   public String getFirstName() { return firstName; }
   public String getLastName() { return lastName; }
   public String getEmail() { return email; }
   public String getPhoneNumber() { return phoneNumber; }
   public UserRole getRole() { return role; }

   // Setters (except for id and role which are final)
   public void setFirstName(String firstName) { this.firstName = firstName; }
   public void setLastName(String lastName) { this.lastName = lastName; }
   public void setEmail(String email) { this.email = email; }
   public void setPhoneNumber(String phoneNumber) { this.phoneNumber = phoneNumber; }
   ```
   - Getters for all fields
   - Setters for mutable fields only
   - No setters for id and role (immutable)

4. **toString Method:**
   ```java
   @Override
   public String toString() {
       return String.format("ID: %s, Name: %s %s, Role: %s",
           id, firstName, lastName, role);
   }
   ```
   - Formatted string representation
   - Includes key identifying information

## Medical Staff

### Doctor.java
```java
public class Doctor extends Person {
    private String specialization;
    private List<String> qualifications;
    private boolean isAvailable;
    private String schedule;
}
```

**Purpose:** Represents medical doctors
**Key Features:**
- Medical specialization
- Professional qualifications
- Availability tracking
- Schedule management

**Complete Analysis:**
1. **Fields:**
   - `specialization`: Medical specialty area
   - `qualifications`: List of professional qualifications
   - `isAvailable`: Current availability status
   - `schedule`: Working hours

2. **Constructor:**
   ```java
   public Doctor(String firstName, String lastName, String email,
                String phoneNumber, String specialization,
                List<String> qualifications) {
       super(firstName, lastName, email, phoneNumber, UserRole.DOCTOR);
       this.specialization = specialization;
       this.qualifications = new ArrayList<>(qualifications);
       this.isAvailable = true;
       this.schedule = "9AM-5PM";
   }
   ```
   - Calls parent constructor
   - Defensive copying for qualifications
   - Default availability and schedule

3. **Methods:**
   ```java
   // Qualification management
   public void addQualification(String qualification) {
       this.qualifications.add(qualification);
   }

   public void removeQualification(String qualification) {
       this.qualifications.remove(qualification);
   }

   // Getters with defensive copying
   public List<String> getQualifications() {
       return new ArrayList<>(qualifications);
   }
   ```
   - Safe collection handling
   - Qualification management
   - Availability control

[Continue with analysis of other entity classes...] 