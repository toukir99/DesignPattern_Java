# Builder Design Pattern

## What is Builder Design Pattern?
The **Builder Design Pattern** is a **creational design pattern** used to construct complex objects step by step. It allows the creation of an object with **optional parameters** while ensuring **immutability** and **readability**.

Instead of using a **long constructor with multiple parameters**, the **Builder Pattern** provides a **flexible and readable way** to construct objects.

### **Key Benefits:**
- **Improves Code Readability** – No need for long constructor arguments.
- **Simplifies Object Creation** – Only set the necessary fields.
- **Ensures Immutability** – The final object is built once and cannot be changed.
- **Method Chaining Support** – Enables a fluent and easy-to-use API.

---

## Components of the Builder Design Pattern
1. **Product Class** – The main object that needs to be built.
2. **Builder Class** – A class that helps construct the product step by step.
3. **Build Method** – A method in the Builder class to finalize and return the object.
4. **Director Class** – Manages object creation using predefined steps.
5. **Client Code** – Uses the Builder to create an object as needed.

---

## Steps to Implement Builder Design Pattern
1. **Create the Product Class** – Define the object that needs to be built with its attributes.
2. **Create the Builder Class** – Provide methods to set attributes and return the builder itself for method chaining.
3. **Add a Build Method** – In the Builder class, add a method to construct and return the final product.
4. **Use the Director** – Create a director class to manage the sequence of building.
5. **Client Uses the Builder** – The client invokes the builder to construct objects as needed.

---

## Builder Design Pattern Example (Ride-Sharing Project)
### **Scenario**
In a ride-sharing application (e.g., Uber, Lyft), a **Ride object** has multiple attributes such as:
- `rideId` – Unique identifier for the ride.
- `passengerName` – Name of the passenger.
- `driverName` – Assigned driver.
- `pickupLocation` – Where the passenger is picked up.
- `dropLocation` – Destination of the ride.
- `fare` – Cost of the ride.
- `status` – Status of the ride (e.g., "Requested", "Accepted", "Completed").

Since **not all attributes are known at the beginning**, we use the **Builder Pattern** to create objects **flexibly** without needing all parameters at once.

### **1. Product Class (`Ride`)**
```java
public class Ride {
    private final String rideId;
    private final String passengerName;
    private final String driverName;
    private final String pickupLocation;
    private final String dropLocation;
    private final double fare;
    private final String status;

    private Ride(RideBuilder builder) {
        this.rideId = builder.rideId;
        this.passengerName = builder.passengerName;
        this.driverName = builder.driverName;
        this.pickupLocation = builder.pickupLocation;
        this.dropLocation = builder.dropLocation;
        this.fare = builder.fare;
        this.status = builder.status;
    }

    @Override
    public String toString() {
        return "Ride{" +
                "rideId='" + rideId + '\'' +
                ", passengerName='" + passengerName + '\'' +
                ", driverName='" + driverName + '\'' +
                ", pickupLocation='" + pickupLocation + '\'' +
                ", dropLocation='" + dropLocation + '\'' +
                ", fare=" + fare +
                ", status='" + status + '\'' +
                '}';
    }
}
```

### **2. Builder Class (`RideBuilder`)**
```java
public class RideBuilder {
    String rideId;
    String passengerName;
    String driverName;
    String pickupLocation;
    String dropLocation;
    double fare;
    String status;

    public RideBuilder(String rideId) {
        this.rideId = rideId;
    }

    public RideBuilder passengerName(String passengerName) {
        this.passengerName = passengerName;
        return this;
    }

    public RideBuilder driverName(String driverName) {
        this.driverName = driverName;
        return this;
    }

    public RideBuilder pickupLocation(String pickupLocation) {
        this.pickupLocation = pickupLocation;
        return this;
    }

    public RideBuilder dropLocation(String dropLocation) {
        this.dropLocation = dropLocation;
        return this;
    }

    public RideBuilder fare(double fare) {
        this.fare = fare;
        return this;
    }

    public RideBuilder status(String status) {
        this.status = status;
        return this;
    }

    public Ride build() {
        return new Ride(this);
    }
}
```

### **3. Director Class (`RideDirector`)**
```java
public class RideDirector {
    public Ride requestRide(String rideId, String passengerName, String pickupLocation, String dropLocation) {
        return new RideBuilder(rideId)
                .passengerName(passengerName)
                .pickupLocation(pickupLocation)
                .dropLocation(dropLocation)
                .status("Requested")
                .build();
    }

    public Ride acceptRide(Ride ride, String driverName, double fare) {
        return new RideBuilder(ride.rideId)
                .passengerName(ride.passengerName)
                .driverName(driverName)
                .pickupLocation(ride.pickupLocation)
                .dropLocation(ride.dropLocation)
                .fare(fare)
                .status("Accepted")
                .build();
    }

    public Ride completeRide(Ride ride) {
        return new RideBuilder(ride.rideId)
                .passengerName(ride.passengerName)
                .driverName(ride.driverName)
                .pickupLocation(ride.pickupLocation)
                .dropLocation(ride.dropLocation)
                .fare(ride.fare)
                .status("Completed")
                .build();
    }
}
```

---

## When to Use Builder Design Pattern?
- When an object has **many optional parameters**.
- When object **immutability** is required.
- When constructing objects with **different configurations**.
- When we need **method chaining** to improve readability.
- When **step-by-step creation** of an object is needed.

## When Not to Use Builder Design Pattern?
- If an object has **few parameters**, a **simple constructor** is sufficient.
- If the object does **not change frequently**, and **constructors work fine**.
- If there is **no need for step-by-step creation**.
- When using **factories** or **prototype patterns** might be a better fit for simpler objects.

---

## Conclusion
The **Builder Pattern** is highly useful in **ride-sharing applications** where different rides have different parameters, and object creation must be flexible. The **Director** manages ride states like "Requested", "Accepted", and "Completed", ensuring a structured approach to building ride objects in Java and Spring Boot projects.

