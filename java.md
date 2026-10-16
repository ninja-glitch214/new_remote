Let's build a more complete Java Student Management System that brings together the major concepts you may be expected to use in an OOP practical or exam.

We'll include inheritance, abstract classes, interfaces, method overloading and overriding, runtime polymorphism, encapsulation, `ArrayList`, `HashSet`, `HashMap`, `Comparable`, multiple `Comparator`s, streams, lambdas, exception handling, file handling, and a menu with nested submenus.

The aim is to show how these concepts work together in one program, rather than adding unrelated concepts just for the sake of it.

## 1. Complete menu structure

Main menu

Student records

Add, display, search, update, delete

Sorting & filtering

Streams, Comparable, Comparators

Statistics

Average, highest, count, grouping

Data utilities

Unique names, ID lookup, export/import

Data is stored in memory during execution and can be saved to a file.

## 2. Full Java program — `Main.java`

This is a single-file program so you can compile and run it easily. It uses Java 17 or later.

```

import java.util.*;
import java.util.function.Predicate;
import java.util.stream.Collectors;
import java.io.*;
import java.nio.file.*;

// ==================================================
// 1. CUSTOM EXCEPTION
// ==================================================
class DuplicateStudentException extends Exception {
    DuplicateStudentException(String message) {
        super(message);
    }
}

// ==================================================
// 2. ABSTRACT CLASS
// ==================================================
abstract class Person {
    private String name;
    private int age;

    Person(String name, int age) {
        setName(name);
        setAge(age);
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public void setName(String name) {
        if (name == null || name.isBlank())
            throw new IllegalArgumentException("Name cannot be empty");
        this.name = name.trim();
    }

    public void setAge(int age) {
        if (age < 1 || age > 120)
            throw new IllegalArgumentException("Age must be 1-120");
        this.age = age;
    }

    public abstract String getRole();

    public void display() {
        System.out.printf("%-15s Age: %-4d", name, age);
    }
}

// ==================================================
// 3. INHERITANCE + COMPARABLE
// ==================================================
class Student extends Person implements Comparable<Student> {
    private final int id;
    private double marks;

    Student(int id, String name, int age, double marks) {
        super(name, age);

        if (id <= 0)
            throw new IllegalArgumentException("ID must be positive");

        this.id = id;
        setMarks(marks);
    }

    public int getId() {
        return id;
    }

    public double getMarks() {
        return marks;
    }

    public void setMarks(double marks) {
        if (marks < 0 || marks > 100)
            throw new IllegalArgumentException("Marks must be 0-100");
        this.marks = marks;
    }

    @Override
    public String getRole() {
        return "Student";
    }

    @Override
    public void display() {
        System.out.printf(
            "ID: %-5d Name: %-15s Age: %-3d Marks: %6.2f%n",
            id, getName(), getAge(), marks
        );
    }

    // Natural ordering: ID ascending
    @Override
    public int compareTo(Student other) {
        return Integer.compare(this.id, other.id);
    }

    // Overloading
    public void updateMarks(double marks) {
        setMarks(marks);
    }

    public void updateMarks(double marks, String message) {
        setMarks(marks);
        System.out.println(message);
    }
}

// ==================================================
// 4. INTERFACE
// ==================================================
interface StudentOperations {
    void addStudent(Student s)
        throws DuplicateStudentException;

    boolean deleteStudent(int id);

    Optional<Student> searchById(int id);

    void displayAll();
}

// ==================================================
// 5. MANAGER CLASS
// ==================================================
class StudentManager implements StudentOperations {
    private final List<Student> students = new ArrayList<>();

    // HashMap for quick ID lookup
    private final Map<Integer, Student> index = new HashMap<>();

    // Interface implementation
    @Override
    public void addStudent(Student s)
            throws DuplicateStudentException {

        if (index.containsKey(s.getId()))
            throw new DuplicateStudentException(
                "Student ID already exists."
            );

        students.add(s);
        index.put(s.getId(), s);
        System.out.println("Student added successfully.");
    }

    // Method overloading
    public void addStudent(
            int id, String name, int age, double marks)
            throws DuplicateStudentException {

        addStudent(new Student(id, name, age, marks));
    }

    @Override
    public boolean deleteStudent(int id) {
        Student s = index.remove(id);

        if (s == null)
            return false;

        students.remove(s);
        return true;
    }

    // Optional handles a possibly missing result
    @Override
    public Optional<Student> searchById(int id) {
        return Optional.ofNullable(index.get(id));
    }

    // Overloaded search
    public List<Student> searchByName(String name) {
        return students.stream()
            .filter(s -> s.getName().equalsIgnoreCase(name))
            .collect(Collectors.toList());
    }

    @Override
    public void displayAll() {
        if (students.isEmpty()) {
            System.out.println("No students available.");
            return;
        }

        // Method reference
        students.forEach(Student::display);
    }

    // UPDATE
    public boolean updateMarks(int id, double marks) {
        Optional<Student> result = searchById(id);

        if (result.isEmpty())
            return false;

        result.get().updateMarks(marks);
        return true;
    }

    // UPDATE NAME
    public boolean updateName(int id, String name) {
        Optional<Student> result = searchById(id);

        if (result.isEmpty())
            return false;

        result.get().setName(name);
        return true;
    }

    // ==================================================
    // COMPARATOR + STREAM SORTING
    // ==================================================

    // Natural order: Comparable, ID ascending
    public void sortById() {
        students.stream()
            .sorted()
            .forEach(Student::display);
    }

    // Comparator: marks descending
    public void sortByMarks() {
        students.stream()
            .sorted(
                Comparator.comparingDouble(Student::getMarks)
                          .reversed()
            )
            .forEach(Student::display);
    }

    // Another Comparator: name ascending
    public void sortByName() {
        students.stream()
            .sorted(
                Comparator.comparing(
                    Student::getName,
                    String.CASE_INSENSITIVE_ORDER
                )
            )
            .forEach(Student::display);
    }

    // Multiple comparison criteria
    public void sortByMarksThenName() {
        students.stream()
            .sorted(
                Comparator.comparingDouble(Student::getMarks)
                    .reversed()
                    .thenComparing(Student::getName)
            )
            .forEach(Student::display);
    }

    // ==================================================
    // STREAM FILTERING
    // ==================================================

    public void filter(Predicate<Student> condition) {
        List<Student> result = students.stream()
            .filter(condition)
            .collect(Collectors.toList());

        if (result.isEmpty()) {
            System.out.println("No matching students.");
            return;
        }

        result.forEach(Student::display);
    }

    // ==================================================
    // STREAM STATISTICS
    // ==================================================

    public void statistics() {
        if (students.isEmpty()) {
            System.out.println("No data available.");
            return;
        }

        DoubleSummaryStatistics stats = students.stream()
            .collect(Collectors.summarizingDouble(Student::getMarks));

        System.out.println("Total students: " + stats.getCount());
        System.out.printf("Average marks: %.2f%n", stats.getAverage());
        System.out.println("Highest marks: " + stats.getMax());
        System.out.println("Lowest marks: " + stats.getMin());

        long passed = students.stream()
            .filter(s -> s.getMarks() >= 40)
            .count();

        System.out.println("Passed: " + passed);
        System.out.println("Failed: " + (stats.getCount() - passed));

        // partitioningBy + counting
        Map<Boolean, Long> groups = students.stream()
            .collect(Collectors.partitioningBy(
                s -> s.getMarks() >= 40,
                Collectors.counting()
            ));

        System.out.println("Pass/fail groups: " + groups);
    }

    // ==================================================
    // SET + MAP UTILITIES
    // ==================================================

    public void uniqueNames() {
        Set<String> names = students.stream()
            .map(Student::getName)
            .collect(Collectors.toCollection(
                () -> new TreeSet<>(String.CASE_INSENSITIVE_ORDER)
            ));

        System.out.println("Unique student names:");
        names.forEach(System.out::println);
    }

    public void displayIdMap() {
        index.forEach((id, student) -> {
            System.out.print("ID " + id + " -> ");
            System.out.println(student.getName());
        });
    }

    public boolean isEmpty() {
        return students.isEmpty();
    }

    // ==================================================
    // FILE HANDLING: SAVE TO TSV FILE
    // ==================================================

    public void saveToFile(String filename) throws IOException {
        try (BufferedWriter writer =
                 Files.newBufferedWriter(Path.of(filename))) {

            for (Student s : students) {
                writer.write(
                    s.getId() + "\t" +
                    s.getName() + "\t" +
                    s.getAge() + "\t" +
                    s.getMarks()
                );
                writer.newLine();
            }
        }

        System.out.println("Data saved to " + filename);
    }

    // LOAD DATA FROM TSV FILE
    public void loadFromFile(String filename)
            throws IOException {

        List<Student> loaded = new ArrayList<>();
        Set<Integer> ids = new HashSet<>();

        try (BufferedReader reader =
                 Files.newBufferedReader(Path.of(filename))) {

            String line;
            int lineNumber = 0;

            while ((line = reader.readLine()) != null) {
                lineNumber++;

                try {
                    String[] data = line.split("\t", -1);

                    if (data.length != 4)
                        throw new IllegalArgumentException("Invalid columns");

                    Student s = new Student(
                        Integer.parseInt(data[0]),
                        data[1],
                        Integer.parseInt(data[2]),
                        Double.parseDouble(data[3])
                    );

                    if (!ids.add(s.getId()))
                        throw new IllegalArgumentException("Duplicate ID");

                    loaded.add(s);

                } catch (IllegalArgumentException e) {
                    System.out.println(
                        "Skipping invalid line " + lineNumber +
                        ": " + e.getMessage()
                    );
                }
            }
        }

        // Replace existing data only after reading the file
        students.clear();
        index.clear();

        for (Student s : loaded) {
            students.add(s);
            index.put(s.getId(), s);
        }

        System.out.println("Loaded " + students.size() + " students.");
    }
}

// ==================================================
// 6. MAIN CLASS + MENU
// ==================================================
public class Main {
    private static final Scanner sc = new Scanner(System.in);
    private static final StudentManager manager = new StudentManager();
    private static final String FILE = "students.txt";

    // Exception handling for invalid numeric input
    static int readInt(String message) {
        while (true) {
            try {
                System.out.print(message);
                return Integer.parseInt(sc.nextLine().trim());
            } catch (NumberFormatException e) {
                System.out.println("Please enter a valid integer.");
            }
        }
    }

    static double readDouble(String message) {
        while (true) {
            try {
                System.out.print(message);
                return Double.parseDouble(sc.nextLine().trim());
            } catch (NumberFormatException e) {
                System.out.println("Please enter a valid number.");
            }
        }
    }

    static String readText(String message) {
        System.out.print(message);
        return sc.nextLine().trim();
    }

    // ADD MENU
    static void addMenu() {
        try {
            int id = readInt("ID: ");
            String name = readText("Name: ");
            int age = readInt("Age: ");
            double marks = readDouble("Marks (0-100): ");

            manager.addStudent(id, name, age, marks);

        } catch (DuplicateStudentException |
                 IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }

    // SEARCH MENU
    static void searchMenu() {
        System.out.println("1. Search by ID");
        System.out.println("2. Search by exact name");

        int choice = readInt("Choice: ");

        switch (choice) {
            case 1 -> {
                int id = readInt("Enter ID: ");

                manager.searchById(id)
                    .ifPresentOrElse(
                        Student::display,
                        () -> System.out.println("Student not found.")
                    );
            }

            case 2 -> {
                String name = readText("Enter name: ");
                List<Student> result = manager.searchByName(name);

                if (result.isEmpty())
                    System.out.println("Student not found.");
                else
                    result.forEach(Student::display);
            }

            default -> System.out.println("Invalid choice.");
        }
    }

    // UPDATE MENU
    static void updateMenu() {
        System.out.println("1. Update name");
        System.out.println("2. Update marks");

        int choice = readInt("Choice: ");
        int id = readInt("Student ID: ");

        try {
            boolean updated;

            switch (choice) {
                case 1 -> updated = manager.updateName(
                    id, readText("New name: ")
                );

                case 2 -> updated = manager.updateMarks(
                    id, readDouble("New marks: ")
                );

                default -> {
                    System.out.println("Invalid choice.");
                    return;
                }
            }

            System.out.println(
                updated ? "Updated successfully."
                        : "Student not found."
            );

        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }

    // SORT MENU
    static void sortMenu() {
        System.out.println("1. By ID (Comparable)");
        System.out.println("2. By marks descending (Comparator)");
        System.out.println("3. By name");
        System.out.println("4. By marks, then name");

        int choice = readInt("Choice: ");

        switch (choice) {
            case 1 -> manager.sortById();
            case 2 -> manager.sortByMarks();
            case 3 -> manager.sortByName();
            case 4 -> manager.sortByMarksThenName();
            default -> System.out.println("Invalid choice.");
        }
    }

    // FILTER MENU
    static void filterMenu() {
        System.out.println("1. Marks above a value");
        System.out.println("2. Passed students (>= 40)");
        System.out.println("3. Failed students (< 40)");
        System.out.println("4. Marks within a range");

        int choice = readInt("Choice: ");

        switch (choice) {
            case 1 -> {
                double min = readDouble("Minimum marks: ");
                manager.filter(s -> s.getMarks() > min);
            }

            case 2 ->
                manager.filter(s -> s.getMarks() >= 40);

            case 3 ->
                manager.filter(s -> s.getMarks() < 40);

            case 4 -> {
                double low = readDouble("Minimum marks: ");
                double high = readDouble("Maximum marks: ");

                if (low < 0 || high > 100 || low > high) {
                    System.out.println("Invalid range.");
                    return;
                }

                manager.filter(s ->
                    s.getMarks() >= low && s.getMarks() <= high
                );
            }

            default -> System.out.println("Invalid choice.");
        }
    }

    // FILE MENU
    static void fileMenu() {
        System.out.println("1. Save data");
        System.out.println("2. Load data (replaces current data)");

        int choice = readInt("Choice: ");

        try {
            switch (choice) {
                case 1 -> manager.saveToFile(FILE);
                case 2 -> manager.loadFromFile(FILE);
                default -> System.out.println("Invalid choice.");
            }
        } catch (IOException e) {
            System.out.println("File error: " + e.getMessage());
        }
    }

    public static void main(String[] args) {
        while (true) {
            System.out.println("\n========== STUDENT MANAGEMENT ==========");
            System.out.println("1. Add student");
            System.out.println("2. Display all students");
            System.out.println("3. Search student");
            System.out.println("4. Update student");
            System.out.println("5. Delete student");
            System.out.println("6. Sorting menu");
            System.out.println("7. Filtering menu");
            System.out.println("8. Statistics");
            System.out.println("9. Unique names");
            System.out.println("10. Display ID map");
            System.out.println("11. Save / Load file");
            System.out.println("12. Exit");

            int choice = readInt("Enter choice: ");

            try {
                switch (choice) {
                    case 1 -> addMenu();

                    case 2 -> manager.displayAll();

                    case 3 -> searchMenu();

                    case 4 -> updateMenu();

                    case 5 -> {
                        int id = readInt("ID to delete: ");

                        System.out.println(
                            manager.deleteStudent(id)
                                ? "Student deleted."
                                : "Student not found."
                        );
                    }

                    case 6 -> sortMenu();

                    case 7 -> filterMenu();

                    case 8 -> manager.statistics();

                    case 9 -> manager.uniqueNames();

                    case 10 -> manager.displayIdMap();

                    case 11 -> fileMenu();

                    case 12 -> {
                        System.out.println("Goodbye!");
                        sc.close();
                        return;
                    }

                    default -> System.out.println("Invalid choice.");
                }

            } catch (IllegalArgumentException e) {
                System.out.println("Error: " + e.getMessage());
            }
        }
    }
}

```

## 3. Concepts implemented — revision table

| Java concept              | Where it appears                                                   |
| ------------------------- | ------------------------------------------------------------------ |
| Class and object          | `Student`, `Person`, `StudentManager`                              |
| Encapsulation             | Private fields and getters/setters                                 |
| Inheritance               | `Student extends Person`                                           |
| Abstract class and method | `Person`, `getRole()`                                              |
| Interface                 | `StudentOperations`                                                |
| Runtime polymorphism      | `StudentOperations manager` can reference `StudentManager`         |
| Method overriding         | `Student.display()` and interface implementations                  |
| Method overloading        | `addStudent()`, `searchById()` / `searchByName()`, `updateMarks()` |
| `Comparable`              | Natural ordering by ID                                             |
| Multiple `Comparator`s    | Sort by marks, name, or both                                       |
| `ArrayList`               | Stores student records                                             |
| `HashMap`                 | Fast ID lookup                                                     |
| `HashSet` / `TreeSet`     | Unique ID validation and unique names                              |
| Streams and lambdas       | Filtering, sorting, statistics                                     |
| Method references         | `Student::display`                                                 |
| `Predicate`               | Reusable filtering condition                                       |
| `Optional`                | Safe representation of a missing student                           |
| `Collectors`              | Statistics and pass/fail grouping                                  |
| Exception handling        | Invalid inputs and duplicate IDs                                   |
| Custom exception          | `DuplicateStudentException`                                        |
| File handling             | Save and load student data                                         |
| `switch` and loops        | Main menu and submenus                                             |
| `Scanner`                 | User input                                                         |
| `static` and `final`      | Shared scanner and fixed configuration                             |

## 4. How to run the program

1. Save the code as `Main.java`.
2. Open a terminal in that folder.
3. Compile:
   ```
   javac Main.java
   ```
4. Run:
   ```
   java Main
   ```
5. Try adding a few students, sorting them by marks, filtering those who passed, and saving the data. Restart the program and load the saved file.

Important limitations: the program stores data in memory until you load or save the file manually. The file format is tab-separated, so names containing tabs or newlines aren't supported. Also, updating a student's name or marks changes the in-memory object, but it does not save that change to disk automatically.

## 5. Best way to study this program

Don't try to memorize the entire program at once. Understand it in this order:

1. `Person` and `Student` — inheritance, constructors, encapsulation.
2. `StudentOperations` and `StudentManager` — interfaces and polymorphism.
3. `ArrayList`, `HashMap`, `Comparable`, and `Comparator`.
4. Stream methods: `filter`, `sorted`, `map`, `count`, `collect`.
5. Menu methods and exception handling.
6. File handling and statistics.

Once you can explain each section and add a new menu option yourself, you'll be able to build similar menu-driven Java programs for employees, library books, bank accounts, or inventory management.
