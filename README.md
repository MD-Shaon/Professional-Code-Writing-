
import java.io.*;
import java.util.*;

public class StudentList {

    static final String FILE_NAME = "students.txt";

    public static void main(String[] args) {
        System.out.println("Loading data ...");

        try {
            if (args.length < 1) {
                System.out.println("Invalid arguments.");
                System.out.println("Data Loaded.");
                return;
            }

            List<String> students = loadStudents();

            switch (args[0].charAt(0)) {

                case 'a': // Show all
                    for (String student : students) {
                        System.out.println(student);
                    }
                    break;

                case 'r': // Random student
                    Random rand = new Random();
                    int index = rand.nextInt(students.size());
                    System.out.println(students.get(index));
                    break;

                case 'c': // Count
                    System.out.println(students.size() + " word(s) found");
                    break;

                case '?': // Search
                    String searchName = args[0].substring(1);
                    if (students.contains(searchName)) {
                        System.out.println("We found it!");
                    } else {
                        System.out.println(searchName + " not found.");
                    }
                    break;

                case '+': // Add student
                    String newStudent = args[0].substring(1);
                    students.add(newStudent);
                    saveStudents(students);
                    break;

                default:
                    System.out.println("Invalid command.");
            }
        } catch (Exception e) {
            System.out.println("An error occurred: " + e.getMessage());
        }

        System.out.println("Data Loaded.");
    }

    private static List<String> loadStudents() throws IOException {
        BufferedReader reader = new BufferedReader(new FileReader(FILE_NAME));
        String line = reader.readLine();
        reader.close();

        String[] parts = line.split(",");
        List<String> students = new ArrayList<>();
        for (String name : parts) {
            students.add(name.trim());
        }
        return students;
    }

    private static void saveStudents(List<String> students) throws IOException {
        BufferedWriter writer = new BufferedWriter(new FileWriter(FILE_NAME));
        for (int i = 0; i < students.size(); i++) {
            writer.write(students.get(i));
            if (i != students.size() - 1) writer.write(", ");
        }
        writer.newLine();
        writer.write("List last updated on " + new Date());
        writer.newLine();
        writer.close();
    }
}
