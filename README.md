import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Scanner;

class Room {
    private int roomNumber;
    private String roomType;
    private double price;
    private boolean isAvailable;

    public Room(int roomNumber, String roomType, double price) {
        this.roomNumber = roomNumber;
        this.roomType = roomType;
        this.price = price;
        this.isAvailable = true;
    }

    public int getRoomNumber() {
        return roomNumber;
    }

    public String getRoomType() {
        return roomType;
    }

    public double getPrice() {
        return price;
    }

    public boolean isAvailable() {
        return isAvailable;
    }

    public void setAvailable(boolean available) {
        isAvailable = available;
    }
}

class Hotel {
    private List<Room> rooms;
    private Map<Integer, String> bookings;

    public Hotel() {
        rooms = new ArrayList<>();
        bookings = new HashMap<>();
    }

    public void addRoom(Room room) {
        rooms.add(room);
    }

    public List<Room> searchAvailableRooms() {
        List<Room> availableRooms = new ArrayList<>();
        for (Room room : rooms) {
            if (room.isAvailable()) {
                availableRooms.add(room);
            }
        }
        return availableRooms;
    }

    public void makeReservation(int roomNumber, String guestName) {
        for (Room room : rooms) {
            if (room.getRoomNumber() == roomNumber && room.isAvailable()) {
                room.setAvailable(false);
                bookings.put(roomNumber, guestName);
                System.out.println("Reservation confirmed for " + guestName + " in room " + roomNumber + ".");
                return;
            }
        }
        System.out.println("Room not available or does not exist.");
    }

    public void viewBookingDetails() {
        for (Map.Entry<Integer, String> entry : bookings.entrySet()) {
            System.out.println("Room Number: " + entry.getKey() + ", Guest Name: " + entry.getValue());
        }
    }
}

public class HotelReservationSystem {
    public static void main(String[] args) {
        Hotel hotel = new Hotel();

        // Add rooms to the hotel
        hotel.addRoom(new Room(101, "Single", 100));
        hotel.addRoom(new Room(102, "Double", 150));
        hotel.addRoom(new Room(103, "Suite", 250));

        Scanner scanner = new Scanner(System.in);

        while (true) {
            System.out.println("\n1. Search Available Rooms");
            System.out.println("2. Make a Reservation");
            System.out.println("3. View Booking Details");
            System.out.println("4. Exit");
            System.out.print("Choose an option: ");
            String choice = scanner.nextLine();

            switch (choice) {
                case "1":
                    List<Room> availableRooms = hotel.searchAvailableRooms();
                    if (!availableRooms.isEmpty()) {
                        System.out.println("Available Rooms:");
                        for (Room room : availableRooms) {
                            System.out.printf("Room Number: %d, Type: %s, Price: $%.2f, Available: %b%n",
                                    room.getRoomNumber(), room.getRoomType(), room.getPrice(), room.isAvailable());
                        }
                    } else {
                        System.out.println("No rooms available.");
                    }
                    break;

                case "2":
                    System.out.print("Enter room number to reserve: ");
                    int roomNumber = Integer.parseInt(scanner.nextLine());
                    System.out.print("Enter your name: ");
                    String guestName = scanner.nextLine();
                    hotel.makeReservation(roomNumber, guestName);
                    break;

                case "3":
                    System.out.println("Booking Details:");
                    hotel.viewBookingDetails();
                    break;

                case "4":
                    System.out.println("Thank you for using the hotel reservation system.");
                    scanner.close();
                    return;

                default:
                    System.out.println("Invalid choice, please try again.");
            }
        }
    }
}

