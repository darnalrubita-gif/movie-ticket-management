import java.io.*; import java.util.*; 
 
// Interface for Polymorphism interface BookingOperations {     void bookTicket(int numTickets); 
} 
 
// Parent Class - Data Hiding & Encapsulation class Movie {     private String movieName;     private String showTime;     private int seatsAvailable;     private double price; 
 
    public Movie(String movieName, String showTime, int seatsAvailable, double price) {         this.movieName = movieName;         this.showTime = showTime;         this.seatsAvailable = seatsAvailable;         this.price = price; 
    } 
 
    // Getters and Setters (Encapsulation)     public String getMovieName() { return movieName; }     public int getSeatsAvailable() { return seatsAvailable; }     public void setSeatsAvailable(int seatsAvailable) { this.seatsAvailable = seatsAvailable; }     public double getPrice() { return price; }     public String getShowTime() { return showTime; } 
 
    @Override     public String toString() {         return movieName + " | " + showTime + " | Seats: " + seatsAvailable + " | Price: $" + price; 
    } 
} 
 
// Child Class - Inheritance class Ticket extends Movie implements BookingOperations {     private int bookingId;     private int bookedSeats; 
 
    public Ticket(Movie movie, int bookedSeats, int id) { 
        super(movie.getMovieName(), movie.getShowTime(), movie.getSeatsAvailable(), movie.getPrice());         this.bookedSeats = bookedSeats;         this.bookingId = id; 
    } 
 
    // Overriding Method - Polymorphism 
    @Override     public void bookTicket(int numTickets) {         if (numTickets > getSeatsAvailable()) { 
            System.out.println("Error: Not enough seats!"); 
            return; 
        } 
        setSeatsAvailable(getSeatsAvailable() - numTickets); 
        System.out.println("Booking Successful! ID: " + bookingId); 
        System.out.println("Total: $" + (numTickets * getPrice())); 
    } 
     
    public String getReceipt() { 
        return "ID: " + bookingId + " | Movie: " + getMovieName() + " | Seats: " + bookedSeats + "\n"; 
    } 
} 
 
public class MovieBookingApp {     static List<Movie> movieList = new ArrayList<>();     static Scanner sc = new Scanner(System.in); 
 
    public static void main(String[] args) { 
        loadMovies();         int choice;         do { 
            System.out.println("\n--- Movie Ticket System ---"); 
            System.out.println("1. View Movies\n2. Book Ticket\n3. Exit");             choice = sc.nextInt();             switch (choice) {                 case 1: displayMovies(); break;                 case 2: performBooking(); break; 
            } 
        } while (choice != 3);         saveBookings(); 
    } 
 
    // Exception Handling & File I/O     static void loadMovies() {         try (BufferedReader br = new BufferedReader(new FileReader("movies.txt"))) { 
            String line;             while ((line = br.readLine()) != null) {                 String[] data = line.split(","); 
                movieList.add(new Movie(data[0], data[1], Integer.parseInt(data[2]), Double.parseDouble(data[3]))); 
            } 
        } catch (IOException e) { 
            System.out.println("Error loading movies: " + e.getMessage()); 
        } 
    } 
 
    static void displayMovies() {         for (int i = 0; i < movieList.size(); i++) { 
            System.out.println((i + 1) + ". " + movieList.get(i)); 
        } 
    } 
 
    static void performBooking() {         displayMovies(); 
        System.out.print("Select Movie (Number): ");         int mIdx = sc.nextInt() - 1;         System.out.print("Enter Seats: ");         int seats = sc.nextInt(); 
 
        Ticket t = new Ticket(movieList.get(mIdx), seats, new Random().nextInt(1000));         t.bookTicket(seats); 
         
        // Save to File         try (FileWriter fw = new FileWriter("bookings.txt", true)) {             fw.write(t.getReceipt());         } catch (IOException e) { 
            System.out.println("Booking file error: " + e.getMessage()); 
        } 
    } 
     
    static void saveBookings() { 
        // Method to save final seat updates back to movies.txt 
    } 
} 
