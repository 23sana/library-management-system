# library-management-system
import javax.mail.*;
import javax.mail.internet.*;
import java.util.*;
import java.io.*;
import java.time.*;
import java.time.temporal.ChronoUnit;

// Main class to run the system
public class LibraryManagementSystem {
    public static void main(String[] args) {
        LibraryService libraryService = new LibraryService();
        Scanner scanner = new Scanner(System.in);
        int choice;

        // Load data from files or database
        libraryService.loadData();

        // Main loop
        do {
            System.out.println("Library Management System");
            System.out.println("1. Admin Login");
            System.out.println("2. Librarian Login");
            System.out.println("3. Patron Login");
            System.out.println("4. Register New Patron");
            System.out.println("5. Recover Password");
            System.out.println("6. Exit");
            System.out.print("Enter your choice: ");
            choice = scanner.nextInt();
            scanner.nextLine(); // Consume newline

            switch (choice) {
                case 1:
                    libraryService.adminLogin(scanner);
                    break;
                case 2:
                    libraryService.librarianLogin(scanner);
                    break;
                case 3:
                    libraryService.patronLogin(scanner);
                    break;
                case 4:
                    libraryService.registerPatron(scanner);
                    break;
                case 5:
                    libraryService.recoverPassword(scanner);
                    break;
                case 6:
                    libraryService.saveData();
                    System.out.println("Exiting...");
                    break;
                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        } while (choice != 6);

        scanner.close();
    }
}

// Book class
class Book {
    private String title;
    private Author author;
    private String isbn;
    private boolean isAvailable;
    private LocalDate dueDate;
    private double lateFee;
    private Set<String> genres;
    private List<String> borrowHistory;
    private List<Patron> waitlist;
    private LocalDate publicationDate;
    private List<Review> reviews;

    public Book(String title, Author author, String isbn, LocalDate publicationDate) {
        this.title = title;
        this.author = author;
        this.isbn = isbn;
        this.isAvailable = true;
        this.genres = new HashSet<>();
        this.borrowHistory = new ArrayList<>();
        this.waitlist = new ArrayList<>();
        this.publicationDate = publicationDate;
        this.reviews = new ArrayList<>();
    }

    // Getters and setters
    public String getTitle() {
        return title;
    }

    public Author getAuthor() {
        return author;
    }

    public String getIsbn() {
        return isbn;
    }

    public boolean isAvailable() {
        return isAvailable;
    }

    public void setAvailable(boolean available) {
        isAvailable = available;
    }

    public LocalDate getDueDate() {
        return dueDate;
    }

    public void setDueDate(LocalDate dueDate) {
        this.dueDate = dueDate;
    }

    public double getLateFee() {
        return lateFee;
    }

    public void setLateFee(double lateFee) {
        this.lateFee = lateFee;
    }

    public Set<String> getGenres() {
        return genres;
    }

    public void addGenre(String genre) {
        this.genres.add(genre);
    }

    public void removeGenre(String genre) {
        this.genres.remove(genre);
    }

    public List<String> getBorrowHistory() {
        return borrowHistory;
    }

    public void addBorrowHistory(String patronName) {
        this.borrowHistory.add(patronName);
    }

    public List<Patron> getWaitlist() {
        return waitlist;
    }

    public void addToWaitlist(Patron patron) {
        this.waitlist.add(patron);
    }

    public void removeFromWaitlist(Patron patron) {
        this.waitlist.remove(patron);
    }

    public LocalDate getPublicationDate() {
        return publicationDate;
    }

    public List<Review> getReviews() {
        return reviews;
    }

    public void addReview(Review review) {
        this.reviews.add(review);
    }

    @Override
    public String toString() {
        return "Book [Title=" + title + ", Author=" + author.getName() + ", ISBN=" + isbn + ", Available=" + isAvailable + "]";
    }
}

// Review class for book reviews
class Review {
    private String patronName;
    private String comment;
    private int rating; // Rating out of 5

    public Review(String patronName, String comment, int rating) {
        this.patronName = patronName;
        this.comment = comment;
        this.rating = rating;
    }

    @Override
    public String toString() {
        return "Review by " + patronName + ": " + comment + " (Rating: " + rating + ")";
    }
}

// Author class
class Author {
    private String name;
    private String biography;

    public Author(String name, String biography) {
        this.name = name;
        this.biography = biography;
    }

    public String getName() {
        return name;
    }

    public String getBiography() {
        return biography;
    }
}

// Patron class
class Patron {
    private String username;
    private String email;
    private String password;
    private List<Book> borrowedBooks;

    public Patron(String username, String email, String password) {
        this.username = username;
        this.email = email;
        this.password = password;
        this.borrowedBooks = new ArrayList<>();
    }

    public String getUsername() {
        return username;
    }

    public String getEmail() {
        return email;
    }

    public String getPassword() {
        return password;
    }

    public List<Book> getBorrowedBooks() {
        return borrowedBooks;
    }

    public void borrowBook(Book book) {
        borrowedBooks.add(book);
    }

    public void returnBook(Book book) {
        borrowedBooks.remove(book);
    }
}

// LibraryService class to handle library operations
class LibraryService {
    private List<Book> books;
    private List<Patron> patrons;

    public LibraryService() {
        books = new ArrayList<>();
        patrons = new ArrayList<>();
    }

    public void loadData() {
        // Load books and patrons from files or database
    }

    public void saveData() {
        // Save books and patrons to files or database
    }

    public void adminLogin(Scanner scanner) {
        // Admin login logic
    }

    public void librarianLogin(Scanner scanner) {
        // Librarian login logic
    }

    public void patronLogin(Scanner scanner) {
        // Patron login logic
    }

    public void registerPatron(Scanner scanner) {
        System.out.print("Enter username: ");
        String username = scanner.nextLine();
        System.out.print("Enter email: ");
        String email = scanner.nextLine();
        System.out.print("Enter password: ");
        String password = scanner.nextLine();
        patrons.add(new Patron(username, email, password));
        System.out.println("Patron registered successfully.");
    }

    public void recoverPassword(Scanner scanner) {
        System.out.print("Enter your email: ");
        String email = scanner.nextLine();
        Optional<Patron> patron = patrons.stream()
                                         .filter(p -> p.getEmail().equals(email))
                                         .findFirst();
        if (patron.isPresent()) {
            // Simulate sending a recovery email
            System.out.println("Recovery email sent to " + email);
        } else {
            System.out.println("Email not found.");
        }
    }

    // Additional methods for book management, borrowing, and reviews
}
``` ```java
    public void searchBooks(Scanner scanner) {
        System.out.print("Enter search term (title/author): ");
        String searchTerm = scanner.nextLine();
        books.stream()
             .filter(book -> book.getTitle().toLowerCase().contains(searchTerm.toLowerCase()) ||
                             book.getAuthor().getName().toLowerCase().contains(searchTerm.toLowerCase()))
             .forEach(System.out::println);
    }

    public void reserveBook(Scanner scanner) {
        System.out.print("Enter ISBN of the book to reserve: ");
        String isbn = scanner.nextLine();
        Optional<Book> bookOpt = books.stream()
                                      .filter(book -> book.getIsbn().equals(isbn) && book.isAvailable())
                                      .findFirst();
        if (bookOpt.isPresent()) {
            System.out.print("Enter your username: ");
            String username = scanner.nextLine();
            Optional<Patron> patronOpt = patrons.stream()
                                                .filter(p -> p.getUsername().equals(username))
                                                .findFirst();
            if (patronOpt.isPresent()) {
                bookOpt.get().addToWaitlist(patronOpt.get());
                System.out.println("Book reserved successfully.");
            } else {
                System.out.println("Patron not found.");
            }
        } else {
            System.out.println("Book is unavailable.");
        }
    }

    public void addReview(Scanner scanner) {
        System.out.print("Enter ISBN of the book: ");
        String isbn = scanner.nextLine();
        Optional<Book> bookOpt = books.stream()
                                      .filter(book -> book.getIsbn().equals(isbn))
                                      .findFirst();
        if (bookOpt.isPresent()) {
            System.out.print("Enter your name: ");
            String patronName = scanner.nextLine();
            System.out.print("Enter your review: ");
            String comment = scanner.nextLine();
            System.out.print("Enter your rating (1-5): ");
            int rating = scanner.nextInt();
            scanner.nextLine(); // Consume newline
            Review review = new Review(patronName, comment, rating);
            bookOpt.get().addReview(review);
            System.out.println("Review added successfully.");
        } else {
            System.out.println("Book not found.");
        }
    }

    public void generateReports() {
        // Logic to generate reports for book statistics and user activity
    }

    public void sendEmailNotification(String email, String message) {
        // Logic to send email notifications for overdue books or new arrivals
    }
}
``` ```java
// Additional methods for data persistence
public void importData() {
    // Logic to import data from files or database
}

public void exportData() {
    // Logic to export data to files or database
}

// User Interface methods
public void displayMenu() {
    System.out.println("1. Search Books");
    System.out.println("2. Reserve Book");
    System.out.println("3. Add Review");
    System.out.println("4. Generate Reports");
    System.out.println("5. Exit");
}

// Main method to run the application
public static void main(String[] args) {
    LibraryManagementSystem librarySystem = new LibraryManagementSystem();
    librarySystem.run();
}

// Run method to handle the application flow
public void run() {
    Scanner scanner = new Scanner(System.in);
    int choice;

    do {
        displayMenu();
        System.out.print("Enter your choice: ");
        choice = scanner.nextInt();
        scanner.nextLine(); // Consume newline

        switch (choice) {
            case 1:
                searchBooks(scanner);
                break;
            case 2:
                reserveBook(scanner);
                break;
            case 3:
                addReview(scanner);
                break;
            case 4:
                generateReports();
                break;
            case 5:
                System.out.println("Exiting...");
                break;
            default:
                System.out.println("Invalid choice. Please try again.");
        }
    } while (choice != 5);

    scanner.close();
}
``` ```java
// Additional methods for data persistence
public void importData() {
    // Logic to import data from files or database
}

public void exportData() {
    // Logic to export data to files or database
}

// User Interface methods
public void displayMenu() {
    System.out.println("1. Search Books");
    System.out.println("2. Reserve Book");
    System.out.println("3. Add Review");
    System.out.println("4. Generate Reports");
    System.out.println("5. Exit");
}

// Main method to run the application
public static void main(String[] args) {
    LibraryManagementSystem librarySystem = new LibraryManagementSystem();
    librarySystem.run();
}

// Run method to handle the application flow
public void run() {
    Scanner scanner = new Scanner(System.in);
    int choice;

    do {
        displayMenu();
        System.out.print("Enter your choice: ");
        choice = scanner.nextInt();
        scanner.nextLine(); // Consume newline

        switch (choice) {
            case 1:
                searchBooks(scanner);
                break;
            case 2:
                reserveBook(scanner);
                break;
            case 3:
                addReview(scanner);
                break;
            case 4:
                generateReports();
                break;
            case 5:
                System.out.println("Exiting...");
                break;
            default:
                System.out.println("Invalid choice. Please try again.");
        }
    } while (choice != 5);

    scanner.close();
}
``` ```java
// Additional methods for data persistence
public void importData() {
    // Logic to import data from files or database
}

public void exportData() {
    // Logic to export data to files or database
}

// User Interface methods
public void displayMenu() {
    System.out.println("1. Search Books");
    System.out.println("2. Reserve Book");
    System.out.println("3. Add Review");
    System.out.println("4. Generate Reports");
    System.out.println("5. Exit");
}

// Main method to run the application
public static void main(String[] args) {
    LibraryManagementSystem librarySystem = new LibraryManagementSystem();
    librarySystem.run();
}

// Run method to handle the application flow
public void run() {
    Scanner scanner = new Scanner(System.in);
    int choice;

    do {
        displayMenu();
        System.out.print("Enter your choice: ");
        choice = scanner.nextInt();
        scanner.nextLine(); // Consume newline

        switch (choice) {
            case 1:
                searchBooks(scanner);
                break;
            case 2:
                reserveBook(scanner);
                break;
            case 3:
                addReview(scanner);
                break;
            case 4:
                generateReports();
                break;
            case 5:
                System.out.println("Exiting...");
                break;
            default:
                System.out.println("Invalid choice. Please try again.");
        }
    } while (choice != 5);

    scanner.close();
}
``` ```java
// Additional methods for data persistence
public void importData() {
    // Logic to import data from files or database
}

public void exportData() {
    // Logic to export data to files or database
}

// User Interface methods
public void displayMenu() {
    System.out.println("1. Search Books");
    System.out.println("2. Reserve Book");
    System.out.println("3. Add Review");
    System.out.println("4. Generate Reports");
    System.out.println("5. Exit");
}

// Main method to run the application
public static void main(String[] args) {
    LibraryManagementSystem librarySystem = new LibraryManagementSystem();
    librarySystem.run();
}

// Run method to handle the application flow
public void run() {
    Scanner scanner = new Scanner(System.in);
    int choice;

    do {
        displayMenu();
        System.out.print("Enter your choice: ");
        choice = scanner.nextInt();
        scanner.nextLine(); // Consume newline

        switch (choice) {
            case 1:
                searchBooks(scanner);
                break;
            case 2:
                reserveBook(scanner);
                break;
            case 3:
                addReview(scanner);
                break;
            case 4:
                generateReports();
                break;
            case 5:
                System.out.println("Exiting...");
                break;
            default:
                System.out.println("Invalid choice. Please try again.");
        }
    } while (choice != 5);

    scanner.close();
}
``` ```java
// Additional methods for data persistence
public void importData() {
    // Logic to import data from files or database
}

public void exportData() {
    // Logic to export data to files or database
}

// User Interface methods
public void displayMenu() {
    System.out.println("1. Search Books");
    System.out.println("2. Reserve Book");
    System.out.println("3. Add Review");
    System.out.println("4. Generate Reports");
    System.out.println("5. Exit");
}

// Main method to run the application
public static void main(String[] args) {
    LibraryManagementSystem librarySystem = new LibraryManagementSystem();
    librarySystem.run();
}

// Run method to handle the application flow
public void run() {
    Scanner scanner = new Scanner(System.in);
    int choice;

    do {
        displayMenu();
        System.out.print("Enter your choice: ");
        choice = scanner.nextInt();
        scanner.nextLine(); // Consume newline

        switch (choice) {
            case 1:
                searchBooks(scanner);
                break;
            case 2:
                reserveBook(scanner);
                break;
            case 3:
                addReview(scanner);
                break;
            case 4:
                generateReports();
                break;
            case 5:
                System.out.println("Exiting...");
                break;
            default:
                System.out.println("Invalid choice. Please try again.");
        }
    } while (choice != 5);

    scanner.close();
}
``` ```java
// Additional methods for data persistence
public void importData() {
    // Logic to import data from files or database
}

public void exportData() {
    // Logic to export data to files or database
}

// User Interface methods
public void displayMenu() {
    System.out.println("1. Search Books");
    System.out.println("2. Reserve Book");
    System.out.println("3. Add Review");
    System.out.println("4. Generate Reports");
    System.out.println("5. Exit");
}

// Main method to run the application
public static void main(String[] args) {
    LibraryManagementSystem librarySystem = new LibraryManagementSystem();
    librarySystem.run();
}

// Run method to handle the application flow
public void run() {
    Scanner scanner = new Scanner(System.in);
    int choice;

    do {
        displayMenu();
        System.out.print("Enter your choice: ");
        choice = scanner.nextInt();
        scanner.nextLine(); // Consume newline

        switch (choice) {
            case 1:
                searchBooks(scanner);
                break;
            case 2:
                reserveBook(scanner);
                break;
            case 3:
                addReview(scanner);
                break;
            case 4:
                generateReports();
                break;
            case 5:
                System.out.println("Exiting...");
                break;
            default:
                System.out.println("Invalid choice. Please try again.");
        }
    } while (choice != 5);

    scanner.close();
}
``` ```java
// Additional methods for data persistence
public void importData() {
    // Logic to import data from files or database
}

public void exportData() {
    // Logic to export data to files or database
}

// User Interface methods
public void displayMenu() {
    System.out.println("1. Search Books");
    System.out.println("2. Reserve Book");
    System.out.println("3. Add Review");
    System.out.println("4. Generate Reports");
    System.out.println("5. Exit");
}

// Main method to run the application
public static void main(String[] args) {
    LibraryManagementSystem librarySystem = new LibraryManagementSystem();
    librarySystem.run();
}

// Run method to handle the application flow
public void run() {
    Scanner scanner = new Scanner(System.in);
    int choice;

    do {
        displayMenu();
        System.out.print("Enter your choice: ");
        choice = scanner.nextInt();
        scanner.nextLine(); // Consume newline

        switch (choice) {
            case 1:
                searchBooks(scanner);
                break;
            case 2:
                reserveBook(scanner);
                break;
            case 3:
                addReview(scanner);
                break;
            case 4:
                generateReports();
                break;
            case 5:
                System.out.println("Exiting...");
                break;
            default:
                System.out.println("Invalid choice. Please try again.");
        }
    } while (choice != 5);

    scanner.close();
}
``` ```java
// Additional methods for data persistence
public void importData() {
    // Logic to import data from files or database
}

public void exportData() {
    // Logic to export data to files or database
}

// User Interface methods
public void displayMenu() {
    System.out.println("1. Search Books");
    System.out.println("2. Reserve Book");
    System.out.println("3. Add Review");
    System.out.println("4. Generate Reports");
    System.out.println("5. Exit");
}

// Main method to run the application
public static void main(String[] args) {
    LibraryManagementSystem librarySystem = new LibraryManagementSystem();
    librarySystem.run();
}

// Run method to handle the application flow
public void run() {
    Scanner scanner = new Scanner(System.in);
    int choice;

    do {
        displayMenu();
        System.out.print("Enter your choice: ");
        choice = scanner.nextInt();
        scanner.nextLine(); // Consume newline

        switch (choice) {
            case 1:
                searchBooks(scanner);
                break;
            case 2:
                reserveBook(scanner);
                break;
            case 3:
                addReview(scanner);
                break;
            case 4:
                generateReports();
                break;
            case 5:
                System.out.println("Exiting...");
                break;
            default:
                System.out.println("Invalid choice. Please try again.");
        }
    } while (choice != 5);

    scanner.close();
}
``` ```java
// Additional methods for data persistence
public void importData() {
    // Logic to import data from files or database
}

public void exportData() {
    // Logic to export data to files or database
}

// User Interface methods
public void displayMenu() {
    System.out.println("1. Search Books");
    System.out.println("2. Reserve Book");
    System.out.println("3. Add Review");
    System.out.println("4. Generate Reports");
    System.out.println("5. Exit");
}

// Main method to run the application
public static void main(String[] args) {
    LibraryManagementSystem librarySystem = new LibraryManagementSystem();
    librarySystem.run();
}

// Run method to handle the application flow
public void run() {
    Scanner scanner = new Scanner(System.in);
    int choice;

    do {
        displayMenu();
        System.out.print("Enter your choice: ");
        choice = scanner.nextInt();
        scanner.nextLine(); // Consume newline

        switch (choice) {
            case 1:
                searchBooks(scanner);
                break;
            case 2:
                reserveBook(scanner);
                break;
            case 3:
                addReview(scanner);
                break;
            case 4:
                generateReports();
                break;
            case 5:
                System.out.println("Exiting...");
                break;
            default:
                System.out.println("Invalid choice. Please try again.");
        }
    } while (choice != 5);

    scanner.close();
}
``````java
// Additional methods for data persistence
public void importData() {
    // Logic to import data from files or database
}

public void exportData() {
    // Logic to export data to files or database
}

// User Interface methods
public void displayMenu() {
    System.out.println("1. Search Books");
    System.out.println("2. Reserve Book");
    System.out.println("3. Add Review");
    System.out.println("4. Generate Reports");
    System.out.println("5. Exit");
}

// Main method to run the application
public static void main(String[] args) {
    LibraryManagementSystem librarySystem = new LibraryManagementSystem();
    librarySystem.run();
}

// Run method to handle the application flow
public void run() {
    Scanner scanner = new Scanner(System.in);
    int choice;

    do {
        displayMenu();
        System.out.print("Enter your choice: ");
        choice = scanner.nextInt();
        scanner.nextLine(); // Consume newline

        switch (choice) {
            case 1:
                searchBooks(scanner);
                break;
            case 2:
                reserveBook(scanner);
                break;
            case 3:
                addReview(scanner);
                break;
            case 4:
                generateReports();
                break;
            case 5:
                System.out.println("Exiting...");
                break;
            default:
                System.out.println("Invalid choice. Please try again.");
        }
    } while (choice != 5);

    scanner.close();
}
``` ```java
// Additional methods for data persistence
public void importData() {
    // Logic to import data from files or database
}

public void exportData() {
    // Logic to export data to files or database
}

// User Interface methods
public void displayMenu() {
    System.out.println("1. Search Books");
    System.out.println("2. Reserve Book");
    System.out.println("3. Add Review");
    System.out.println("4. Generate Reports");
    System.out.println("5. Exit");
}

// Main method to run the application
public static void main(String[] args) {
    LibraryManagementSystem librarySystem = new LibraryManagementSystem();
    librarySystem.run();
}

// Run method to handle the application flow
public void run() {
    Scanner scanner = new Scanner(System.in);
    int choice;

    do {
        displayMenu();
        System.out.print("Enter your choice: ");
        choice = scanner.nextInt();
        scanner.nextLine(); // Consume newline

        switch (choice) {
            case 1:
                searchBooks(scanner);
                break;
            case 2:
                reserveBook(scanner);
                break;
            case 3:
                addReview(scanner);
                break;
            case 4:
                generateReports();
                break;
            case 5:
                System.out.println("Exiting...");
                break;
            default:
                System.out.println("Invalid choice. Please try again.");
        }
    } while (choice != 5);

    scanner.close();
}
