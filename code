#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <stdbool.h>
#include <ctype.h>
#include <conio.h>

// Define color codes for console output
#define red "\033[1;31m"
#define green "\033[1;32m"
#define yellow "\033[1;33m"
#define blue "\033[1;34m"
#define megenta "\033[1;35m"
#define cyan "\033[1;36m"
#define white "\033[1;37m"
#define reset "\033[0m\n"
#define bold "\033[1m"
#define italic "\033[3m"

typedef struct data
{
    // Structure to store passenger information
    char date[12];
    char origin[20];
    char destination[20];
    char airlines[15];
    char flight_no[10];
    char departure[10];
    char arrival[10];
    int price;
    char name[20];
    int age;
    char seat_no[3];
    int pnr;
    struct data *next;
} passenger;

typedef struct
{
    // Structure to represent seat arrangement
    char a;
    char b;
    char c;
    char d;
    char e;
    char f;
} Seat;

// Function prototypes
int main();
int display_flights(char destination[], char origin[]);
void select_flight(passenger* head);
void readFlightData(char* flightNo, passenger* passenger);
void seat_display();
void select_seat(passenger* head);
bool is_seat_occupied(int row, int col);
void update_seat(int row, int col);
void payment(passenger* head);
void generate_pnr(passenger*head);
void data_copy(passenger *p);
void Ticket_Display();
int dur(int originmin,int originhr);
void time_sort(char origin[], char destination[]);

int main() {
    // Welcome message
    printf(bold italic blue "\n\n\tWELCOME!"reset);
    printf(italic white"\n\nHow can we help you?\n\n"reset);

    // Menu options
    printf(cyan"1. Book Flight\n"reset);
    printf(cyan"2. Display Ticket\n"reset);
    printf(cyan"3. Exit\n\n"reset);

    // Get user's choice
    int choice;
    printf(italic white"Enter your choice: ");
    scanf("%d"reset, &choice);

    if (choice == 1) {
        // Book Ticket option

        // Get origin, destination, date, and number of passengers
        printf(italic yellow"\nSure!! Could you please help us with your Origin and Destination city?\n\n"reset);
        passenger* head, * p;
        char Origin[20];
        char Destination[20];
        char Date[12];
        int noOfpassengers;

        // Display city options
        printf(cyan"1. DELHI    (DEL)\n"reset);
        printf(cyan"2. MUMBAI   (BOM)\n"reset);
        printf(cyan"3. KOLKATA  (CCU)\n"reset);
        printf(cyan"4. CHENNAI  (MAA)\n\n"reset);

        // Get user input for origin, destination, date, and number of passengers
        printf(italic white"Select origin (Enter corresponding code): ");
        scanf("%s"reset, Origin);
        printf(italic white"\nSelect destination (Enter corresponding code): ");
        scanf("%s"reset, Destination);
        printf(italic white"\nEnter date (DD-MM-YYYY): ");
        scanf("%s"reset, Date);
        printf(italic white"\nNumber of passengers: ");
        scanf("%d"reset, &noOfpassengers);

        // Create passenger linked list
        for (int i = 0; i < noOfpassengers; i++) {
            if (i == 0) {
                head = (passenger*)malloc(sizeof(passenger));
                p = head;
            } else {
                p->next = (passenger*)malloc(sizeof(passenger));
                p = p->next;
            }
            strcpy(p->date, Date);
        }
        p->next = NULL;

        // Display available flights
        printf(italic yellow"\n\n\tList of all the flights from %s to %s on %s \n\n"reset, Origin, Destination, Date);
        int z=display_flights(Destination, Origin);

        char k[4];
        printf(italic white"\nDo you want me to display the available flights in sorted order according to departure time (yes/no)? ");
        scanf("%s"reset,k);
        if(strcmp(k,"yes")==0){
            printf(italic yellow"\nList of flights in sorted order according to departure time!\n"reset);
            time_sort(Origin,Destination);
        }
        // Select flight and seat
        printf(italic yellow"\nSelect flight"reset);
        select_flight(head);
        printf(italic yellow"\nSelect your desired seat from the list of available seats\n"reset);
        seat_display();
        select_seat(head);

        // Make payment
        payment(head);

        // Generate PNR
        printf(italic yellow"\nGenerating PNR........\n"reset);
        generate_pnr(head);

        // Display ticket option
        char ans[4];
        do {
            printf(italic white"\nWould you like to display the ticket (yes/no)? ");
            scanf("%s"reset,ans);
            if(strcmp(ans,"yes")==0) {
                Ticket_Display();
            }
            else if(strcmp(ans,"no")==0){
                printf(italic yellow"\nDirecting to home page.....\n"reset);
                main();
            }
            else{
                printf(italic red"\nEnter valid response!\n"reset);
            }
        } while (strcmp(ans,"yes")==0);
    } else if (choice == 2) {
        // Display Ticket option
        printf(italic yellow"\nSure! "reset);
        Ticket_Display();

        // Display more tickets option
        char ans[4];
        do {
            printf(italic white"\nWould you like to display more tickets (yes/no)? ");
            scanf("%s"reset,ans);
            if(strcmp(ans,"yes")==0) {
                Ticket_Display();
            }
            else {
                printf(italic yellow"\nDirecting to home page.....\n"reset);
                main();
            }
        } while (strcmp(ans,"yes")==0);
    } else {
        // Exit option
        printf(italic yellow"\nExiting...\n"reset);
        exit(1);
    }

    return 0;
}

int display_flights(char destination[], char origin[]) {
    // Open the CSV file for reading
    FILE *fp;
    fp = fopen("flight_data.csv", "r");
    if (fp == NULL) {
        printf("Failed to open the CSV file.\n");
        exit(1);
    }
    int numberOfflights=0;
    int line_no = 0;
    char *sp;
    char *ptr[7];
    char line[100];

    // Print flight data table header
    printf("%-19s %-15s %-12s %-12s %-12s %-12s %-10s\n",megenta"Flight No", "Airlines", "Origin", "Destination", "Departure", "Arrival", "Price"reset);
    printf(white"---------------------------------------------------------------------------------------\n"reset);

    while (fgets(line, 100, fp) != NULL) {
        line_no++;
        sp = strtok(line, ",");
        ptr[0] = sp;

        // Get flight data fields from CSV
        for (int i = 1; i <= 2; i++) {
            sp = strtok(NULL, ",");
            ptr[i] = sp;
        }

        // Check if origin and destination match
        if (strcmp(sp, origin) == 0) {
            sp = strtok(NULL, ",");
            ptr[3] = sp;

            // Check if destination matches
            if (strcmp(sp, destination) == 0) {
                numberOfflights++;
                sp = strtok(NULL, ",");
                ptr[4] = sp;
                sp = strtok(NULL, ",");
                ptr[5] = sp;
                sp = strtok(NULL, ",");
                ptr[6] = sp;

                // Print flight data
                printf(cyan"%-12s %-15s %-13s %-12s %-12s %-12s %-10s\n"reset, ptr[0], ptr[1], ptr[2], ptr[3], ptr[4], ptr[5], ptr[6]);
            }
        }
    }

    // Print table footer
    printf(white"---------------------------------------------------------------------------------------\n"reset);

    // Close the CSV file
    fclose(fp);
    return (numberOfflights);
}

void select_flight(passenger* head) {
    // Prompt user to enter flight number
    printf(italic white"\nEnter Flight number: ");
    scanf("%s"reset, head->flight_no);

    // Read flight data based on the entered flight number
    printf(yellow italic"\nProvide passengers information"reset);
    readFlightData(head->flight_no, head);

    int i = 0;
    passenger* p = head;
    while (p != NULL) {
        // Prompt user to enter passenger details
        printf(italic megenta"\nEnter details of passenger %d"reset, i + 1);
        printf(italic white"\nPassenger name: ");
        scanf("%s"reset, p->name);
        printf(italic white"\nAge: ");
        scanf("%d"reset, &p->age);

        // Copy flight details from the head passenger to the current passenger
        strcpy(p->airlines, head->airlines);
        strcpy(p->origin, head->origin);
        strcpy(p->destination, head->destination);
        strcpy(p->departure, head->departure);
        strcpy(p->arrival, head->arrival);
        p->price = head->price;
        strcpy(p->flight_no, head->flight_no);

        i++;

        // Move to the next passenger
        if (p->next == NULL) {
            break;
        } else {
            p = p->next;
        }
    }
}

void readFlightData(char* flightNo, passenger* passenger) {
    FILE* file = fopen("flight_data.csv", "r");
    if (file == NULL) {
        printf("Failed to open the CSV file.\n");
        return;
    }

    // Copy the provided flight number to the passenger structure
    strcpy(passenger->flight_no,flightNo);

    char line[256];
    while (fgets(line, sizeof(line), file)) {
        char token[256];

        // Read the flight number from the CSV line
        strcpy(token,strtok(line,","));

        if (strcmp(token, flightNo) == 0) {
            // Read and store flight details

            // Read and copy the airlines
            strcpy(token, strtok(NULL, ","));
            strcpy(passenger->airlines, token);
            
            // Read and copy the origin
            strcpy(token,strtok(NULL, ","));
            strcpy(passenger->origin, token);
            
            // Read and copy the destination
            strcpy(token,strtok(NULL, ","));
            strcpy(passenger->destination, token);
            
            // Read and copy the departure time
            strcpy(token,strtok(NULL, ","));
            strcpy(passenger->departure,token);

            // Read and copy the arrival time
            strcpy(token,strtok(NULL, ","));
            strcpy(passenger->arrival,token);

            // Read and convert the price to integer
            strcpy(token,strtok(NULL, ","));
            passenger->price=atoi(token);
            
            break;
        }
    }

    fclose(file);
}

void seat_display() {
    // Open the seat arrangement file
    FILE* seat = fopen("Seat_arrangement.csv", "r");
    if (seat == NULL) {
        printf("File can't be opened.\n");
        exit(1);
    }

    // Display the header for available seats
    printf(italic cyan"\n\n    Available seats\n\n"reset);
    printf(megenta"    A  B  C  D  E  F\n"reset);
    printf(white"---------------------\n"reset);

    char line[256];
    int i = 1;
    while (fgets(line, sizeof(line), seat)) {
        Seat s;
        sscanf(line, "%c,%c,%c,%c,%c,%c", &s.a, &s.b, &s.c, &s.d, &s.e, &s.f);
        // Display the seat arrangement for each row
        printf(cyan"%2d  %c  %c  %c  %c  %c  %c\n"reset, i, s.a, s.b, s.c, s.d, s.e, s.f);
        i++;
    }
    printf(white"---------------------\n"reset);
    printf(italic green"\n(*) - Booked seats\n"reset);
    printf(italic green"(-) - Available seats\n"reset);

    // Close the file
    fclose(seat);
}

void select_seat(passenger* head) {
    // Iterate over each passenger in the linked list
    passenger* p = head;
    while (p != NULL) {
        char seat_number[3];
        printf(italic white"\nSelect seat for passenger named %s (18A): ", p->name);
        scanf("%s"reset, seat_number);

        int row, col;

        // Determine the row and column of the selected seat
        if (isdigit(seat_number[1])) {
            // Seat number format is "10A", "10B", etc.
            row = 10 * (seat_number[0] - '0') + (seat_number[1] - '0');
            col = seat_number[2] - 'A' + 1;
        }
        else {
            // Seat number format is "1A", "1B", etc.
            row = seat_number[0] - '0';
            col = seat_number[1] - 'A' + 1;
        }

        // Check if the selected seat is already occupied
        int isOccupied = is_seat_occupied(row, col);
        if (isOccupied) {
            printf(italic bold red"\nSorry, This seat is already booked. \nPlease select another seat."reset);
            seat_display();
            continue; // Skip to the next iteration and prompt for seat selection again
        }

        // Update the passenger's seat information
        sprintf(p->seat_no, "%d%c", row, col + 'A' - 1);
        update_seat(row, col);
        
        // Adjust the passenger's ticket price if it's a window seat (column 1 or 6)
        if (col == 1 || col == 6) {
            p->price += 300;
        }

        // Move to the next passenger in the linked list
        p = p->next;
    }
}

bool is_seat_occupied(int row, int col) {
    // Open the seat arrangement file
    FILE* seat = fopen("Seat_arrangement.csv", "r");
    if (seat == NULL) {
        printf("File can't be opened.\n");
        return false;
    }

    char line[256];
    int currentRow = 0;
    while (fgets(line, sizeof(line), seat)) {
        currentRow++;
        if (currentRow == row) {
            int currentCol = 0;
            char* token = strtok(line, ",");
            while (token != NULL) {
                currentCol++;
                if (currentCol == col) {
                    // Check if the seat is occupied or available
                    if (*token == '*') {
                        fclose(seat);
                        return true; // Seat is occupied
                    } else {
                        fclose(seat);
                        return false; // Seat is available
                    }
                }
                token = strtok(NULL, ",");
            }
        }
    }

    fclose(seat);

    return false; // Seat not found (invalid row or column)
}

void update_seat(int targetRow, int targetColumn) {
    // Open the seat arrangement file in read and write mode
    FILE* file = fopen("Seat_arrangement.csv", "r+");
    if (file == NULL) {
        printf("Unable to open the file.\n");
        return;
    }

    // Array to store the updated seat arrangement
    char seat_arrangement[32][12];
    char line[256];
    int currentRow = 0;

    // Read the file line by line
    while (fgets(line, sizeof(line), file)) {
        if (currentRow == targetRow - 1) {
            int currentCol = 0;
            char* token = strtok(line, ",");
            while (token != NULL) {
                currentCol++;
                if (currentCol == targetColumn) {
                    // Update the seat to '*'
                    seat_arrangement[currentRow][2 * (currentCol - 1)] = '*';
                    seat_arrangement[currentRow][2 * (currentCol - 1) + 1] = ',';
                } else {
                    // Copy the existing seat value
                    seat_arrangement[currentRow][2 * (currentCol - 1)] = token[0];
                    seat_arrangement[currentRow][2 * (currentCol - 1) + 1] = ',';
                }
                token = strtok(NULL, ",");
            }
            seat_arrangement[currentRow][11] = '\n';
        } else {
            // Copy the unchanged line
            strcpy(seat_arrangement[currentRow], line);
        }
        currentRow++;
    }

    // Move the file pointer to the beginning of the file
    fseek(file, 0, SEEK_SET);
    // Write the updated seat arrangement back to the file
    fprintf(file, "%s", seat_arrangement);

    fclose(file);
}

void payment(passenger* head) {
    int payment = 0;
    int price;
    while (head != NULL) {
        price = head->price;
        payment += price;
        head = head->next;
    }

    // Calculate the total payment by summing up the prices of all passengers

    printf(italic blue"\nPlease make a payment of %d\n\n"reset, payment);

    char a[4];
    while (strcmp(a, "yes") != 0) {
        printf(italic white"Have you made the payment (yes/no)? ");
        scanf("%s"reset, a);

        if (strcmp(a, "no") == 0) {
            printf(italic red"\nPlease make the payment!"reset);
        }
    }

    // Prompt the user to confirm if the payment has been made

    printf(italic green"\nThank you for your payment.\n"reset);

    char b[4];
    printf(italic white"\nWould you like to donate 100Rs for Girlchild Education (yes/no)? ");
    scanf("%s"reset, b);

    if (strcmp(b, "yes") == 0) {
        printf(italic blue"\nPlease make the payment on the 'NANHI KALI' homepage on Chrome.\n"reset);
        printf(italic green"Thank you.\nHave a good day!!\n"reset);
    } else {
        printf(italic red"\nSorry for asking miser!!\n"reset);
    }

    // Ask the user if they want to donate and provide instructions for making the donation
}

void generate_pnr(passenger* head){
    FILE *fp;
    char buffer[10];
    int pnr;

    fp = fopen("Passengers_data.csv", "r");
    if (fp == NULL) {
        printf("Error opening file\n");
        exit(1);
    }

    // Open the file containing passenger data

    while(head != NULL){
        fseek(fp, -4, SEEK_END);

        // Move the file pointer to the beginning of the last PNR number in the file

        fgets(buffer, 5, fp);
        sscanf(buffer, "%d", &pnr);
        pnr += 7;

        // Read the last PNR number from the file and increment it by 7 to generate a new PNR number for the current passenger

        printf(italic green"\nPNR Number for passenger %s: %d"reset, head->name, pnr); 

        // Print the generated PNR number for the current passenger

        head->pnr = pnr;
        data_copy(head);
        head = head->next;
    }

    // Assign the generated PNR number to the current passenger and copy the passenger's data to the appropriate file

    fclose(fp);

    // Close the file
}

void data_copy(passenger* p) {
    FILE* sourceFile = fopen("Passengers_data.csv", "a");
    if (sourceFile == NULL) {
        printf("Failed to open the file.\n");
        return;
    }

    // Open the file in append mode to add passenger data at the end

    fprintf(sourceFile, "\n%s,%s,%s,%s,%s,%s,%s,%s,%d,%s,%d",
            p->date, p->origin, p->destination, p->airlines, p->flight_no,
            p->departure, p->arrival, p->name, p->age, p->seat_no, p->pnr);

    // Write the passenger data to the file in CSV format

    fclose(sourceFile);

    // Close the file
}

void Ticket_Display() {
    FILE* fp = fopen("Passengers_data.csv", "r");
    int pn;
    printf(italic white"\nPlease provide your PNR number: ");
    scanf("%d", &pn);
    printf(reset);
    int x = pn;

    if (fp == NULL) {
        printf("Can't open file\n");
    } else {
        char data[1000];
        int column = 0;
        char Date[10];
        char Origin[20];
        char Destination[20];
        char Airlines[20];
        char FlightNumber[20];
        char Departure[10];
        char Arrival[10];
        char Name[10];
        char SeatNumber[10];
        int Age;
        int PNR;
        int flag = 0;

        while (fgets(data, 1000, fp)) {
            column = 0;
            char* value = strtok(data, ", ");
            
            while (value) {
                // Extracting data based on column position
                if (column == 0){
                    strcpy(Date, value);
                }

                if (column == 1){
                    strcpy(Origin, value);
                }

                if (column == 2){
                    strcpy(Destination, value);
                }

                if (column == 3){
                    strcpy(Airlines, value);
                }

                if (column == 4){
                    strcpy(FlightNumber, value);
                }
                    
                if (column == 5){
                    strcpy(Departure, value);
                }
                    
                if (column == 6){
                    strcpy(Arrival, value);
                }
                    
                if (column == 7){
                    strcpy(Name, value);
                }
                    
                if (column == 8){
                    Age = atoi(value);
                }
                    
                if (column == 9){
                    strcpy(SeatNumber, value);
                }
                //passenger data file contains pnr number in the 10th column
                if (column == 10) {
                    PNR = atoi(value);
                    if (x == PNR) {
                        // Printing ticket information
                        printf(italic yellow"Displaying ticket...\n"reset);
                        flag = 1;
                        for (int i = 0; i < 50; i++){
                            printf(white"* ");
                        }
                        printf(reset);
                        for (int i = 0; i < 17; i++) {
                            printf("\n");
                            if (i == 1){
                                printf(bold red "   %s"reset, Airlines);
                            }
                                
                            if (i == 4){
                                printf(bold italic white "   %s                          %s   TO   %s"reset, Name, Origin, Destination);
                            }
                                
                            if (i == 6){
                                printf(bold italic white "   %d Years"reset, Age);
                            }
                                
                            if (i == 8){
                                printf(bold italic blue "   Flight Number : " white"%s" reset, FlightNumber);
                            }
                                
                            if (i == 10){
                                printf(bold italic blue "   Seat Number : "white"%s" reset, SeatNumber);
                            }
                                
                            if (i == 12){
                                printf(bold italic cyan "   Date : "white"%s" reset, Date);
                            }
                               
                            if (i == 14){
                                printf(bold italic cyan "   Departure : " white "%s" cyan "         Arrival : " white "%s" reset, Departure, Arrival);
                            }
                                
                        }
                        for (int i = 0; i < 50; i++){
                            printf(white"* ");
                        }
                        printf(reset);
                    }
                }
                column++;
                value = strtok(NULL, ", ");
            }
            printf("\n");
        }
        
        //condition if no valid pnr is found in passenger data

        if (flag == 0) {
            printf(bold red"Invalid PNR Number\n\n"reset);
            Ticket_Display();
        }

        fclose(fp);

        // Close the file
    }
}

void time_sort(char origin[], char destination[]){
    int line_no=0;
    FILE *fp;
    int durcount=0;
    int linecount=0;
    int duration[30];
    int line_arr[30];
    fp=fopen("flight_data.csv","r");
    char *sp;
    char line[100];
    
    // Read each line of the file
    while(fgets(line,100,fp)!=NULL){
        line_no++;
        sp=strtok(line,",");
        char *ptrr;
        ptrr=sp;
        
        // Skip the first two elements in the line
        for(int i=1;i<=2;i++){
            sp=strtok(NULL,",");
        }
        
        // Check if the origin matches the provided origin
        if(strcmp(sp,origin)==0){
            sp=strtok(NULL,",");
            int originhr;
            int originmin;
            
            // Check if the destination matches the provided destination
            if(strcmp(sp,destination)==0){
                sp=strtok(NULL,",");
                originhr=atoi(&sp[0]);
                if(atoi(&sp[2])/10==0){
                    originmin=atoi(&sp[3]);
                }
                else{
                    originmin=atoi(&sp[2]);
                }

                duration[durcount]=dur(originmin,originhr);
                durcount++;
                line_arr[linecount]=line_no;
                linecount++;
            }
        }
    }
    
    // Sort the durations in ascending order using bubble sort
    for(int j=0;j<=durcount-2;j++){
        for(int k=0;k<=durcount-2-j;k++){
            if(duration[k]>duration[k+1]){
                int swap;
                swap=duration[k];
                duration[k]=duration[k+1];
                duration[k+1]=swap;

                swap=line_arr[k];
                line_arr[k]=line_arr[k+1];
                line_arr[k+1]=swap;
            }
        }
    }
    fclose(fp);
    
    char*cs;
    printf("%-19s %-15s %-12s %-12s %-12s %-12s %-10s\n",megenta"Flight No", "Airlines", "Origin", "Destination", "Departure", "Arrival", "Price"reset);
    printf(white"---------------------------------------------------------------------------------------\n"reset);

    // Print the sorted flights
    for(int h=0;h<durcount;h++){
        char *arrptr[7];
        line_no=0;
        char nline[100];
        fp=fopen("flight_data.csv","r");
        
        // Read each line of the file
        while(fgets(nline,100,fp)!=NULL){
            line_no++;
            
            // Check if the current line number matches the sorted line number
            if(line_arr[h]==line_no){
                for(int q=0;q<=6;q++){
                    if(q==0){
                        cs=strtok(nline,",");
                        arrptr[q]=cs;
                    }
                    else{
                        cs=strtok(NULL,",");
                        arrptr[q]=cs;
                    }
                }
                
                // Print the flight details
                printf(cyan"%-12s %-15s %-13s %-12s %-12s %-12s %-10s\n"reset, arrptr[0], arrptr[1], arrptr[2], arrptr[3], arrptr[4], arrptr[5], arrptr[6]);
                break;
            }
        }
        fclose(fp);
    }
    fclose(fp);
}

int dur(int originmin,int originhr){

    return ((originhr*100)+originmin);
}
