# parking-space-dynamicallty
#include <stdio.h>
#include <string.h>

// Constants defined for better understanding

// Maximum number of parking spaces available
#define MAX_PARKING_SPACES 50  
//maximum numbwer of vehciles  
#define MAX_VEHICLES 500 
// Indicates that a parking space is free       
#define FREE 0  
// Indicates that a parking space is occupied                 
#define OCCUPIED 1  
// Minimum hours needed to achieve Gold membership             
#define GOLDEN_HOURS 200     
// Minimum hours needed to achieve Premium membership    
#define PREMIUM_HOURS 100 
// Gold membership identifier       
#define GOLD 2 
// premium membership identifier                  
#define PREMIUM 1  
 // No membership identifier              
#define NONE 0  
// Base parking fee for the first 3 hours                
#define BASE_FEES 100     
// Extra fee for each additional hour after 3 hours of parking   
#define EXTRA_FEES 50  .
// 10% discount for members having the membership          
#define DISCOUNT 0.10            

// Structure to store vehicle details
struct vehicle {
    char vehicle_num[10];         // Vehicle number
    char owner_name[20];          // Owner's name
    struct arrival {
    int time;            // Arrival time when the vehicle is parked
      int date;
      int month;
      int year ;}arrival;
   struct departure {
    int time;            // Departure time when the vehicle leaves
      int date;
      int month;
      int year ; } departure;                 
                          
    int membership;               // Membership status: Gold, Premium, or None
    int total_parking_hours;      // Total hours the vehicle has been parked over time
    int parking_ID;               // ID of the parking space allocated to the vehicle
    int total_amount_paid;        // Total amount the vehicle owner has paid for parking
    int parking_count;            // Total number of times the vehicle has been parked
};

// Structure to store parking space details
struct parkinginfo {
    int parking_space_ID;         // Unique ID for the parking space
    int status;                   // Status of the parking space (Free/Occupied)
    int occupancy_count;          // Number of times the parking space has been used
    int space_revenue;            // Total revenue generated from this parking space
};

//  variable to keep track of the total number of vehicles registered it is a global variable
int vehicle_count = 0;  

// Function to initialize all parking spaces to 'Free' and set other attributes to zero
void initialize_parking_spaces(struct parkinginfo parkingspots[]) {
    for (int i = 0; i < MAX_PARKING_SPACES; i++) {
        parkingspots[i].parking_space_ID = i + 1;  // Assign unique parking ID starting from 1 to 50
        parkingspots[i].status = FREE;             // Initially, all spaces are free
        parkingspots[i].occupancy_count = 0;       // No vehicle has occupied the spot till now
        parkingspots[i].space_revenue = 0;         // No revenue has been generated yet
    }
}

// Function to initialize all vehicle details
void initialize_vehicle_details(struct vehicle vehicles[]) {
    for (int j = 0; j < MAX_VEHICLES; j++) {
        vehicles[j].vehicle_num[0] = '\0';          // Initialize vehicle number to empty
        vehicles[j].owner_name[0] = '\0';           // Initialize owner's name to empty
        vehicles[j].arrival.time = 0;           // Set initial arrival time to 0
        vehicles[j].arrival.date = 0;
        vehicles[j].arrival.month = 0;
        vehicles[j].arrival.year = 0;          
         vehicles[j].departure.time = 0;           // Set initial departure time to 0
        vehicles[j].departure.date = 0;
        vehicles[j].departure.month = 0;
        vehicles[j].departure.year = 0;                           
      
        vehicles[j].membership = NONE;              // Initially, no membership assigned
        vehicles[j].total_parking_hours = 0;        // Total parking hours set to 0
        vehicles[j].parking_ID=0;                      // No parking space assigned initially
        vehicles[j].total_amount_paid = 0;          // Total amount paid set to 0
        vehicles[j].parking_count = 0;              // No parkings done initially
    }
}

// Function to check if a vehicle is already registered
int check_registered(char vehicle_num[], struct vehicle vehicles[]) {
    // Loop through all registered vehicles to find a match by vehicle number.
        int index=-1;
    for (int i = 0; i < vehicle_count; i++) {
        if (strcmp(vehicles[i].vehicle_num,vehicle_num) == 0) {
           
            index=i; // Return the vehicle if found
        }
    }
    return index; // Return -1 if the vehicle is not registered 
}

// Function to check if a parking space is available based on membership 
int is_parking_available(struct parkinginfo parkingspots[], int membership) {
    // Golden membership users get priority to park in spaces 1-10
    if (membership == GOLD) {
        for (int i = 0; i < 10; i++) {
            if (parkingspots[i].status == FREE) {
                return parkingspots[i].parking_space_ID;
            }
        }
    }
    // Premium membership users first gets the priority to park in spaces 11 between 20
    else if (membership == PREMIUM) {
        for (int i = 10; i < 20; i++) {
            if (parkingspots[i].status == FREE) {
                return parkingspots[i].parking_space_ID;
            }
        }
    }
    // Non-members get spaces from 21-50
    else {
        for (int i = 20; i < MAX_PARKING_SPACES; i++) {
            if (parkingspots[i].status == FREE) {
                return parkingspots[i].parking_space_ID;
            }
        }
    }
    return -1; // Return -1 if no parking space is available
}

// Function to park a vehicle
void park_vehicle(struct parkinginfo parkingspots[], struct vehicle vehicles[]) {
    char vehicle_num[10], owner_name[20];
    int arrival_time;
    int arrival_date;
    int arrival_month;
    int arrival_year;

    // Get user input for vehicle number and arrival time
    printf("Enter vehicle number: ");
    scanf("%s", vehicle_num);
    printf("Enter arrival time: ");
    scanf("%d", &arrival_time);

    printf("Enter arrival date: ");
    scanf("%d", &arrival_date);
    printf("Enter arrival month: ");
    scanf("%d", &arrival_month);
    printf("Enter arrival year: ");
    scanf("%d", &arrival_year);
    
    // Check if the vehicle is already registered
    int vehicle_index = check_registered(vehicle_num, vehicles);
    
    if (vehicle_index==-1) {
        // the vehcile is not registered, enter yhe owner's name and register the vehicle
        printf("Enter owner's name: ");
        scanf("%s", owner_name);
        
        // Store new vehicle details
        strcpy(vehicles[vehicle_count].vehicle_num,vehicle_num);
        strcpy(vehicles[vehicle_count].owner_name, owner_name);
        vehicles[vehicle_count].arrival.time= arrival_time;
        vehicles[vehicle_count].arrival.date = arrival_date;
        vehicles[vehicle_count].arrival.month = arrival_month;
        vehicles[vehicle_count].arrival.year= arrival_year;
        vehicles[vehicle_count].total_parking_hours = 0;
        vehicles[vehicle_count].membership = NONE;  // Default to no membership
        vehicles[vehicle_count].parking_count = 0;
        vehicles[vehicle_count].total_amount_paid = 0;
     vehicle_index=vehicle_count; 
        vehicle_count++;  // Increment vehicle count
         } 
         else {
        // If already registered, just update the arrival time
        vehicles[vehicle_index].arrival.time = arrival_time;
        vehicles[vehicle_index].arrival.date=arrival_date;
        vehicles[vehicle_index].arrival.month=arrival_month;
        vehicles[vehicle_index].arrival.year=arrival_year;
    }

    // Allocate a parking space based on membership
    int parking_space_ID = is_parking_available(parkingspots, vehicles[vehicle_index].membership);
    if (parking_space_ID != -1) {
        // Park the vehicle in the allocated space
        vehicles[vehicle_index].parking_ID = parking_space_ID;
        parkingspots[parking_space_ID - 1].status = OCCUPIED;
        parkingspots[parking_space_ID - 1].occupancy_count++;
        printf("Vehicle parked at space %d\n", parking_space_ID);
    } else {
        printf("Parking lot is full.\n");
    }
}

// Function to calculate parking fee based on the number of hours parked
int calculate_parking_fee(int hours_parked, int membership) {
    int fee = BASE_FEES;  // Start with the base fee for up to 3 hours
    if (hours_parked > 3) {
        fee += (hours_parked - 3) *50 ;  // Add extra fee for additional hours
    }
    if (membership > 0) {  // Apply discount if the vehicle has a membership
        fee -= (fee * DISCOUNT);
    }
    return fee;
}


int days_in_month(int month) {
    int days[] = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
    return days[month - 1];
}

// Function to calculate the total number of days from a date
int totaldays(int date, int month, int year) {
    int days = 0;

    // Add days for each completed year
    for (int i = 1990; i < year; i++) {
        days += 365;
    }

    // Add days for each completed month in the current year
    for (int i = 1; i < month; i++) {
        days += days_in_month(i);
    }

    // Add the days of the current month
    days += date;

    return days;
}

// Function to calculate the total number of days between two dates
int date_difference(int date1, int month1, int year1, int date2, int month2, int year2) {
    int days1 = totaldays(date1, month1, year1);
    int days2 = totaldays(date2, month2, year2);

    return days2 - days1;
}

// Function to calculate the total hours parked, considering date and time
int hours_parked(int arrival_time, int arrival_date, int arrival_month, int arrival_year,
                 int departure_time, int departure_date, int departure_month, int departure_year) {
    
    // Calculate total day difference
    int total_days_difference = date_difference(arrival_date,arrival_month,arrival_year, 
                                                departure_date,departure_month,departure_year);

    // Calculate total hours difference
    int total_hours = total_days_difference * 24 + (departure_time - arrival_time);
    if(total_hours<0){
    total_hours+=24;}
    return total_hours;
}


void exit_vehicle(struct parkinginfo parkingspots[], struct vehicle vehicles[]) {
    char vehicle_num[10];
    int departure_time;
    int departure_date;
    int departure_month;
    int departure_year;

    // Get user input for vehicle number and departure time
    printf("Enter vehicle number: ");
    scanf("%s", vehicle_num);
    printf("Enter departure time: ");
    scanf("%d", &departure_time);
    printf("Enter departure date: ");
    scanf("%d", &departure_date);
    printf("Enter departure month: ");
    scanf("%d", &departure_month);
    printf("Enter departure year: ");
    scanf("%d", &departure_year);
  
  
    // finding whether the vehicle is present in the database or not by vehicle number
   int vehicle_index = check_registered(vehicle_num,vehicles);
    if (vehicle_index!=-1) {
                
        // funtion called to calculate the number of hours
        int parked_hours = hours_parked(vehicles[vehicle_index].arrival.time, vehicles[vehicle_index].arrival.date, vehicles[vehicle_index].arrival.month, vehicles[vehicle_index].arrival.year,departure_time,departure_date,departure_month,departure_year);
        
        vehicles[vehicle_index].total_parking_hours += parked_hours;

        // Calculate the fee and update the total amount paid
        int fee = calculate_parking_fee(parked_hours, vehicles[vehicle_index].membership);
      vehicles[vehicle_index].total_amount_paid += fee;
        vehicles[vehicle_index].parking_count++;

        // Free the parking space and update the space's revenue
        int parking_space_ID =vehicles[vehicle_index].parking_ID;
        parkingspots[parking_space_ID - 1].status = FREE;
        parkingspots[parking_space_ID - 1].space_revenue += fee;

        // Update membership based on total parking hours
        if (vehicles[vehicle_index].total_parking_hours >= GOLDEN_HOURS) {
           vehicles[vehicle_index].membership = GOLD;
        }
         else if (vehicles[vehicle_index].total_parking_hours >= PREMIUM_HOURS) {
         vehicles[vehicle_index].membership = PREMIUM;
        }
         else {
           vehicles[vehicle_index].membership = NONE;
        }

        printf("Vehicle exited \n Hours parked: %d\n, Fee: %d Rs\n", parked_hours, fee);
        printf("the total parking hours:%d\n",vehicles[vehicle_index].total_parking_hours);
        printf("vehcile membership is :%d\n",vehicles[vehicle_index].membership);
        
    } else {
        printf("Vehicle not found.\n");
    }
}
// Function to sort vehicles by the total number of parkings
void sort_by_total_parkings(struct vehicle vehicles[]) {
    for (int i = 0; i < vehicle_count - 1; i++) {
        for (int j = i + 1; j < vehicle_count; j++) {
            if (vehicles[i].parking_count < vehicles[j].parking_count) {
                struct vehicle temp = vehicles[i];
                vehicles[i] = vehicles[j];
                vehicles[j] = temp;
            }
        }
    }
}

// Function to sort vehicles by the total amount paid
void sort_by_total_amount_paid(struct vehicle vehicles[]) {
    for (int i = 0; i < vehicle_count - 1; i++) {
        for (int j = i + 1; j < vehicle_count; j++) {
            if (vehicles[i].total_amount_paid < vehicles[j].total_amount_paid) {
                struct vehicle temp = vehicles[i];
                vehicles[i] = vehicles[j];
                vehicles[j] = temp;
            }
        }
    }
}

// Function to sort parking spaces by occupancy count
void sort_by_occupancy_count(struct parkinginfo parkingspots[]) {
    for (int i = 0; i < MAX_PARKING_SPACES - 1; i++) {
        for (int j = i + 1; j < MAX_PARKING_SPACES; j++) {
            if (parkingspots[i].occupancy_count < parkingspots[j].occupancy_count) {
                struct parkinginfo temp = parkingspots[i];
                parkingspots[i] = parkingspots[j];
                parkingspots[j] = temp;
            }
        }
    }
}

// Function to sort parking spaces by revenue generated by parking spaces
void sort_by_revenue_generated(struct parkinginfo parkingspots[]) {
    for (int i = 0; i < MAX_PARKING_SPACES - 1; i++) {
        for (int j = i + 1; j < MAX_PARKING_SPACES; j++) {
            if (parkingspots[i].space_revenue < parkingspots[j].space_revenue) {
                struct parkinginfo temp = parkingspots[i];
                parkingspots[i] = parkingspots[j];
                parkingspots[j] = temp;
            }
        }
    }
}

// Main function to demonstrate parking operations and sorting
int main() {
    struct parkinginfo parkingspots[MAX_PARKING_SPACES]; // Array to store parking space info
    struct vehicle vehicles[MAX_VEHICLES];              // Array to store vehicle info

    // Initialize parking spaces and vehicle details
    initialize_parking_spaces(parkingspots);
    initialize_vehicle_details(vehicles);

    // Sample simulation of vehicle entries and exits
    // User can also provide input to test the system functionality
    int choice;

    // Basic menu for user input
    do {
        printf("\n--- smart Parking system ---\n");
        printf("1. Park Vehicle\n");
        printf("2. Exit Vehicle\n");
        printf("3. Sort and Display Vehicles by Total Parkings\n");
        printf("4. Sort and Display Vehicles by Total Amount Paid\n");
        printf("5. Sort and Display Parking Spaces by Occupancy Count\n");
        printf("6. Sort and Display Parking Spaces by Revenue Generated\n");
        printf("0. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                park_vehicle(parkingspots, vehicles);  // Call to park a vehicle
                break;
            case 2:
                exit_vehicle(parkingspots, vehicles);  // Call to exit a vehicle
                break;
            case 3:
                sort_by_total_parkings(vehicles);  // Sort vehicles by total parkings
                printf("Vehicles sorted by total parkings:\n");
                for (int i = 0; i < vehicle_count; i++) {
                    printf("%s: %d parkings\n", vehicles[i].vehicle_num, vehicles[i].parking_count);
                }
                break;
            case 4:
                sort_by_total_amount_paid(vehicles);  // Sort vehicles by total amount paid
                printf("Vehicles sorted by total amount paid:\n");
                for (int i = 0; i < vehicle_count; i++) {
                    printf("%s: %d Rs paid\n", vehicles[i].vehicle_num, vehicles[i].total_amount_paid);
                }
                break;
            case 5:
                sort_by_occupancy_count(parkingspots);  // Sort parking spaces by occupancy count
                printf("Parking spaces sorted by occupancy count:\n");
                for (int i = 0; i < MAX_PARKING_SPACES; i++) {
                    printf("Space %d: %d times occupied\n", parkingspots[i].parking_space_ID, parkingspots[i].occupancy_count);
                }
                break;
            case 6:
                sort_by_revenue_generated(parkingspots);  // Sort parking spaces by revenue
                printf("Parking spaces sorted by revenue generated:\n");
                for (int i = 0; i < MAX_PARKING_SPACES; i++) {
                    printf("Space %d: %d Rs revenue\n", parkingspots[i].parking_space_ID, parkingspots[i].space_revenue);
                }
                break;
            case 0:
                printf("Exiting the system. Goodbye!\n");
                break;
            default:
                printf("Invalid choice, please try again.\n");
        }

    } while (choice != 0);  // Loop until the user chooses to exit

    return 0;}
