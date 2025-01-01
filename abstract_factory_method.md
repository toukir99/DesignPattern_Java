# Abstract Factory Method

## What is Abstract Factory Method?
The **Abstract Factory Method** is a creational design pattern that provides an interface for creating families of related or dependent objects without specifying their concrete classes. It encapsulates a group of factories with a common theme, allowing the client to create objects from different families without knowing the exact class being instantiated.

---

## Why is it Used?
The Abstract Factory Method is used when:
- The system needs to be independent of how its objects are created.
- The application needs to create families of related objects that work together.
- You want to enforce consistency among objects in a family.
- It should be easy to add new families of products without modifying the existing code.

### Benefits
- **Encapsulation of Object Creation**: Clients are not aware of the specific classes being instantiated.
- **Consistency**: Ensures that a family of related objects is used together.
- **Extensibility**: Adding new families of products is straightforward.

---

## How it Works (Components)

### Key Components
1. **Abstract Factory**: Defines the interface for creating abstract products.
2. **Concrete Factory**: Implements the creation methods to return specific products.
3. **Abstract Product**: Declares the interface for a type of product.
4. **Concrete Product**: Implements the interface defined by the abstract product.
5. **Client**: Uses the factories to create objects but does not know their concrete classes.

---

## Example: Abstract Factory in Ride-Sharing Project
Let’s say we need to create vehicles for two different categories:
- **Economy Vehicles**: Includes `Sedan` and `Hatchback`.
- **Luxury Vehicles**: Includes `SUV` and `Limousine`.

### Step 1: Abstract Product
```java
// Abstract product for vehicles
public interface Vehicle {
    void displayInfo();
}
```

### Step 2: Concrete Products
```java
// Concrete products for Economy vehicles
public class Sedan implements Vehicle {
    @Override
    public void displayInfo() {
        System.out.println("Economy Vehicle: Sedan");
    }
}

public class Hatchback implements Vehicle {
    @Override
    public void displayInfo() {
        System.out.println("Economy Vehicle: Hatchback");
    }
}

// Concrete products for Luxury vehicles
public class SUV implements Vehicle {
    @Override
    public void displayInfo() {
        System.out.println("Luxury Vehicle: SUV");
    }
}

public class Limousine implements Vehicle {
    @Override
    public void displayInfo() {
        System.out.println("Luxury Vehicle: Limousine");
    }
}
```

### Step 3: Abstract Factory
```java
// Abstract factory for creating vehicles
public interface VehicleFactory {
    Vehicle createPrimaryVehicle();  // e.g., Sedan or SUV
    Vehicle createSecondaryVehicle();  // e.g., Hatchback or Limousine
}
```

### Step 4: Concrete Factories
```java
// Factory for Economy vehicles
public class EconomyVehicleFactory implements VehicleFactory {
    @Override
    public Vehicle createPrimaryVehicle() {
        return new Sedan();  // Primary economy vehicle
    }

    @Override
    public Vehicle createSecondaryVehicle() {
        return new Hatchback();  // Secondary economy vehicle
    }
}

// Factory for Luxury vehicles
public class LuxuryVehicleFactory implements VehicleFactory {
    @Override
    public Vehicle createPrimaryVehicle() {
        return new SUV();  // Primary luxury vehicle
    }

    @Override
    public Vehicle createSecondaryVehicle() {
        return new Limousine();  // Secondary luxury vehicle
    }
}
```

### Step 5: Client Code
```java
public class RideSharingApp {
    public static void main(String[] args) {
        // Get a factory based on the category
        VehicleFactory economyFactory = new EconomyVehicleFactory();
        VehicleFactory luxuryFactory = new LuxuryVehicleFactory();

        // Create and display Economy vehicles
        Vehicle economyPrimary = economyFactory.createPrimaryVehicle();
        Vehicle economySecondary = economyFactory.createSecondaryVehicle();
        economyPrimary.displayInfo();
        economySecondary.displayInfo();

        // Create and display Luxury vehicles
        Vehicle luxuryPrimary = luxuryFactory.createPrimaryVehicle();
        Vehicle luxurySecondary = luxuryFactory.createSecondaryVehicle();
        luxuryPrimary.displayInfo();
        luxurySecondary.displayInfo();
    }
}
```

### Output
```
Economy Vehicle: Sedan
Economy Vehicle: Hatchback
Luxury Vehicle: SUV
Luxury Vehicle: Limousine
```

---

## Summary
The Abstract Factory Method provides a way to encapsulate the creation of families of related objects. In the ride-sharing project example, it helps create category-specific vehicles while maintaining consistency and extensibility. This ensures that the client code remains flexible and easy to maintain.
