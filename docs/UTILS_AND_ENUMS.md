# Hospital Management System - Utilities and Enums Analysis

## Utility Classes

### IdGenerator.java
```java
public class IdGenerator implements Id {
    private static IdGenerator instance;
    private Map<UserRole, Integer> counters;
}
```

**Purpose:** Generates unique IDs for system entities
**Key Features:**
- Singleton pattern
- Thread-safe operation
- Role-based prefixes

**Complete Analysis:**
1. **Fields:**
   ```java
   private static IdGenerator instance;
   private Map<UserRole, Integer> counters;
   ```
   - Singleton instance
   - Counter tracking per role

2. **Constructor:**
   ```java
   private IdGenerator() {
       counters = new EnumMap<>(UserRole.class);
       for (UserRole role : UserRole.values()) {
           counters.put(role, 0);
       }
   }
   ```
   - Private constructor (singleton)
   - Initializes counters for each role

3. **Core Methods:**
   ```java
   // Thread-safe ID generation
   public synchronized String generateId(UserRole role) {
       int counter = counters.get(role);
       counter++;
       counters.put(role, counter);
       return String.format("%s%04d", role.getPrefix(), counter);
   }

   // Singleton access
   public static synchronized IdGenerator getInstance() {
       if (instance == null) {
           instance = new IdGenerator();
       }
       return instance;
   }
   ```
   - Thread-safe singleton access
   - Synchronized ID generation
   - Role-specific formatting

## Enum Classes

### UserRole.java
```java
public enum UserRole {
    DOCTOR("DOC"),
    NURSE("NUR"),
    RECEPTIONIST("REC"),
    ADMIN("ADM");

    private final String prefix;

    UserRole(String prefix) {
        this.prefix = prefix;
    }

    public String getPrefix() {
        return prefix;
    }
}
```

**Purpose:** Defines available user roles
**Key Features:**
- Role identification
- ID prefix generation
- Type safety

**Complete Analysis:**
1. **Enum Constants:**
   - `DOCTOR`: Medical doctors
   - `NURSE`: Nursing staff
   - `RECEPTIONIST`: Front desk staff
   - `ADMIN`: System administrators

2. **Fields:**
   ```java
   private final String prefix;
   ```
   - Role-specific ID prefix
   - Immutable value

3. **Usage:**
   ```java
   // Role assignment
   public Doctor(String firstName, ...) {
       super(firstName, ..., UserRole.DOCTOR);
   }

   // Permission checking
   if (user.getRole() == UserRole.ADMIN) {
       // Admin-specific operations
   }
   ```

### Permission.java
```java
public enum Permission {
    // Patient Management
    VIEW_PATIENTS,
    ADD_PATIENT,
    EDIT_PATIENT,
    DELETE_PATIENT,

    // Appointment Management
    VIEW_APPOINTMENTS,
    SCHEDULE_APPOINTMENT,
    CANCEL_APPOINTMENT,
    MODIFY_APPOINTMENT,

    // Medical Records
    VIEW_MEDICAL_RECORDS,
    CREATE_MEDICAL_RECORD,
    EDIT_MEDICAL_RECORD,
    DELETE_MEDICAL_RECORD,

    // Staff Management
    VIEW_STAFF,
    ADD_STAFF,
    EDIT_STAFF,
    DELETE_STAFF,

    // System Administration
    SYSTEM_ADMIN,
    VIEW_LOGS,
    MANAGE_ROLES
}
```

**Purpose:** Defines system permissions
**Key Features:**
- Granular access control
- Operation categorization
- Clear permission naming

**Complete Analysis:**
1. **Permission Categories:**
   - Patient Management
   - Appointment Management
   - Medical Records
   - Staff Management
   - System Administration

2. **Usage:**
   ```java
   // Permission checking
   public void checkPermission(Person user, Permission permission) {
       if (!hasPermission(user, permission)) {
           throw new SecurityException("Access denied");
       }
   }

   // Role-permission mapping
   private void initializePermissions() {
       rolePermissions.put(UserRole.ADMIN, EnumSet.allOf(Permission.class));
       rolePermissions.put(UserRole.DOCTOR, EnumSet.of(
           Permission.VIEW_PATIENTS,
           Permission.VIEW_MEDICAL_RECORDS,
           Permission.CREATE_MEDICAL_RECORD
       ));
       // etc...
   }
   ```

### AppointmentStatus.java
```java
public enum AppointmentStatus {
    SCHEDULED,
    COMPLETED,
    CANCELLED,
    NO_SHOW
}
```

**Purpose:** Defines appointment states
**Key Features:**
- Status tracking
- State management
- Clear status values

**Complete Analysis:**
1. **Status Values:**
   - `SCHEDULED`: Future appointment
   - `COMPLETED`: Finished appointment
   - `CANCELLED`: Cancelled by doctor/patient
   - `NO_SHOW`: Patient didn't attend

2. **Usage:**
   ```java
   // Status checking
   if (appointment.getStatus() == AppointmentStatus.SCHEDULED) {
       // Process scheduled appointment
   }

   // Status update
   appointment.setStatus(AppointmentStatus.COMPLETED);
   ```

## Implementation Notes

1. **Thread Safety:**
   - Synchronized singleton access
   - Thread-safe ID generation
   - Immutable enum values

2. **Type Safety:**
   - Enum-based role definition
   - Permission categorization
   - Status management

3. **Extensibility:**
   - Easy to add new roles
   - Simple permission addition
   - Status expansion support 