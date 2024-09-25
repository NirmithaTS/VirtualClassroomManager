# VirtualClassroomManager
import java.util.*;

// Classroom entity to handle student and assignment management
class Classroom {
    private String name;
    private Set<String> students;
    private List<String> assignments;

    public Classroom(String name) {
        this.name = name;
        this.students = new HashSet<>();
        this.assignments = new ArrayList<>();
    }

    public String getName() {
        return name;
    }

    public void addStudent(String studentId) {
        if (students.contains(studentId)) {
            System.out.println("Student " + studentId + " is already enrolled in " + name + ".");
        } else {
            students.add(studentId);
            System.out.println("Student " + studentId + " has been enrolled in " + name + ".");
        }
    }

    public void scheduleAssignment(String assignmentDetails) {
        assignments.add(assignmentDetails);
        System.out.println("Assignment for " + name + " has been scheduled.");
    }

    public boolean hasStudent(String studentId) {
        return students.contains(studentId);
    }

    public void displayStudents() {
        if (students.isEmpty()) {
            System.out.println("No students enrolled in " + name + ".");
        } else {
            System.out.println("Students in " + name + ": " + students);
        }
    }

    public void displayAssignments() {
        if (assignments.isEmpty()) {
            System.out.println("No assignments scheduled for " + name + ".");
        } else {
            System.out.println("Assignments for " + name + ": " + assignments);
        }
    }
}

// Manager for Classroom management (Adding, removing, fetching classrooms)
class ClassroomManager {
    private Map<String, Classroom> classrooms;

    public ClassroomManager() {
        this.classrooms = new HashMap<>();
    }

    public void addClassroom(String name) {
        if (classrooms.containsKey(name)) {
            System.out.println("Classroom " + name + " already exists.");
        } else {
            classrooms.put(name, new Classroom(name));
            System.out.println("Classroom " + name + " has been created.");
        }
    }

    public Classroom getClassroom(String name) {
        if (!classrooms.containsKey(name)) {
            System.out.println("Classroom " + name + " does not exist.");
            return null;
        }
        return classrooms.get(name);
    }

    public void deleteClassroom(String name) {
        if (classrooms.remove(name) != null) {
            System.out.println("Classroom " + name + " has been deleted.");
        } else {
            System.out.println("Classroom " + name + " does not exist.");
        }
    }

    public void displayClassrooms() {
        if (classrooms.isEmpty()) {
            System.out.println("No classrooms available.");
        } else {
            System.out.println("Available Classrooms: " + classrooms.keySet());
        }
    }
}

// Manages assignments (scheduling and submission)
class AssignmentManager {
    private ClassroomManager classroomManager;

    public AssignmentManager(ClassroomManager classroomManager) {
        this.classroomManager = classroomManager;
    }

    public void scheduleAssignment(String className, String assignmentDetails) {
        Classroom classroom = classroomManager.getClassroom(className);
        if (classroom != null) {
            classroom.scheduleAssignment(assignmentDetails);
        }
    }

    public void submitAssignment(String studentId, String className, String assignmentDetails) {
        Classroom classroom = classroomManager.getClassroom(className);
        if (classroom != null) {
            if (classroom.hasStudent(studentId)) {
                System.out.println("Assignment submitted by Student " + studentId + " in " + className + ".");
            } else {
                System.out.println("Student " + studentId + " is not enrolled in " + className + ".");
            }
        }
    }
}

// Main class to run the application and handle user interaction
public class VirtualClassroomManager {
    public static void main(String[] args) {
        // Instantiate classroom and assignment managers
        ClassroomManager classroomManager = new ClassroomManager();
        AssignmentManager assignmentManager = new AssignmentManager(classroomManager);

        // Scanner for user input
        Scanner scanner = new Scanner(System.in);
        int choice = -1;

        // User interaction loop
        while (choice != 9) {
            // Display Menu
            System.out.println("\n--- Virtual Classroom Manager ---");
            System.out.println("1. Add Classroom");
            System.out.println("2. Enroll Student");
            System.out.println("3. Schedule Assignment");
            System.out.println("4. Submit Assignment");
            System.out.println("5. Display Classrooms");
            System.out.println("6. Display Students in a Classroom");
            System.out.println("7. Display Assignments in a Classroom");
            System.out.println("8. Delete Classroom");
            System.out.println("9. Exit");
            System.out.print("Enter your choice: ");

            // Handle user input
            try {
                choice = Integer.parseInt(scanner.nextLine());

                switch (choice) {
                    case 1:  // Add Classroom
                        System.out.print("Enter Classroom Name: ");
                        String className = scanner.nextLine();
                        classroomManager.addClassroom(className);
                        break;

                    case 2:  // Enroll Student
                        System.out.print("Enter Student ID: ");
                        String studentId = scanner.nextLine();
                        System.out.print("Enter Classroom Name: ");
                        className = scanner.nextLine();
                        Classroom classroom = classroomManager.getClassroom(className);
                        if (classroom != null) {
                            classroom.addStudent(studentId);
                        }
                        break;

                    case 3:  // Schedule Assignment
                        System.out.print("Enter Classroom Name: ");
                        className = scanner.nextLine();
                        System.out.print("Enter Assignment Details: ");
                        String assignmentDetails = scanner.nextLine();
                        assignmentManager.scheduleAssignment(className, assignmentDetails);
                        break;

                    case 4:  // Submit Assignment
                        System.out.print("Enter Student ID: ");
                        studentId = scanner.nextLine();
                        System.out.print("Enter Classroom Name: ");
                        className = scanner.nextLine();
                        System.out.print("Enter Assignment Details: ");
                        assignmentDetails = scanner.nextLine();
                        assignmentManager.submitAssignment(studentId, className, assignmentDetails);
                        break;

                    case 5:  // Display Classrooms
                        classroomManager.displayClassrooms();
                        break;

                    case 6:  // Display Students in a Classroom
                        System.out.print("Enter Classroom Name: ");
                        className = scanner.nextLine();
                        classroom = classroomManager.getClassroom(className);
                        if (classroom != null) {
                            classroom.displayStudents();
                        }
                        break;

                    case 7:  // Display Assignments in a Classroom
                        System.out.print("Enter Classroom Name: ");
                        className = scanner.nextLine();
                        classroom = classroomManager.getClassroom(className);
                        if (classroom != null) {
                            classroom.displayAssignments();
                        }
                        break;

                    case 8:  // Delete Classroom
                        System.out.print("Enter Classroom Name to delete: ");
                        className = scanner.nextLine();
                        classroomManager.deleteClassroom(className);
                        break;

                    case 9:  // Exit
                        System.out.println("Exiting the program...");
                        break;

                    default:
                        System.out.println("Invalid choice. Please enter a number between 1 and 9.");
                }
            } catch (NumberFormatException e) {
                System.out.println("Invalid input. Please enter a valid number.");
            }
        }

        scanner.close();
    }
}
