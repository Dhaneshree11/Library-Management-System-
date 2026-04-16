

import java.util.Scanner;


class Book {
    private int serialNo;
    private String bookName;
    private String authorName;
    private int totalQty;
    private int availableQty;

    // Setters with validation
    public void setSerialNo(int serialNo) {
        if (serialNo > 0) this.serialNo = serialNo;
        else System.out.println("Serial number must be positive!");
    }

    public int getSerialNo() {
        return serialNo;
    }

    public void setBookName(String bookName) {
        if (bookName != null && !bookName.isEmpty())
            this.bookName = bookName;
        else System.out.println("Book name cannot be empty!");
    }

    public String getBookName() {
        return bookName;
    }

    public void setAuthorName(String authorName) {
        if (authorName != null && !authorName.isEmpty())
            this.authorName = authorName;
        else System.out.println("Author name cannot be empty!");
    }

    public String getAuthorName() {
        return authorName;
    }

    public void setTotalQty(int qty) {
        if (qty > 0) this.totalQty = qty;
        else System.out.println("Quantity must be positive!");
    }

    public int getTotalQty() {
        return totalQty;
    }

    public void setAvailableQty(int qty) {
        if (qty >= 0) this.availableQty = qty;
    }

    public int getAvailableQty() {
        return availableQty;
    }
}

// -------------------- LIBRARY CLASS --------------------
class Library {
    Book[] books = new Book[50];
    int count = 0;
    Scanner sc = new Scanner(System.in);

    // Add Book
    public void addBook(Book b) {

        for (int i = 0; i < count; i++) {
            if (b.getSerialNo() == books[i].getSerialNo() ||
                b.getBookName().equalsIgnoreCase(books[i].getBookName())) {
                System.out.println("Book already exists!");
                return;
            }
        }

        if (count < 50) {
            books[count] = b;
            count++;
            System.out.println("Book added successfully!");
        } else {
            System.out.println("Library is full!");
        }
    }

    // Show all books
    public void showAllBooks() {
        if (count == 0) {
            System.out.println("No books available.");
            return;
        }

        System.out.println("SNo\tName\tAuthor\tTotal\tAvailable");
        for (int i = 0; i < count; i++) {
            printBook(books[i]);
        }
    }

    // Search by Serial Number
    public void searchBySno() {
        System.out.print("Enter Serial No: ");
        int sno = sc.nextInt();

        for (int i = 0; i < count; i++) {
            if (sno == books[i].getSerialNo()) {
                printBook(books[i]);
                return;
            }
        }
        System.out.println("Book not found.");
    }

    // Search by Author
    public void searchByAuthor() {
        sc.nextLine();
        System.out.print("Enter Author Name: ");
        String name = sc.nextLine();

        boolean found = false;

        for (int i = 0; i < count; i++) {
            if (name.equalsIgnoreCase(books[i].getAuthorName())) {
                printBook(books[i]);
                found = true;
            }
        }

        if (!found) System.out.println("No books found.");
    }

    // Upgrade Quantity
    public void upgradeQty() {
        System.out.print("Enter Serial No: ");
        int sno = sc.nextInt();

        for (int i = 0; i < count; i++) {
            if (sno == books[i].getSerialNo()) {
                System.out.print("Enter quantity to add: ");
                int qty = sc.nextInt();

                books[i].setTotalQty(books[i].getTotalQty() + qty);
                books[i].setAvailableQty(books[i].getAvailableQty() + qty);

                System.out.println("Quantity updated!");
                return;
            }
        }

        System.out.println("Book not found.");
    }

    // Check availability
    public int isAvailable(int sno) {
        for (int i = 0; i < count; i++) {
            if (sno == books[i].getSerialNo()) {
                if (books[i].getAvailableQty() > 0)
                    return i;
                else {
                    System.out.println("Book not available.");
                    return -1;
                }
            }
        }
        System.out.println("Book not found.");
        return -1;
    }

    // Checkout
    public Book checkOutBook() {
        System.out.print("Enter Serial No: ");
        int sno = sc.nextInt();

        int index = isAvailable(sno);
        if (index != -1) {
            books[index].setAvailableQty(books[index].getAvailableQty() - 1);
            System.out.println("Book issued!");
            return books[index];
        }
        return null;
    }

    // Return
    public void returnBook(Book b) {
        for (int i = 0; i < count; i++) {
            if (b.getSerialNo() == books[i].getSerialNo()) {
                books[i].setAvailableQty(books[i].getAvailableQty() + 1);
                System.out.println("Book returned!");
                return;
            }
        }
    }

    private void printBook(Book b) {
        System.out.println(b.getSerialNo() + "\t" +
                b.getBookName() + "\t" +
                b.getAuthorName() + "\t" +
                b.getTotalQty() + "\t" +
                b.getAvailableQty());
    }

    // Menu
    public void menu() {
        System.out.println("\n1. Add Book");
        System.out.println("2. Upgrade Quantity");
        System.out.println("3. Search Book");
        System.out.println("4. Show All Books");
        System.out.println("5. Issue Book");
        System.out.println("6. Return Book");
        System.out.println("0. Exit");
    }
}


public class LibraryManagementSystem {

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        Library lib = new Library();
        Book issuedBook = null;

        int choice;

        do {
            lib.menu();
            choice = sc.nextInt();

            switch (choice) {

                case 1:
                    Book b = new Book();

                    System.out.print("Enter Serial No: ");
                    b.setSerialNo(sc.nextInt());
                    sc.nextLine();

                    System.out.print("Enter Book Name: ");
                    b.setBookName(sc.nextLine());

                    System.out.print("Enter Author Name: ");
                    b.setAuthorName(sc.nextLine());

                    System.out.print("Enter Quantity: ");
                    int qty = sc.nextInt();
                    b.setTotalQty(qty);
                    b.setAvailableQty(qty);

                    lib.addBook(b);
                    break;

                case 2:
                    lib.upgradeQty();
                    break;

                case 3:
                    System.out.println("1. By Serial No");
                    System.out.println("2. By Author");
                    int ch = sc.nextInt();

                    if (ch == 1) lib.searchBySno();
                    else lib.searchByAuthor();
                    break;

                case 4:
                    lib.showAllBooks();
                    break;

                case 5:
                    issuedBook = lib.checkOutBook();
                    break;

                case 6:
                    if (issuedBook != null)
                        lib.returnBook(issuedBook);
                    else
                        System.out.println("No book to return.");
                    break;

                case 0:
                    System.out.println("Exiting...");
                    break;

                default:
                    System.out.println("Invalid choice!");
            }

        } while (choice != 0);
    }
}**
