# Clinic Booking System

A relational database schema for managing clinical operations such as departments, doctors, patients, appointments, services, and prescriptions.

## Features

- Department and doctor management
- Patient registration and records
- Appointment scheduling and status tracking
- Service offerings by doctors
- Prescription and medication tracking

## Database Schema

### 1. Departments

```sql
CREATE TABLE departments (
    department_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL UNIQUE
);
```

### 2. Doctors

```sql
CREATE TABLE doctors (
    doctor_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    phone VARCHAR(20) NOT NULL,
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES departments(department_id)
);
```

### 3. Patients

```sql
CREATE TABLE patients (
    patient_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    date_of_birth DATE NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    phone VARCHAR(20) NOT NULL
);
```

### 4. Appointments

```sql
CREATE TABLE appointments (
    appointment_id INT AUTO_INCREMENT PRIMARY KEY,
    patient_id INT NOT NULL,
    doctor_id INT NOT NULL,
    appointment_date DATETIME NOT NULL,
    reason TEXT,
    status ENUM('Scheduled', 'Completed', 'Cancelled') DEFAULT 'Scheduled',
    FOREIGN KEY (patient_id) REFERENCES patients(patient_id),
    FOREIGN KEY (doctor_id) REFERENCES doctors(doctor_id)
);
```

### 5. Services

```sql
CREATE TABLE services (
    service_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL UNIQUE,
    cost DECIMAL(10,2) NOT NULL
);
```

### 6. Doctor Services (Many-to-Many)

```sql
CREATE TABLE doctor_services (
    doctor_id INT NOT NULL,
    service_id INT NOT NULL,
    PRIMARY KEY (doctor_id, service_id),
    FOREIGN KEY (doctor_id) REFERENCES doctors(doctor_id),
    FOREIGN KEY (service_id) REFERENCES services(service_id)
);
```

### 7. Medications

```sql
CREATE TABLE medications (
    medication_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    dosage VARCHAR(50) NOT NULL
);
```

### 8. Prescriptions

```sql
CREATE TABLE prescriptions (
    prescription_id INT AUTO_INCREMENT PRIMARY KEY,
    appointment_id INT NOT NULL UNIQUE,
    medication_id INT NOT NULL,
    instructions TEXT,
    FOREIGN KEY (appointment_id) REFERENCES appointments(appointment_id),
    FOREIGN KEY (medication_id) REFERENCES medications(medication_id)
);
```

## Getting Started

1. Import the schema into a MySQL-compatible database.
2. Populate data for departments, doctors, patients, and services.
3. Use this structure as a backend for a clinic management or booking system.

## Future Enhancements

- User authentication for doctors/patients
- Billing and invoice module
- Reminders and notifications
- Recurring appointment support
