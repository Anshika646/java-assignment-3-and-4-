# java-assignment-3
import java.util.*; 

class InvalidMarksException extends Exception {        
    InvalidMarksException() {                          
        System.out.println("Invalid Marks Exception Occurred");  
    }
}
class Student {
    int roll;
    String name;
    int[] marks = new int[3];
    Student(int r, String n, int[] m) {  
        roll = r;                         
        name = n;                         
        marks = m;                        
    }
    void validateMarks() throws InvalidMarksException {  
        for(int i=0; i<3; i++) {                          
            if(marks[i] < 0 || marks[i] > 100) {          
                throw new InvalidMarksException();        
            }
        }
    }
    double average() {           
        int sum = 0;             
        for(int x : marks) {     
            sum += x;
        }
        return sum / 3.0;        
    }
    void show() {                                           
        System.out.println("Roll: " + roll);                
        System.out.println("Name: " + name);                
        System.out.println("Marks: " + marks[0] + " " + marks[1] + " " + marks[2]); 
        double avg = average();                             
        System.out.println("Average: " + avg);              
        if(avg >= 40) System.out.println("Result: Pass");   
        else System.out.println("Result: Fail");
    }
}

public class ResultManager {
    Scanner sc = new Scanner(System.in);       
    Student[] list = new Student[10];          
    int count = 0;                              
    void addStudent() {                          
        try {
            System.out.print("Enter Roll: ");    
            int r = sc.nextInt();
            sc.nextLine();                       
            System.out.print("Enter Name: ");    
            String n = sc.nextLine();
            int[] m = new int[3];                
            for(int i=0; i<3; i++) {
                System.out.print("Marks " + (i+1) + ": ");  
                m[i] = sc.nextInt();
            }
            Student s = new Student(r, n, m);    
            s.validateMarks();                   
            list[count] = s;                      
            count++;                              
            System.out.println("Student Added Successfully.");
        } catch(InvalidMarksException e) {        
            System.out.println("Please enter valid marks.");
        }
        catch(InputMismatchException e) {         
            System.out.println("Invalid Input Type.");
            sc.nextLine();                        
        }
        catch(Exception e) {                      
            System.out.println("Some Error Occurred.");
        }
        finally {
            System.out.println("Returning to Main Menu..."); 
        }
    }
    void showDetails() {                           
        try {
            System.out.print("Enter Roll to Search: ");
            int r = sc.nextInt();
            boolean found = false;                
            for(int i=0; i<count; i++) {
                if(list[i].roll == r) {           
                    list[i].show();               
                    found = true;
                    break;
                }
            }
            if(!found) System.out.println("Student Not Found.");  
        } catch(Exception e) {
            System.out.println("Error while Showing Details.");
        }
        finally {
            System.out.println("Search Finished.");    
        }
    }
    void menu() {                                    
        int ch;
        do {                                         
            System.out.println("====== Student Result System ======");
            System.out.println("1. Add Student");
            System.out.println("2. Show Student Details");
            System.out.println("3. Exit");
            System.out.print("Enter Choice: ");
            ch = sc.nextInt();
            if(ch == 1) addStudent();
            else if(ch == 2) showDetails();
            else if(ch == 3) System.out.println("Exiting... Thank You!");
            else System.out.println("Invalid Choice.");
        } while(ch != 3);     
    }
    public static void main(String[] args) {          
        ResultManager rm = new ResultManager();  

        
ASSINGMENT 4 
        import java.io.*;
import java.util.*;

// ---------------- BOOK CLASS ----------------
class Book implements Comparable {
    int bookId;
    String title;
    String author;
    String category;
    boolean isIssued;

    Book(int id, String t, String a, String c) {
        bookId = id;
        title = t;
        author = a;
        category = c;
        isIssued = false;
    }

    public void displayBookDetails() {
        System.out.println("ID: " + bookId);
        System.out.println("Title: " + title);
        System.out.println("Author: " + author);
        System.out.println("Category: " + category);
        System.out.println("Issued: " + isIssued);
        System.out.println("----------------------");
    }

    public void markAsIssued() {
        isIssued = true;
    }

    public void markAsReturned() {
        isIssued = false;
    }

    public int compareTo(Object o) {
        Book b = (Book) o;
        return title.compareToIgnoreCase(b.title);
    }
}

// ---------------- MEMBER CLASS ----------------
class Member {
    int memberId;
    String name;
    String email;

    ArrayList issuedBooks = new ArrayList(); // raw list

    Member(int id, String n, String e) {
        memberId = id;
        name = n;
        email = e;
    }

    public void displayMemberDetails() {
        System.out.println("Member ID: " + memberId);
        System.out.println("Name: " + name);
        System.out.println("Email: " + email);
        System.out.println("Issued Books: " + issuedBooks);
        System.out.println("----------------------");
    }

    public void addIssuedBook(int id) {
        issuedBooks.add(id);
    }

    public void returnIssuedBook(int id) {
        issuedBooks.remove((Integer) id);
    }
}

// ---------------- LIBRARY MANAGER ----------------
public class LibraryManager {
    HashMap books = new HashMap();   // raw map
    HashMap members = new HashMap(); // raw map
    Scanner sc = new Scanner(System.in);
    // --------- ADD BOOK ----------
    public void addBook() {
        try {
            System.out.print("Enter Book ID: ");
            int id = Integer.parseInt(sc.nextLine());
            System.out.print("Enter Title: ");
            String t = sc.nextLine();
            System.out.print("Enter Author: ");
            String a = sc.nextLine();

            System.out.print("Enter Category: ");
            String c = sc.nextLine();

            Book b = new Book(id, t, a, c);
            books.put(id, b);

            saveBooksToFile();
            System.out.println("Book Added.\n");

        } catch (Exception e) {
            System.out.println("Error");
        }
    }

    // ---------- ADD MEMBER ----------
    public void addMember() {
        try {
            System.out.print("Enter Member ID: ");
            int id = Integer.parseInt(sc.nextLine());

            System.out.print("Enter Name: ");
            String n = sc.nextLine();

            System.out.print("Enter Email: ");
            String e = sc.nextLine();

            Member m = new Member(id, n, e);
            members.put(id, m);

            saveMembersToFile();
            System.out.println("Member Added.\n");

        } catch (Exception e) {
            System.out.println("Error");
        }
    }

    // ---------- ISSUE BOOK ----------
    public void issueBook() {
        System.out.print("Enter Book ID: ");
        int bid = Integer.parseInt(sc.nextLine());

        System.out.print("Enter Member ID: ");
        int mid = Integer.parseInt(sc.nextLine());

        if (!books.containsKey(bid)) {
            System.out.println("Book not found.");
            return;
        }
        if (!members.containsKey(mid)) {
            System.out.println("Member not found.");
            return;
        }

        Book b = (Book) books.get(bid);
        Member m = (Member) members.get(mid);

        if (b.isIssued) {
            System.out.println("Book already issued.");
            return;
        }

        b.markAsIssued();
        m.addIssuedBook(bid);

        saveBooksToFile();
        saveMembersToFile();
        System.out.println("Book Issued.\n");
    }

    // ---------- RETURN BOOK ----------
    public void returnBook() {
        System.out.print("Enter Book ID: ");
        int bid = Integer.parseInt(sc.nextLine());

        System.out.print("Enter Member ID: ");
        int mid = Integer.parseInt(sc.nextLine());

        if (!books.containsKey(bid) || !members.containsKey(mid)) {
            System.out.println("Invalid IDs");
            return;
        }
        Book b = (Book) books.get(bid);
        Member m = (Member) members.get(mid);
        b.markAsReturned();
        m.returnIssuedBook(bid);
        saveBooksToFile();
        saveMembersToFile();
        System.out.println("Book Returned.\n");
    }
    public void searchBooks() {
    System.out.print("Enter keyword: ");
    String k = sc.nextLine().toLowerCase();
    for (Object o : books.values()) {
        Book b = (Book) o;
        if (b.title.toLowerCase().contains(k) ||
            b.author.toLowerCase().contains(k) ||
            b.category.toLowerCase().contains(k)) {
            b.displayBookDetails();
        }
    }
}
// SORT
public void sortBooks() {
    ArrayList list = new ArrayList(books.values());
    Collections.sort(list);
    for (Object o : list) ((Book)o).displayBookDetails();
}

// SAVE BOOKS
public void saveBooksToFile() {
    try {
        BufferedWriter bw = new BufferedWriter(new FileWriter("books.txt"));
        for (Object o : books.values()) {
            Book b = (Book) o;
            bw.write(b.bookId+","+b.title+","+b.author+","+b.category+","+b.isIssued);
            bw.newLine();
        }
        bw.close();
    } catch (Exception e) { System.out.println("File error"); }
}

// SAVE MEMBERS
public void saveMembersToFile() {
    try {
        BufferedWriter bw = new BufferedWriter(new FileWriter("members.txt"));
        for (Object o : members.values()) {
            Member m = (Member) o;
            bw.write(m.memberId+","+m.name+","+m.email+","+m.issuedBooks);
            bw.newLine();
        }
        bw.close();
    } catch (Exception e) { System.out.println("File error"); }
}

public void menu() {
    int ch = 0;
    while (ch != 7) {
        System.out.println("1.Add Book\n2.Add Member\n3.Issue\n4.Return\n5.Search\n6.Sort\n7.Exit");
        ch = Integer.parseInt(sc.nextLine());
        if (ch==1) addBook();
        else if (ch==2) addMember();
        else if (ch==3) issueBook();
        else if (ch==4) returnBook();
        else if (ch==5) searchBooks();
        else if (ch==6) sortBooks();
    }
}
public static void main(String[] args) {
    new LibraryManager().menu();
}
        rm.menu();    
        ASSIGNMENT 4  
         p
