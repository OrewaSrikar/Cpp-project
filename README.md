# Cpp-project
My cpp project

#include <iostream>
#include <string>
#include <vector>
using namespace std;

class Vehicle {
protected:
    double costPerKm;
    double distance;
public:
    Vehicle() : costPerKm(0), distance(0) {}
    virtual void calculateCost() = 0;
    void setDistance(double dist) {
        distance = dist;
    }
};

class Car : public Vehicle {
public:
    Car(double distance) {
        costPerKm = 12.0;  // Cost per kilometer for a car
        this->distance = distance;
    }
    void calculateCost() override {
        cout << "Car ride cost: " << distance * costPerKm << endl;
    }
};

class Bike : public Vehicle {
public:
    Bike(double distance) {
        costPerKm = 8.0;  // Cost per kilometer for a bike
        this->distance = distance;
    }
    void calculateCost() override {
        cout << "Bike ride cost: " << distance * costPerKm << endl;
    }
};

class Taxi : public Vehicle {
public:
    Taxi(double distance) {
        costPerKm = 10.0;  // Cost per kilometer for a taxi
        this->distance = distance;
    }
    void calculateCost() override {
        cout << "Taxi ride cost: " << distance * costPerKm << endl;
    }
};

class Driver {
private:
    string name;
    string vehicleType;
public:
    Driver(string name, string vehicleType) : name(name), vehicleType(vehicleType) {}

    string getVehicleType() const {
        return vehicleType;
    }

    void printDetails() const {
        cout << "Driver Name: " << name << endl;
        cout << "Vehicle Type: " << vehicleType << endl;
    }
};

class Rider {
public:
    string name;
    string preferredType;
    string destination;
    
    Rider(string name, string preferredType, string destination)
        : name(name), preferredType(preferredType), destination(destination) {}

    void printDetails() const {
        cout << "Rider Name: " << name << endl;
        cout << "Preferred Vehicle Type: " << preferredType << endl;
        cout << "Destination: " << destination << endl;
    }

    // Method to get the distance based on the destination
    double getDistanceForDestination() const {
        if (destination == "Guntur") {
            return 50;  // Guntur - 50 km
        } else if (destination == "Vijayawada") {
            return 80;  // Vijayawada - 80 km
        } else if (destination == "Mangalgiri") {
            return 150; // Hyderabad - 150 km
        } else if (destination == "Tenali") {
            return 200; // Chennai - 200 km
        } else {
            return 20;   // Default distance for unknown destinations
        }
    }
};

class RideSharingApp {
private:
    vector<Driver> drivers;
    vector<Rider> riders;

public:
    void addDriver(const Driver& driver) {
        drivers.push_back(driver);
        // Automatically try to match drivers with riders after adding a new driver
        matchAndAssignRide();
    }

    void addRider(const Rider& rider) {
        riders.push_back(rider);
        // Automatically try to match riders with drivers after adding a new rider
        matchAndAssignRide();
    }

    void matchAndAssignRide() {
        // Try to match each rider to an available driver
        for (auto& rider : riders) {
            bool matched = false;
            double distance = rider.getDistanceForDestination(); // Get distance based on destination
            for (auto& driver : drivers) {
                if (driver.getVehicleType() == rider.preferredType) {
                    // If the vehicle types match, assign the ride
                    cout << "\nMatching Driver and Rider found!" << endl;
                    driver.printDetails();
                    rider.printDetails();

                    // Now calculate the ride cost based on vehicle type and destination distance
                    if (rider.preferredType == "car") {
                        Car carRide(distance);  // Create car object
                        carRide.calculateCost();
                    } 
                    else if (rider.preferredType == "bike") {
                        Bike bikeRide(distance);  // Create bike object
                        bikeRide.calculateCost();
                    }
                    else if (rider.preferredType == "taxi") {
                        Taxi taxiRide(distance);  // Create taxi object
                        taxiRide.calculateCost();
                    }
                    matched = true;
                    break;  // Once a match is found, break out of the loop
                }
            }
            if (!matched) {
                cout << "No available driver for rider " << rider.name << " with preferred vehicle " << rider.preferredType << endl;
            }
        }
    }

    void showAllDrivers() {
        cout << "\nAll Drivers Registered:\n";
        for (const auto& driver : drivers) {
            driver.printDetails();
        }
    }

    void showAllRiders() {
        cout << "\nAll Riders Registered:\n";
        for (const auto& rider : riders) {
            rider.printDetails();
        }
    }
};

int main() {
    RideSharingApp app;

    int choice;
    do {
        cout << "\n--- Ride Sharing App ---\n";
        cout << "1. Add Driver\n";
        cout << "2. Add Rider\n";
        cout << "3. Show All Drivers\n";
        cout << "4. Show All Riders\n";
        cout << "5. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        if (choice == 1) {
            string name, vehicleType;
            cout << "Enter driver name: ";
            cin >> name;
            cout << "Enter vehicle type (car/bike/taxi): ";
            cin >> vehicleType;
            app.addDriver(Driver(name, vehicleType));
        } 
        else if (choice == 2) {
            string name, preferredType, destination;
            cout << "Enter rider name: ";
            cin >> name;
            cout << "Enter preferred vehicle type (car/bike/taxi): ";
            cin >> preferredType;
            cout << "Enter destination: ";
            cin >> destination;
            app.addRider(Rider(name, preferredType, destination));
        }
        else if (choice == 3) {
            app.showAllDrivers();
        }
        else if (choice == 4) {
            app.showAllRiders();
        }
        else if (choice == 5) {
            cout << "Exiting the app...\n";
        } 
        else {
            cout << "Invalid choice, please try again.\n";
        }

    } while (choice != 5);

    return 0;
}
