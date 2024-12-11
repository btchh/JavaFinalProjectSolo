# Hospital Management System - Service Classes Analysis

## Authentication Services

### AuthenticationService.java
```java
public class AuthenticationService {
    private static AuthenticationService instance;
    private Map<String, String> userCredentials;
    private Map<String, Person> loggedInUsers;
}
```

**Purpose:** Manages user authentication and sessions
**Key Features:**
- Singleton pattern
- Credential management
- Session tracking

**Complete Analysis:**
1. **Fields:**
   ```java
   private static AuthenticationService instance;
   private Map<String, String> userCredentials;  // email -> password
   private Map<String, Person> loggedInUsers;    // email -> user
   ```
   - Singleton instance
   - Credential storage
   - Active session tracking

2. **Constructor:**
   ```java
   private AuthenticationService() {
       userCredentials = new HashMap<>();
       loggedInUsers = new HashMap<>();
   }
   ```
   - Private constructor (singleton)
   - Initializes storage maps

3. **Core Methods:**
   ```java
   // User registration
   public Person registerNewUser(Person admin, String email, String password,
                               String firstName, String lastName, String phone,
                               UserRole role, String specialization) {
       // Verify admin permissions
       rbacService.checkPermission(admin, Permission.ADD_STAFF);
       
       // Create user based on role
       Person newUser;
       switch (role) {
           case DOCTOR:
               newUser = new Doctor(...);
               break;
           case NURSE:
               newUser = new Nurse(...);
               break;
           // etc...
       }
       
       // Store credentials
       userCredentials.put(email, password);
       return newUser;
   }

   // User authentication
   public Person login(String email, String password) {
       String storedPassword = userCredentials.get(email);
       if (storedPassword != null && storedPassword.equals(password)) {
           Person user = findUserByEmail(email);
           loggedInUsers.put(email, user);
           return user;
       }
       throw new SecurityException("Invalid credentials");
   }

   // Session management
   public void logout(String email) {
       loggedInUsers.remove(email);
   }

   public boolean isLoggedIn(String email) {
       return loggedInUsers.containsKey(email);
   }
   ```

## Medical Services

### AppointmentService.java
```java
public class AppointmentService {
    private static AppointmentService instance;
    private List<Appointment> appointments;
}
```

**Purpose:** Manages medical appointments
**Key Features:**
- Appointment scheduling
- Conflict resolution
- Availability checking

**Complete Analysis:**
1. **Fields:**
   ```java
   private static AppointmentService instance;
   private List<Appointment> appointments;
   ```
   - Singleton instance
   - Appointment storage

2. **Core Methods:**
   ```java
   // Schedule appointment
   public Appointment scheduleAppointment(Doctor doctor, Patient patient,
                                        LocalDateTime dateTime) {
       // Check doctor availability
       if (!isDoctorAvailable(doctor, dateTime)) {
           throw new IllegalStateException("Doctor not available");
       }
       
       // Create and store appointment
       Appointment appointment = new Appointment(doctor, patient, dateTime);
       appointments.add(appointment);
       return appointment;
   }

   // Availability check
   private boolean isDoctorAvailable(Doctor doctor, LocalDateTime dateTime) {
       return appointments.stream()
           .filter(a -> a.getDoctor().equals(doctor))
           .filter(a -> a.getStatus() != AppointmentStatus.CANCELLED)
           .noneMatch(a -> a.getDateTime().equals(dateTime));
   }

   // View appointments
   public List<Appointment> getDoctorAppointments(Doctor doctor) {
       return appointments.stream()
           .filter(a -> a.getDoctor().equals(doctor))
           .collect(Collectors.toList());
   }

   public List<Appointment> getPatientAppointments(Patient patient) {
       return appointments.stream()
           .filter(a -> a.getPatient().equals(patient))
           .collect(Collectors.toList());
   }
   ```

[Continue with analysis of other service classes...] 