import java.io.*;
import java.util.*;

class Contact {
    String name;
    String phoneNumber;
    String email;

    public Contact(String name, String phoneNumber, String email) {
        this.name = name;
        this.phoneNumber = phoneNumber;
        this.email = email;
    }

    @Override
    public String toString() {
        return "Name: " + name + ", Phone: " + phoneNumber + ", Email: " + email;
    }

    // Convert Contact object to a CSV string format for file storage
    public String toCSV() {
        return name + "," + phoneNumber + "," + email;
    }

    // Static method to create Contact from CSV string
    public static Contact fromCSV(String csv) {
        String[] parts = csv.split(",");
        return new Contact(parts[0], parts[1], parts[2]);
    }
}

public class ContactManagementSystem {

    private static final String FILE_NAME = "contacts.txt";
    private static ArrayList<Contact> contactList = new ArrayList<>();
    private static Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        loadContactsFromFile();

        int choice;
        do {
            System.out.println("\nContact Management System");
            System.out.println("1. Add Contact");
            System.out.println("2. View Contacts");
            System.out.println("3. Edit Contact");
            System.out.println("4. Delete Contact");
            System.out.println("5. Exit");
            System.out.print("Enter your choice: ");
            choice = scanner.nextInt();
            scanner.nextLine(); // Consume newline

            switch (choice) {
                case 1:
                    addContact();
                    break;
                case 2:
                    viewContacts();
                    break;
                case 3:
                    editContact();
                    break;
                case 4:
                    deleteContact();
                    break;
                case 5:
                    System.out.println("Exiting the system. Goodbye!");
                    saveContactsToFile();
                    break;
                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        } while (choice != 5);
    }

    private static void loadContactsFromFile() {
        try {
            BufferedReader reader = new BufferedReader(new FileReader(FILE_NAME));
            String line;
            while ((line = reader.readLine()) != null) {
                contactList.add(Contact.fromCSV(line));
            }
            reader.close();
        } catch (IOException e) {
            System.out.println("No existing contacts file found. Starting fresh.");
        }
    }

    private static void saveContactsToFile() {
        try {
            BufferedWriter writer = new BufferedWriter(new FileWriter(FILE_NAME));
            for (Contact contact : contactList) {
                writer.write(contact.toCSV());
                writer.newLine();
            }
            writer.close();
        } catch (IOException e) {
            System.out.println("Error saving contacts to file.");
        }
    }

    private static void addContact() {
        System.out.print("\nEnter Name: ");
        String name = scanner.nextLine();
        System.out.print("Enter Phone Number: ");
        String phoneNumber = scanner.nextLine();
        System.out.print("Enter Email: ");
        String email = scanner.nextLine();

        Contact newContact = new Contact(name, phoneNumber, email);
        contactList.add(newContact);
        System.out.println("Contact added successfully!");
    }

    private static void viewContacts() {
        if (contactList.isEmpty()) {
            System.out.println("No contacts available.");
        } else {
            System.out.println("\nContacts List:");
            for (int i = 0; i < contactList.size(); i++) {
                System.out.println((i + 1) + ". " + contactList.get(i));
            }
        }
    }

    private static void editContact() {
        if (contactList.isEmpty()) {
            System.out.println("No contacts available to edit.");
            return;
        }
        viewContacts();
        System.out.print("\nEnter the contact number to edit: ");
        int contactNumber = scanner.nextInt();
        scanner.nextLine(); // Consume newline

        if (contactNumber > 0 && contactNumber <= contactList.size()) {
            Contact contactToEdit = contactList.get(contactNumber - 1);

            System.out.print("Enter new name (or press Enter to keep \"" + contactToEdit.name + "\"): ");
            String name = scanner.nextLine();
            if (!name.isEmpty()) contactToEdit.name = name;

            System.out.print("Enter new phone number (or press Enter to keep \"" + contactToEdit.phoneNumber + "\"): ");
            String phoneNumber = scanner.nextLine();
            if (!phoneNumber.isEmpty()) contactToEdit.phoneNumber = phoneNumber;

            System.out.print("Enter new email (or press Enter to keep \"" + contactToEdit.email + "\"): ");
            String email = scanner.nextLine();
            if (!email.isEmpty()) contactToEdit.email = email;

            System.out.println("Contact updated successfully!");
        } else {
            System.out.println("Invalid contact number.");
        }
    }

    private static void deleteContact() {
        if (contactList.isEmpty()) {
            System.out.println("No contacts available to delete.");
            return;
        }
        viewContacts();
        System.out.print("\nEnter the contact number to delete: ");
        int contactNumber = scanner.nextInt();
        scanner.nextLine(); // Consume newline

        if (contactNumber > 0 && contactNumber <= contactList.size()) {
            contactList.remove(contactNumber - 1);
            System.out.println("Contact deleted successfully!");
        } else {
            System.out.println("Invalid contact number.");
        }
    }
}
