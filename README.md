https://www.programiz.com/online-compiler/4UmQMLWOTZIww
import java.util.*;

class Person {
    String name;
    int age;
    String gender;

    Person(String name, int age, String gender) {
        this.name = name;
        this.age = age;
        this.gender = gender;
    }
}

class Doctor extends Person {
    String specialization;

    Doctor(String name, int age, String gender, String specialization) {
        super(name, age, gender);
        this.specialization = specialization;
    }

    public String toString() {
        return "Dr. " + name + " | Age: " + age + " | Gender: " + gender + " | Specialization: " + specialization;
    }
}

class Patient extends Person {
    int id;
    static int idCounter = 1;

    Patient(String name, int age, String gender) {
        super(name, age, gender);
        this.id = idCounter++;
    }

    public String toString() {
        return "Patient ID: " + id + " | Name: " + name + " | Age: " + age + " | Gender: " + gender;
    }
}

class Appointment {
    Patient patient;
    Doctor doctor;
    String date;

    Appointment(Patient patient, Doctor doctor, String date) {
        this.patient = patient;
        this.doctor = doctor;
        this.date = date;
    }

    public String toString() {
        return patient.name + " has an appointment with " + doctor.name + " on " + date;
    }
}

public class HospitalManagementSystem {
    static List<Doctor> doctors = new ArrayList<>();
    static List<Patient> patients = new ArrayList<>();
    static List<Appointment> appointments = new ArrayList<>();

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int choice;

        while (true) {
            System.out.println("\n=== Hospital Management System ===");
            System.out.println("1. Add Doctor");
            System.out.println("2. Add Patient");
            System.out.println("3. View Doctors");
            System.out.println("4. View Patients");
            System.out.println("5. Book Appointment");
            System.out.println("6. View Appointments");
            System.out.println("0. Exit");
            System.out.print("Enter your choice: ");
            choice = sc.nextInt();
            sc.nextLine();  // consume newline

            switch (choice) {
                case 1 -> addDoctor(sc);
                case 2 -> addPatient(sc);
                case 3 -> viewDoctors();
                case 4 -> viewPatients();
                case 5 -> bookAppointment(sc);
                case 6 -> viewAppointments();
                case 0 -> {
                    System.out.println("Exiting...");
                    return;
                }
                default -> System.out.println("Invalid choice. Try again.");
            }
        }
    }

    static void addDoctor(Scanner sc) {
        System.out.print("Enter name: ");
        String name = sc.nextLine();
        System.out.print("Enter age: ");
        int age = sc.nextInt();
        sc.nextLine(); // consume newline
        System.out.print("Enter gender: ");
        String gender = sc.nextLine();
        System.out.print("Enter specialization: ");
        String spec = sc.nextLine();

        doctors.add(new Doctor(name, age, gender, spec));
        System.out.println("Doctor added successfully.");
    }

    static void addPatient(Scanner sc) {
        System.out.print("Enter name: ");
        String name = sc.nextLine();
        System.out.print("Enter age: ");
        int age = sc.nextInt();
        sc.nextLine(); // consume newline
        System.out.print("Enter gender: ");
        String gender = sc.nextLine();

        patients.add(new Patient(name, age, gender));
        System.out.println("Patient added successfully.");
    }

    static void viewDoctors() {
        System.out.println("\n-- List of Doctors --");
        for (Doctor d : doctors) {
            System.out.println(d);
        }
    }

    static void viewPatients() {
        System.out.println("\n-- List of Patients --");
        for (Patient p : patients) {
            System.out.println(p);
        }
    }

    static void bookAppointment(Scanner sc) {
        if (patients.isEmpty() || doctors.isEmpty()) {
            System.out.println("Cannot book appointment. Add both doctors and patients first.");
            return;
        }

        viewPatients();
        System.out.print("Enter Patient ID to book appointment: ");
        int pid = sc.nextInt();
        sc.nextLine();

        Patient selectedPatient = null;
        for (Patient p : patients) {
            if (p.id == pid) {
                selectedPatient = p;
                break;
            }
        }

        if (selectedPatient == null) {
            System.out.println("Invalid patient ID.");
            return;
        }

        viewDoctors();
        System.out.print("Enter Doctor number (0-based index): ");
        int did = sc.nextInt();
        sc.nextLine();

        if (did < 0 || did >= doctors.size()) {
            System.out.println("Invalid doctor index.");
            return;
        }

        System.out.print("Enter appointment date (e.g. 2025-04-20): ");
        String date = sc.nextLine();

        appointments.add(new Appointment(selectedPatient, doctors.get(did), date));
        System.out.println("Appointment booked successfully.");
    }

    static void viewAppointments() {
        System.out.println("\n-- Appointments --");
        for (Appointment a : appointments) {
            System.out.println(a);
        }
    }
}
