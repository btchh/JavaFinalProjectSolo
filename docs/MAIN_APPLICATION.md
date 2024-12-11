# Hospital Management System - Main Application Analysis

## Main.java

```java
public class Main {
    private static final Scanner scanner = new Scanner(System.in);
    private static final List<Doctor> doctors = new ArrayList<>();
    private static final List<Nurse> nurses = new ArrayList<>();
    private static final List<Patient> patients = new ArrayList<>();
    private static final AppointmentService appointmentService = AppointmentService.getInstance();
    private static final MedicalRecordService medicalRecordService = MedicalRecordService.getInstance();
    private static final PrescriptionService prescriptionService = PrescriptionService.getInstance();
    private static final AuthenticationService authService = AuthenticationService.getInstance();
    private static final RBACService rbacService = RBACService.getInstance();
    private static final DateTimeFormatter dateFormatter = DateTimeFormatter.ofPattern("dd/MM/yyyy");
    private static Person currentUser = null;
}
```

**Purpose:** Application entry point and control flow
**Key Features:**
- User interface management
- System initialization
- Service coordination
- Session handling

## Complete Analysis

### 1. Static Fields
```java
// Data Storage
private static final List<Doctor> doctors = new ArrayList<>();
private static final List<Nurse> nurses = new ArrayList<>();
private static final List<Patient> patients = new ArrayList<>();

// Services
private static final AppointmentService appointmentService = AppointmentService.getInstance();
private static final MedicalRecordService medicalRecordService = MedicalRecordService.getInstance();
private static final PrescriptionService prescriptionService = PrescriptionService.getInstance();
private static final AuthenticationService authService = AuthenticationService.getInstance();
private static final RBACService rbacService = RBACService.getInstance();

// Utilities
private static final Scanner scanner = new Scanner(System.in);
private static final DateTimeFormatter dateFormatter = DateTimeFormatter.ofPattern("dd/MM/yyyy");

// Session
private static Person currentUser = null;
```

**Analysis:**
1. Data Storage:
   - In-memory lists for entities
   - Final collections for thread safety
   - Separate lists by entity type

2. Services:
   - All services are singletons
   - Final references for immutability
   - Complete system functionality coverage

3. Utilities:
   - Scanner for user input
   - Date formatter for consistency
   - Session tracking

### 2. Main Method
```java
public static void main(String[] args) {
    initializeSampleData();
    
    while (true) {
        if (currentUser == null) {
            displayLoginMenu();
        } else {
            displayMainMenu();
        }
        
        int choice = getIntInput("Enter your choice: ");
        processMenuChoice(choice);
    }
}
```

**Analysis:**
1. Initialization:
   - Sample data loading
   - System preparation

2. Main Loop:
   - Continuous operation
   - Session-based menu display
   - User input processing

### 3. Menu System

#### Login Menu
```java
private static void displayLoginMenu() {
    System.out.println("\n=== Hospital Management System - Login ===");
    System.out.println("1. Login");
    System.out.println("0. Exit");
}
```

#### Main Menu
```java
private static void displayMainMenu() {
    System.out.println("\n=== Hospital Management System ===");
    System.out.println("1. Add New Staff");
    System.out.println("2. Register New Patient");
    System.out.println("3. Schedule Appointment");
    System.out.println("4. View Appointments");
    System.out.println("5. Create Medical Record");
    System.out.println("6. Create Prescription");
    System.out.println("7. View All Records");
    System.out.println("0. Exit");
}
```

### 4. Menu Processing
```java
private static void processMenuChoice(int choice) {
    try {
        if (currentUser == null) {
            processLoginMenu(choice);
            return;
        }

        switch (choice) {
            case 1:
                if (rbacService.hasPermission(currentUser, Permission.ADD_STAFF)) {
                    addNewStaff();
                } else {
                    System.out.println("Access denied");
                }
                break;
            case 2:
                registerNewPatient();
                break;
            // ... more cases
        }
    } catch (SecurityException e) {
        System.out.println("Access denied: " + e.getMessage());
    } catch (Exception e) {
        System.out.println("Error: " + e.getMessage());
    }
}
```

### 5. Core Operations

#### Staff Management
```java
private static void addNewStaff() {
    String firstName = getStringInput("Enter first name: ");
    String lastName = getStringInput("Enter last name: ");
    String email = getStringInput("Enter email: ");
    String phone = getStringInput("Enter phone number: ");
    String password = getStringInput("Enter password: ");
    
    System.out.println("\nSelect role:");
    System.out.println("1. Doctor");
    System.out.println("2. Nurse");
    System.out.println("3. Receptionist");
    System.out.println("4. Admin");
    
    int roleChoice = getIntInput("Enter choice: ");
    // Process role selection and create staff
}
```

#### Patient Management
```java
private static void registerNewPatient() {
    String firstName = getStringInput("Enter first name: ");
    String lastName = getStringInput("Enter last name: ");
    String email = getStringInput("Enter email: ");
    String phone = getStringInput("Enter phone number: ");
    String dateStr = getStringInput("Enter date of birth (dd/MM/yyyy): ");
    String bloodType = getStringInput("Enter blood type: ");

    LocalDate dob = LocalDate.parse(dateStr, dateFormatter);
    Patient patient = new Patient(firstName, lastName, email, phone, dob, bloodType);
    
    // Add medical history and allergies
    while (true) {
        String history = getStringInput("Enter medical history item (or 'done'): ");
        if (history.equalsIgnoreCase("done")) break;
        patient.addMedicalHistory(history);
    }
}
```

### 6. Utility Methods

#### Input Handling
```java
private static String getStringInput(String prompt) {
    System.out.print(prompt);
    return scanner.nextLine().trim();
}

private static int getIntInput(String prompt) {
    while (true) {
        try {
            System.out.print(prompt);
            return Integer.parseInt(scanner.nextLine().trim());
        } catch (NumberFormatException e) {
            System.out.println("Please enter a valid number");
        }
    }
}
```

### 7. Error Handling
```java
try {
    // Operation execution
} catch (SecurityException e) {
    System.out.println("Access denied: " + e.getMessage());
} catch (IllegalArgumentException e) {
    System.out.println("Invalid input: " + e.getMessage());
} catch (Exception e) {
    System.out.println("Error: " + e.getMessage());
}
```

## Implementation Notes

1. **User Interface:**
   - Console-based interaction
   - Clear menu structure
   - Input validation
   - Error feedback

2. **Security:**
   - Session management
   - Permission checking
   - Access control
   - Secure operations

3. **Data Management:**
   - In-memory storage
   - Service coordination
   - Entity relationships
   - Data validation

4. **Error Handling:**
   - Exception catching
   - User feedback
   - Graceful recovery
   - Input validation 