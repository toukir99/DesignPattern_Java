# Factory Method Design Pattern

## What is the Factory Method Design Pattern?
The Factory Method Pattern is a creational design pattern that defines an interface or abstract class for creating an object, but lets subclasses decide which object to instantiate. It helps promote loose coupling by delegating the responsibility of object creation to a separate factory method.

## Why Use Factory Method Pattern?
- **Decoupling:** Clients don’t need to know the exact class names of objects they use.
- **Extensibility:** Adding new types of objects is easier because you only need to add new factories, not modify existing code.
- **Centralized Object Creation:** Object creation logic is centralized in factories, making it easier to maintain.

## How Does It Work?
The pattern involves:

1. **Product Interface:** Defines the interface for objects created by the factory.
2. **Concrete Products:** Implements the Product interface and represents the specific types of objects.
3. **Creator/Factory:** Declares the factory method (`createProduct()`).
4. **Concrete Factories:** Implements the factory method to create specific types of products.

---

# Ride-Sharing Example

Imagine we are building a ride-sharing platform. The platform provides different types of rides:

- **Economy:** e.g., standard cars.
- **Premium:** e.g., luxury cars.
- **Bike:** e.g., two-wheelers.

The system needs to create these rides dynamically based on user preferences. Using the Factory Method Pattern, we can decouple the ride creation process and make it extensible for future ride types.

---

# UML Diagram
                +-------------------+
                |       Ride        |  <<Interface>>
                +-------------------+
                | + bookRide()      |
                +-------------------+
                         ▲
          +--------------+--------------+
          |                             |
+-------------------+         +-------------------+
|   EconomyRide     |         |   PremiumRide     |
+-------------------+         +-------------------+
| + bookRide()      |         | + bookRide()      |
+-------------------+         +-------------------+
                         ▲
                         |
                +-------------------+
                |   RideFactory     |  <<Abstract Class>>
                +-------------------+
                | + createRide()    |  <<Abstract Method>>
                +-------------------+
                         ▲
          +--------------+--------------+--------------+
          |                             |              |
+-------------------+         +-------------------+    +-------------------+
| EconomyRideFactory|         | PremiumRideFactory|    |   BikeRideFactory |
+-------------------+         +-------------------+    +-------------------+
| + createRide()    |         | + createRide()    |    | + createRide()    |
+-------------------+         +-------------------+    +-------------------+


## Implementation Steps

### Step 1: Define the Product Interface
The `Ride` interface will represent the rides in the system. Every ride type will implement this interface.

```java
public interface Ride {
    void bookRide();  // Common behavior for all rides
}
```

### Step 2: Implement Concrete Products
Each ride type implements the `Ride` interface with its own behavior.

```java
// Concrete Product: Economy Ride
public class EconomyRide implements Ride {
    @Override
    public void bookRide() {
        System.out.println("Economy ride booked!");
    }
}

// Concrete Product: Premium Ride
public class PremiumRide implements Ride {
    @Override
    public void bookRide() {
        System.out.println("Premium ride booked!");
    }
}

// Concrete Product: Bike Ride
public class BikeRide implements Ride {
    @Override
    public void bookRide() {
        System.out.println("Bike ride booked!");
    }
}
```

### Step 3: Define the Factory (Creator)
The `RideFactory` is an abstract class or interface that declares the factory method `createRide()`.

```java
public abstract class RideFactory {
    // Factory Method
    public abstract Ride createRide();
}
```

### Step 4: Implement Concrete Factories
Each concrete factory is responsible for creating a specific type of ride.

```java
// Concrete Factory for Economy Rides
public class EconomyRideFactory extends RideFactory {
    @Override
    public Ride createRide() {
        return new EconomyRide();  // Creates an Economy Ride
    }
}

// Concrete Factory for Premium Rides
public class PremiumRideFactory extends RideFactory {
    @Override
    public Ride createRide() {
        return new PremiumRide();  // Creates a Premium Ride
    }
}

// Concrete Factory for Bike Rides
public class BikeRideFactory extends RideFactory {
    @Override
    public Ride createRide() {
        return new BikeRide();  // Creates a Bike Ride
    }
}
```

### Step 5: Client Code
The client uses the factory to create rides dynamically based on user preferences.

```java
public class RideSharingApp {
    public static void main(String[] args) {
        // 1. Create an Economy Ride
        RideFactory economyFactory = new EconomyRideFactory();
        Ride economyRide = economyFactory.createRide();
        economyRide.bookRide(); // Output: Economy ride booked!

        // 2. Create a Premium Ride
        RideFactory premiumFactory = new PremiumRideFactory();
        Ride premiumRide = premiumFactory.createRide();
        premiumRide.bookRide(); // Output: Premium ride booked!

        // 3. Create a Bike Ride
        RideFactory bikeFactory = new BikeRideFactory();
        Ride bikeRide = bikeFactory.createRide();
        bikeRide.bookRide(); // Output: Bike ride booked!
    }
}
```

---

## Advantages of Using Factory Method in Ride-Sharing
- **Extensibility:** Adding a new ride type (e.g., Pool Ride) requires creating a new `PoolRide` class and a corresponding factory (`PoolRideFactory`), without altering existing code.
- **Encapsulation:** The ride creation logic is centralized in factories, making it easier to manage.
- **Loose Coupling:** The client (`RideSharingApp`) interacts only with the `Ride` interface and `RideFactory`, not with the concrete ride implementations.

---

## How to Add a New Ride Type?

### New Ride Type: Pool Ride

#### Step 1: Create a Pool Ride Class
```java
// Concrete Product: Pool Ride
public class PoolRide implements Ride {
    @Override
    public void bookRide() {
        System.out.println("Pool ride booked!");
    }
}
```

#### Step 2: Create a Pool Ride Factory
```java
// Concrete Factory for Pool Rides
public class PoolRideFactory extends RideFactory {
    @Override
    public Ride createRide() {
        return new PoolRide();
    }
}
```

#### Step 3: Update Client Code
```java
public class RideSharingApp {
    public static void main(String[] args) {
        // 4. Create a Pool Ride
        RideFactory poolFactory = new PoolRideFactory();
        Ride poolRide = poolFactory.createRide();
        poolRide.bookRide(); // Output: Pool ride booked!
    }
}
```

---

## Conclusion
The Factory Method Pattern allows us to dynamically create ride types in the ride-sharing system without tightly coupling the client to specific ride implementations. It makes the system extensible and easier to maintain.
