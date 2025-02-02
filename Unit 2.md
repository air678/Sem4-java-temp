## Q12. What is the super keyword? Explain its use in inheritance with an example.
The `super` keyword in Java is used to refer to the **immediate parent class** of the current object. It is commonly used in inheritance to access parent class members (methods, variables, or constructors) that are overridden or hidden by the child class.

### Uses of `super` in Inheritance:

1. **Access Parent Class Methods**: You can use `super` to call methods from the parent class, especially when they are overridden by the subclass.
2. **Access Parent Class Constructors**: `super()` is used to call the constructor of the parent class. If the parent class has no default constructor, you must explicitly call a constructor of the parent class using `super()`.
3. **Access Parent Class Fields**: If a field in the subclass hides a field in the parent class, you can use `super` to refer to the parent's field.

### Example:

```java
class Animal {
    String name = "Animal";

    // Parent class constructor
    Animal() {
        System.out.println("Animal constructor called");
    }

    void sound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    String name = "Dog";

    // Child class constructor
    Dog() {
        super();  // Calls the parent class constructor
        System.out.println("Dog constructor called");
    }

    void sound() {
        super.sound();  // Calls the parent class method
        System.out.println("Dog barks");
    }

    void printNames() {
        System.out.println(name);       // Prints "Dog"
        System.out.println(super.name); // Prints "Animal"
    }
}

public class Test {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.sound();   // Calls Dog's overridden method, but also calls Animal's sound method
        dog.printNames();  // Prints Dog and Animal's name using super
    }
}
```

### Explanation:

- **Calling Parent Constructor**: `super()` is used in the `Dog` constructor to call the constructor of the `Animal` class, ensuring that the parent class is initialized first.
- **Calling Parent Method**: `super.sound()` calls the `sound()` method from the `Animal` class, even though it's overridden in `Dog`.
- **Accessing Parent Field**: `super.name` is used to access the `name` field from the `Animal` class, which is hidden by the `name` field in `Dog`.
---
## Q13. What is method overriding? Explain the rules for overriding methods in Java.
### **Method Overriding** in Java:

**Method overriding** is a feature of Java that allows a subclass to provide its own implementation of a method that is already defined in its superclass. The method in the subclass must have the same name, return type, and parameters as the method in the superclass.

When a subclass overrides a method, it provides its own specific behavior for that method, and the version of the method that gets called is determined at runtime (this is a key feature of **runtime polymorphism**).

### **Rules for Method Overriding**:

To properly override a method in Java, the following rules must be followed:

1. **Same Method Signature**:
    
    - The method in the subclass must have the same **name**, **return type**, and **parameter list** as the method in the superclass.
    - The **access modifier** can be broader or the same (more on that below), but it cannot be more restrictive.
    
    Example:
    
    ```java
    class Animal {
        void sound() {
            System.out.println("Animal makes a sound");
        }
    }
    
    class Dog extends Animal {
        @Override
        void sound() {  // Correct overriding
            System.out.println("Dog barks");
        }
    }
    ```
    
2. **Inheritance**:
    
    - The subclass must inherit from the superclass (directly or indirectly) to override its method.
    - The method being overridden must be part of the parent class or interface.
3. **Return Type**:
    
    - The return type of the subclass method must be the same as or a subtype of the return type of the superclass method. This is known as **covariant return type**.
    
    Example:
    
    ```java
    class Animal {
        Animal getAnimal() {
            return new Animal();
        }
    }
    
    class Dog extends Animal {
        @Override
        Dog getAnimal() {  // Covariant return type
            return new Dog();
        }
    }
    ```
    
4. **Access Modifiers**:
    
    - The overriding method can have the same or **broader** access modifier than the overridden method.
        - For example, if the superclass method is `protected`, the subclass method can be `protected` or `public` but cannot be `private`.
    - It cannot be more restrictive. For instance, if the superclass method is `public`, the subclass method cannot be `protected` or `private`.
5. **No Static Methods**:
    
    - **Static methods** are not subject to method overriding. If a method is static in the superclass, it is not actually overridden in the subclass. Instead, it is hidden (hiding the static method in the subclass is different from overriding).
6. **No Final or Private Methods**:
    
    - **Final** methods in the superclass cannot be overridden in the subclass.
    - **Private** methods cannot be inherited, so they cannot be overridden.

### **Example of Method Overriding**:

```java
class Animal {
    void sound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Dog barks");
    }
}

public class Test {
    public static void main(String[] args) {
        Animal animal = new Dog();
        animal.sound();  // Output: Dog barks (runtime polymorphism)
    }
}
```

### Explanation:

- The `sound()` method in `Dog` overrides the `sound()` method in `Animal`.
- Even though the reference type is `Animal`, the object is of type `Dog`, so the `sound()` method from `Dog` is called at runtime.
---
## Q14. Explain the concept of runtime polymorphism with an example.
### **Runtime Polymorphism** in Java:

**Runtime polymorphism** (also known as **dynamic method dispatch**) is a concept in object-oriented programming where the method that gets executed is determined **at runtime** based on the actual object type, not the reference type. This allows a subclass to provide a specific implementation of a method that is already defined in its superclass, and the correct method is called at runtime based on the object being referred to.

### Key Points:

- **Overriding**: Runtime polymorphism is achieved through **method overriding** (not overloading).
- **Dynamic Method Dispatch**: The method call is resolved at runtime, meaning that Java decides which method to call based on the actual object type (not the reference type).
- **Flexibility**: It enables flexible and reusable code by allowing a subclass to modify the behavior of an inherited method.

### Example:

```java
class Animal {
    void sound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Dog barks");
    }
}

class Cat extends Animal {
    @Override
    void sound() {
        System.out.println("Cat meows");
    }
}

public class Test {
    public static void main(String[] args) {
        Animal animal;

        // Reference type is Animal, but the actual object is Dog
        animal = new Dog();
        animal.sound();  // Output: Dog barks

        // Reference type is Animal, but the actual object is Cat
        animal = new Cat();
        animal.sound();  // Output: Cat meows
    }
}
```

### Explanation:

- The variable `animal` is of type `Animal`, but at runtime, it is assigned an object of type `Dog` or `Cat`.
- **Dynamic Method Dispatch**: When `animal.sound()` is called, Java determines which `sound()` method to call based on the **actual object** (`Dog` or `Cat`), not the reference type (`Animal`).
- **Runtime Binding**: The method `sound()` in `Dog` or `Cat` is called dynamically based on the object that `animal` points to at runtime.

### Output:

```
Dog barks
Cat meows
```

### Benefits of Runtime Polymorphism:

1. **Flexibility**: You can write more generic code that works with objects of different types, making it easier to extend and maintain.
2. **Code Reusability**: You can use polymorphic methods to work with different subclasses of a superclass, reducing the need for repetitive code.
3. **Loose Coupling**: You can design classes that are less dependent on specific subclasses, allowing for better code decoupling and easier future modifications.
---
## Q15. What is the difference between method overloading and method overriding? Provide examples.
### **Difference Between Method Overloading and Method Overriding**

**Method overloading** and **method overriding** are two important concepts in Java that enable polymorphism, but they differ in several key ways. Here’s a detailed comparison:

### **1. Method Overloading**:

- **Definition**: Method overloading occurs when a class has multiple methods with the **same name** but different **parameter lists** (different number, type, or order of parameters).
    
- **Binding**: The method to be called is determined at **compile time** (this is called **static polymorphism**).
    
- **Purpose**: It allows a class to define multiple methods that perform similar tasks but with different input parameters.
    
- **Example**:
    
    ```java
    class Calculator {
        // Method to add two integers
        int add(int a, int b) {
            return a + b;
        }
    
        // Overloaded method to add three integers
        int add(int a, int b, int c) {
            return a + b + c;
        }
    
        // Overloaded method to add two doubles
        double add(double a, double b) {
            return a + b;
        }
    }
    
    public class Test {
        public static void main(String[] args) {
            Calculator calc = new Calculator();
            System.out.println(calc.add(5, 10));        // Calls add(int, int)
            System.out.println(calc.add(5, 10, 15));    // Calls add(int, int, int)
            System.out.println(calc.add(5.5, 10.5));    // Calls add(double, double)
        }
    }
    ```
    
    **Output**:
    
    ```
    15
    30
    16.0
    ```
    
    **Explanation**:
    
    - The `add()` method is overloaded with different parameter types and counts. The compiler decides which method to call based on the method signature at compile time.

---

### **2. Method Overriding**:

- **Definition**: Method overriding occurs when a **subclass** provides its own implementation of a method that is already defined in its **parent class**. The method signature (name, parameters, return type) must be the same as in the parent class.
    
- **Binding**: The method to be called is determined at **runtime** (this is called **dynamic polymorphism**).
    
- **Purpose**: It allows a subclass to modify or extend the behavior of an inherited method.
    
- **Example**:
    
    ```java
    class Animal {
        // Parent class method
        void sound() {
            System.out.println("Animal makes a sound");
        }
    }
    
    class Dog extends Animal {
        // Method overriding in subclass
        @Override
        void sound() {
            System.out.println("Dog barks");
        }
    }
    
    class Cat extends Animal {
        // Method overriding in subclass
        @Override
        void sound() {
            System.out.println("Cat meows");
        }
    }
    
    public class Test {
        public static void main(String[] args) {
            Animal animal;
    
            animal = new Dog();
            animal.sound();   // Calls Dog's overridden method
    
            animal = new Cat();
            animal.sound();   // Calls Cat's overridden method
        }
    }
    ```
    
    **Output**:
    
    ```
    Dog barks
    Cat meows
    ```
    
    **Explanation**:
    
    - The `sound()` method is overridden in both the `Dog` and `Cat` classes. The method call is resolved at runtime based on the **actual object type**, not the reference type, which is why the method from `Dog` or `Cat` is called, even though the reference is of type `Animal`.

---

### **Key Differences Between Method Overloading and Method Overriding**:

|Aspect|**Method Overloading**|**Method Overriding**|
|---|---|---|
|**Definition**|Multiple methods with the same name but different parameters|Subclass provides a specific implementation for a method already defined in the parent class|
|**Parameters**|Methods must have different parameter types, number, or order|Methods must have the same parameters (signature)|
|**Return Type**|Can have different return types|Must have the same return type or a subtype (covariant return type)|
|**Access Modifier**|Access modifier can vary (can be more restrictive)|Access modifier must be the same or more permissive|
|**Binding Time**|Compile-time binding (static polymorphism)|Runtime binding (dynamic polymorphism)|
|**Purpose**|Provides flexibility to call methods with the same name but different parameters|Allows a subclass to provide its own behavior for an inherited method|
|**Method Type**|Overloading is based on method signatures (parameters)|Overriding is based on method implementation (behavior)|

---
## Q16. Explain the concept of hierarchical inheritance with an example.
Hierarchical inheritance is a type of inheritance in object-oriented programming (OOP) where multiple classes inherit from a single parent class. In this model, one base (parent) class provides common properties and methods, and multiple derived (child) classes inherit those features. This allows for code reusability and better organization of classes.

### Example of **hierarchical inheritance** in Java:

```java
// Parent class (Base class)
class Animal {
    String name;

    public Animal(String name) {
        this.name = name;
    }

    public void speak() {
        // Default behavior, can be overridden by child classes
        System.out.println("Animal makes a sound");
    }
}

// Child class 1
class Dog extends Animal {

    public Dog(String name) {
        super(name);  // Call to the parent class constructor
    }

    @Override
    public void speak() {
        System.out.println(name + " says Woof!");
    }
}

// Child class 2
class Cat extends Animal {

    public Cat(String name) {
        super(name);  // Call to the parent class constructor
    }

    @Override
    public void speak() {
        System.out.println(name + " says Meow!");
    }
}

// Child class 3
class Cow extends Animal {

    public Cow(String name) {
        super(name);  // Call to the parent class constructor
    }

    @Override
    public void speak() {
        System.out.println(name + " says Moo!");
    }
}

public class Main {
    public static void main(String[] args) {
        // Creating objects of different animal types
        Dog dog = new Dog("Buddy");
        Cat cat = new Cat("Whiskers");
        Cow cow = new Cow("Daisy");

        // Calling the speak method for each object
        dog.speak();  // Output: Buddy says Woof!
        cat.speak();  // Output: Whiskers says Meow!
        cow.speak();  // Output: Daisy says Moo!
    }
}
```

### Explanation:

- The `Animal` class is the **parent class** that contains a constructor for setting the name and a `speak()` method.
- The `Dog`, `Cat`, and `Cow` classes **extend** the `Animal` class, inheriting the `name` attribute and `speak()` method. Each subclass **overrides** the `speak()` method to provide their own specific behavior.
- The `Main` class creates instances of `Dog`, `Cat`, and `Cow`, and calls their `speak()` method to show polymorphism in action.

This is an example of **hierarchical inheritance** where multiple child classes (`Dog`, `Cat`, `Cow`) share common behavior from a single parent class (`Animal`).

---
## Q17. What is the role of the final keyword in inheritance? Explain with an example.
In Java, the `final` keyword plays an important role in inheritance, specifically in preventing further modifications to classes, methods, or variables. It can be used in the following contexts:

1. **Final Class**: A class declared with the `final` keyword cannot be subclassed (i.e., no other class can inherit from it).
2. **Final Method**: A method declared with the `final` keyword cannot be overridden by any subclass.
3. **Final Variable**: A variable declared as `final` can only be assigned once and cannot be modified after that.

### 1. **Final Class**: Preventing Inheritance

When a class is marked as `final`, it cannot be extended by any subclass. This is useful when you want to prevent any further modification or specialization of the class.

#### Example:

```java
// Final class
final class Animal {
    String name;

    public Animal(String name) {
        this.name = name;
    }

    public void speak() {
        System.out.println(name + " makes a sound");
    }
}

// This will cause a compile-time error
// class Dog extends Animal {  // Error: Cannot subclass the final class Animal
//    public Dog(String name) {
//        super(name);
//    }
// }
```

In this example, the `Animal` class is declared as `final`. Therefore, trying to extend the `Animal` class with `Dog` will result in a compile-time error.

### 2. **Final Method**: Preventing Method Overriding

When a method is declared as `final`, it cannot be overridden by any subclass. This is useful when you want to prevent a subclass from changing the behavior of that method.

#### Example:

```java
class Animal {
    String name;

    public Animal(String name) {
        this.name = name;
    }

    // Final method
    public final void speak() {
        System.out.println(name + " makes a sound");
    }
}

class Dog extends Animal {
    
    public Dog(String name) {
        super(name);
    }

    // This will cause a compile-time error
    // public void speak() {   // Error: Cannot override the final method speak() from Animal
    //     System.out.println(name + " says Woof!");
    // }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog("Buddy");
        dog.speak();  // Output: Buddy makes a sound
    }
}
```

In this example, the `speak()` method in the `Animal` class is marked as `final`. As a result, the `Dog` class cannot override this method, and trying to do so will lead to a compile-time error.

### 3. **Final Variable**: Preventing Re-assignment

A variable declared as `final` can only be assigned once. This can be useful when you want to ensure that the value of the variable remains constant throughout the program.

#### Example:

```java
class Animal {
    final String name;

    public Animal(String name) {
        this.name = name;
    }

    public void changeName() {
        // This will cause a compile-time error
        // name = "New Name"; // Error: Cannot assign a value to final variable 'name'
    }
}

public class Main {
    public static void main(String[] args) {
        Animal animal = new Animal("Buddy");
        // animal.name = "New Name"; // Error: Cannot assign a value to final variable 'name'
        System.out.println(animal.name);  // Output: Buddy
    }
}
```

In this example, the `name` variable is declared as `final`, meaning it can only be assigned once and cannot be changed later.

### Summary of the Role of `final` in Inheritance:

4. **Final Class**: Prevents the class from being subclassed.
5. **Final Method**: Prevents the method from being overridden in subclasses.
6. **Final Variable**: Ensures the variable's value remains constant after initialization.

In essence, the `final` keyword gives you more control over your classes and methods by preventing unintended modifications, thus ensuring a more predictable and secure behavior in your code.

---
## Q18. What is a constructor in inheritance? Explain how constructors are called in a subclass.
In object-oriented programming (OOP), **constructors** are special methods that are used to initialize objects when they are created. In inheritance, a **constructor** in a subclass (child class) works in conjunction with the constructor of its superclass (parent class).

### Constructor in Inheritance:

When a subclass is created, the constructor of the subclass is called to initialize the subclass's specific attributes. However, the subclass might also need to initialize attributes that are inherited from the superclass. This is where the constructor of the superclass comes into play.

#### Calling Constructors in a Subclass:

1. **Automatic Call to Superclass Constructor (Implicit call)**:
    
    - In many programming languages, when a subclass object is created, the constructor of the **superclass** (parent class) is automatically called before the subclass's constructor is executed. This ensures that the attributes inherited from the parent class are initialized before the subclass-specific initialization happens.
    - For example, in languages like Java, the superclass constructor is implicitly called if no explicit constructor is provided in the subclass.
2. **Explicit Call to Superclass Constructor**:
    
    - In some languages (like Java), you can explicitly call the superclass constructor using the keyword `super()` (without parameters) or `super(arguments)` (with parameters) from within the subclass constructor.
    - The explicit call must be the first statement in the subclass constructor.

### Example (Java):

```java
class Animal {
    String name;
    
    // Constructor of the parent class
    public Animal(String name) {
        this.name = name;
        System.out.println("Animal constructor called");
    }
}

class Dog extends Animal {
    String breed;
    
    // Constructor of the child class
    public Dog(String name, String breed) {
        super(name);  // Call to the superclass constructor
        this.breed = breed;
        System.out.println("Dog constructor called");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog("Buddy", "Golden Retriever");
    }
}
```

### Output:

```
Animal constructor called
Dog constructor called
```

#### Key Points:

- In the `Dog` class, we explicitly call the constructor of the `Animal` class with `super(name)`, which ensures the `name` field is initialized first.
- The constructor of the superclass (`Animal`) is called before the constructor of the subclass (`Dog`).
- If you don't explicitly call `super()`, Java will insert an implicit call to the no-argument constructor of the superclass (if one exists).

### Constructor Call Order:

1. Superclass constructor is called first.
2. Subclass constructor is called afterward.

This order is important because the subclass might depend on the initialization done by the superclass constructor, especially if there are attributes inherited from the superclass.

---
## Q19. Explain the concept of abstract classes and how they relate to inheritance.
In Java, an **abstract class** is a class that cannot be instantiated on its own (i.e., you cannot create an object of an abstract class directly). It is designed to be a base or blueprint for other classes. An abstract class may contain abstract methods (methods without a body) that must be implemented by its subclasses, as well as non-abstract methods (methods with a body) that can be inherited directly.

### Key Features of Abstract Classes:

1. **Abstract Methods**: An abstract class can have one or more abstract methods. These methods only have their signature (method name, return type, parameters) defined but do not have an implementation. Subclasses that extend the abstract class must provide an implementation for these abstract methods.
2. **Non-Abstract Methods**: An abstract class can also have concrete methods with a body. These methods can be inherited by subclasses without being overridden.
3. **Cannot Be Instantiated**: You cannot create an instance of an abstract class directly. It serves as a template for its subclasses.
4. **Constructors**: Abstract classes can have constructors, which can be called by subclasses when they are instantiated.

### The Role of Abstract Classes in Inheritance:

- Abstract classes allow you to define common behaviors (implemented methods) and leave some methods to be implemented by subclasses.
- When a subclass extends an abstract class, it must provide implementations for all abstract methods (unless the subclass is also abstract).
- An abstract class can be used to enforce a contract for the subclasses. Subclasses that extend the abstract class are required to adhere to this contract.

### Example of Abstract Class and Inheritance:

```java
// Abstract class
abstract class Animal {
    String name;

    // Constructor
    public Animal(String name) {
        this.name = name;
    }

    // Abstract method (no body)
    public abstract void sound();  // Subclasses must provide implementation

    // Non-abstract method (with body)
    public void sleep() {
        System.out.println(name + " is sleeping.");
    }
}

// Subclass of Animal
class Dog extends Animal {

    public Dog(String name) {
        super(name);  // Call to the superclass constructor
    }

    // Implementing the abstract method
    @Override
    public void sound() {
        System.out.println(name + " says Woof!");
    }
}

// Another subclass of Animal
class Cat extends Animal {

    public Cat(String name) {
        super(name);  // Call to the superclass constructor
    }

    // Implementing the abstract method
    @Override
    public void sound() {
        System.out.println(name + " says Meow!");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog("Buddy");
        Cat cat = new Cat("Whiskers");

        dog.sound();  // Output: Buddy says Woof!
        dog.sleep();  // Output: Buddy is sleeping.

        cat.sound();  // Output: Whiskers says Meow!
        cat.sleep();  // Output: Whiskers is sleeping.
    }
}
```

### Explanation:

- **Animal Class**: The `Animal` class is abstract and defines:
    - An abstract method `sound()`, which subclasses must implement.
    - A concrete method `sleep()`, which provides behavior that can be inherited by subclasses.
    - A constructor to initialize the `name` attribute.
- **Dog and Cat Classes**: Both `Dog` and `Cat` extend the `Animal` class. They provide implementations for the `sound()` method, which was declared abstract in the `Animal` class. These subclasses also inherit the `sleep()` method, which they don’t need to implement because it’s already provided in the abstract class.

### Why Use Abstract Classes in Inheritance?

1. **Code Reusability**: Abstract classes allow you to define common functionality once (in the abstract class) and let subclasses inherit and use that functionality without having to redefine it.
2. **Polymorphism**: You can use abstract classes to refer to objects of subclasses and invoke methods (including abstract ones) that are specific to each subclass, enabling polymorphic behavior.
3. **Enforcing Consistency**: Abstract classes can enforce that certain methods must be implemented by all subclasses, which helps ensure a consistent interface for all classes in the inheritance hierarchy.

### Comparison to Interfaces:

- **Abstract Classes**: Can have both abstract and concrete methods, constructors, and fields. They are used when classes have common behavior, but you still need to allow for specialized behavior in subclasses.
- **Interfaces**: Can only have abstract methods (before Java 8) and constants. Starting with Java 8, interfaces can have default methods (concrete methods with a body), but they are primarily used to define a contract that classes must follow, without enforcing an inheritance structure.

### Key Points:

- **Abstract Class** is a class that cannot be instantiated directly and may contain abstract methods, which must be implemented by subclasses.
- Subclasses inherit both abstract and concrete methods from the abstract class and must implement the abstract methods.
- Abstract classes are a powerful tool in OOP for establishing common behavior while allowing flexibility for subclass-specific behavior.

---
## Q20. What is the difference between super() and this() in Java? Provide examples.
In Java, `super()` and `this()` are both special keywords used to call constructors, but they have different purposes and are used in different contexts:

### 1. **`super()`**:

`super()` is used to call a constructor of the superclass (parent class). It's used to invoke the constructor of the parent class from the subclass.

- **Usage**: It is called in the subclass constructor to initialize the superclass part of the object.
- **When it's used**: It must be the first statement in the subclass constructor.

#### Example of `super()`:

```java
class Animal {
    Animal() {
        System.out.println("Animal constructor called");
    }
}

class Dog extends Animal {
    Dog() {
        super();  // Calls the Animal constructor
        System.out.println("Dog constructor called");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
    }
}
```

**Output:**

```
Animal constructor called
Dog constructor called
```

### 2. **`this()`**:

`this()` is used to call another constructor in the same class (within the same class's constructor). It can be used for constructor chaining, meaning you can call one constructor from another constructor in the same class.

- **Usage**: It is used to call another constructor of the same class.
- **When it's used**: It must be the first statement in the constructor.

#### Example of `this()`:

```java
class Dog {
    String name;
    
    Dog() {
        this("Unknown");  // Calls the Dog(String name) constructor
    }
    
    Dog(String name) {
        this.name = name;
        System.out.println("Dog name is: " + name);
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog1 = new Dog();  // Calls the no-argument constructor
        Dog dog2 = new Dog("Buddy");  // Calls the constructor with a string argument
    }
}
```

**Output:**

```
Dog name is: Unknown
Dog name is: Buddy
```

### Key Differences:

- **`super()`** is used to call a constructor of the superclass, while **`this()`** is used to call a constructor of the same class.
- **`super()`** must be used when you want to invoke a superclass constructor, whereas **`this()`** is used to chain constructors within the same class.
- Both must be the first statement in the constructor.

### Summary:

- Use **`super()`** when you need to call a superclass constructor.
- Use **`this()`** for calling another constructor from the same class (constructor chaining).

---
## Q21. Explain the concept of inheritance in Java. Write a program to demonstrate single-level, multilevel, and hierarchical inheritance.
### Inheritance in Java:

**Inheritance** is one of the fundamental concepts of Object-Oriented Programming (OOP). It allows a new class (subclass/child class) to inherit properties and behaviors (fields and methods) from an existing class (superclass/parent class). The subclass can then add its own fields and methods or override inherited methods to provide specific functionality.

There are several types of inheritance in Java:

1. **Single-level inheritance**: A class inherits from only one superclass.
2. **Multilevel inheritance**: A class inherits from a subclass, which in turn inherits from another superclass.
3. **Hierarchical inheritance**: Multiple classes inherit from a single superclass.

### Example of the different types of inheritance in Java:

#### 1. **Single-level Inheritance**

In single-level inheritance, a subclass directly inherits from a single superclass.

```java
// Parent class
class Animal {
    void sound() {
        System.out.println("Animal makes a sound");
    }
}

// Child class (inherits from Animal)
class Dog extends Animal {
    void bark() {
        System.out.println("Dog barks");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.sound();  // Inherited from Animal
        dog.bark();   // Defined in Dog
    }
}
```

**Output:**

```
Animal makes a sound
Dog barks
```

#### 2. **Multilevel Inheritance**

In multilevel inheritance, a subclass inherits from a superclass, and then another class inherits from that subclass.

```java
// Parent class
class Animal {
    void eat() {
        System.out.println("Animal eats");
    }
}

// Child class (inherits from Animal)
class Dog extends Animal {
    void bark() {
        System.out.println("Dog barks");
    }
}

// Grandchild class (inherits from Dog)
class Puppy extends Dog {
    void play() {
        System.out.println("Puppy plays");
    }
}

public class Main {
    public static void main(String[] args) {
        Puppy puppy = new Puppy();
        puppy.eat();    // Inherited from Animal
        puppy.bark();   // Inherited from Dog
        puppy.play();   // Defined in Puppy
    }
}
```

**Output:**

```
Animal eats
Dog barks
Puppy plays
```

#### 3. **Hierarchical Inheritance**

In hierarchical inheritance, multiple classes inherit from a single superclass.

```java
// Parent class
class Animal {
    void sound() {
        System.out.println("Animal makes a sound");
    }
}

// Child class 1 (inherits from Animal)
class Dog extends Animal {
    void bark() {
        System.out.println("Dog barks");
    }
}

// Child class 2 (inherits from Animal)
class Cat extends Animal {
    void meow() {
        System.out.println("Cat meows");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        Cat cat = new Cat();
        
        dog.sound();   // Inherited from Animal
        dog.bark();    // Defined in Dog
        
        cat.sound();   // Inherited from Animal
        cat.meow();    // Defined in Cat
    }
}
```

**Output:**

```
Animal makes a sound
Dog barks
Animal makes a sound
Cat meows
```

### Key Points about Inheritance in Java:

4. **Single-level inheritance**: A subclass inherits from only one superclass.
5. **Multilevel inheritance**: A subclass inherits from another subclass, creating a chain of inheritance.
6. **Hierarchical inheritance**: Multiple subclasses inherit from a single superclass.

In all these cases, the subclass can inherit methods and fields from the superclass, allowing code reuse and establishing a relationship between the classes.

### Overriding Methods in Inheritance:

A subclass can override a method from the superclass to provide its own implementation. This allows the subclass to modify or extend the functionality of the inherited method.

---
## Q22. What is polymorphism in Java? Write a program to demonstrate runtime polymorphism using method overriding.
### Polymorphism in Java:

**Polymorphism** is another fundamental concept of Object-Oriented Programming (OOP), which allows objects of different classes to be treated as objects of a common superclass. The most important benefit of polymorphism is that it allows one interface to be used for a general class of actions, making the code more flexible and extensible.

There are two types of polymorphism in Java:

1. **Compile-time polymorphism (Method Overloading)**: This occurs when two or more methods in the same class have the same name but different parameters.
2. **Runtime polymorphism (Method Overriding)**: This occurs when a subclass provides a specific implementation of a method that is already defined in its superclass. The version of the method that gets called is determined at runtime based on the object type, not the reference type.

### **Runtime Polymorphism (Method Overriding)**:

Method overriding is a technique where a subclass provides its own implementation for a method that is already provided by its superclass. The decision of which method to call is made at runtime based on the object type.

**Key Points of Method Overriding**:

- The method in the subclass must have the same name, return type, and parameters as the method in the superclass.
- The method in the subclass overrides the method in the superclass.
- The actual method that gets executed is determined at runtime based on the object type.

### Example of Runtime Polymorphism:

```java
// Superclass (Parent class)
class Animal {
    // Method in superclass
    void sound() {
        System.out.println("Animal makes a sound");
    }
}

// Subclass (Child class)
class Dog extends Animal {
    // Method in subclass (overrides the method in Animal)
    @Override
    void sound() {
        System.out.println("Dog barks");
    }
}

// Another subclass (Child class)
class Cat extends Animal {
    // Method in subclass (overrides the method in Animal)
    @Override
    void sound() {
        System.out.println("Cat meows");
    }
}

public class Main {
    public static void main(String[] args) {
        // Create objects of Dog and Cat
        Animal myAnimal = new Animal();
        Animal myDog = new Dog();
        Animal myCat = new Cat();
        
        // Call the sound() method
        myAnimal.sound();  // Calls Animal's sound() method
        myDog.sound();     // Calls Dog's sound() method (runtime polymorphism)
        myCat.sound();     // Calls Cat's sound() method (runtime polymorphism)
    }
}
```

**Output:**

```
Animal makes a sound
Dog barks
Cat meows
```

### Explanation:

- The **`Animal`** class has a method `sound()`, which is inherited by the **`Dog`** and **`Cat`** subclasses.
- Both **`Dog`** and **`Cat`** classes override the `sound()` method to provide their own specific implementations.
- In the `main` method, we create objects of `Animal`, `Dog`, and `Cat`. However, the reference type is `Animal` for both `myDog` and `myCat`.
- At **runtime**, the JVM determines which version of the `sound()` method to call based on the actual object type (`Dog` or `Cat`), not the reference type (`Animal`).

### Why This is Polymorphism:

- **Polymorphism** allows us to treat objects of `Dog` and `Cat` as objects of the `Animal` type.
- When we call the `sound()` method on the `myDog` and `myCat` references, Java determines at runtime which version of the `sound()` method to invoke, which is an example of **runtime polymorphism**.

### Advantages of Polymorphism:

1. **Flexibility**: The code can work with objects of different types (as long as they are subclasses of the same superclass).
2. **Maintainability**: It simplifies the code and improves readability, as the same method name can be used for different behaviors.
3. **Extensibility**: You can add new subclasses with their own implementations of methods without changing the existing code.
---
## Q23. Explain the concept of method overloading and method overriding. Write a program to demonstrate both.
### Method Overloading and Method Overriding in Java

In Java, **method overloading** and **method overriding** are two concepts that deal with methods in classes, but they are different in terms of behavior, usage, and how the Java compiler determines which method to call.

---

### **1. Method Overloading**

**Method Overloading** occurs when multiple methods in the same class have the same name but different parameters (either in number, type, or both). It is a form of **compile-time polymorphism** because the method to be called is determined at compile-time based on the method signature.

#### **Key Points about Method Overloading**:

- Methods must have the same name.
- They must have different parameter lists (different type or number of parameters).
- Return type can be the same or different, but it alone cannot be used to differentiate overloaded methods.

#### **Example of Method Overloading**:

```java
class Calculator {
    
    // Method to add two integers
    int add(int a, int b) {
        return a + b;
    }
    
    // Overloaded method to add three integers
    int add(int a, int b, int c) {
        return a + b + c;
    }

    // Overloaded method to add two doubles
    double add(double a, double b) {
        return a + b;
    }
}

public class Main {
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        
        // Calling the overloaded methods
        System.out.println("Sum of 2 and 3: " + calc.add(2, 3));           // Calls the method with two integers
        System.out.println("Sum of 1, 2, and 3: " + calc.add(1, 2, 3));     // Calls the method with three integers
        System.out.println("Sum of 2.5 and 3.5: " + calc.add(2.5, 3.5));   // Calls the method with two doubles
    }
}
```

**Output:**

```
Sum of 2 and 3: 5
Sum of 1, 2, and 3: 6
Sum of 2.5 and 3.5: 6.0
```

---

### **2. Method Overriding**

**Method Overriding** occurs when a subclass provides its own implementation of a method that is already defined in its superclass. This is a form of **runtime polymorphism** because the method that gets executed is determined at runtime based on the actual object type (not the reference type).

#### **Key Points about Method Overriding**:

- The method in the subclass must have the same name, return type, and parameters as the method in the superclass.
- The method in the subclass overrides the method in the superclass.
- The decision of which method to call is made at runtime based on the object type.

#### **Example of Method Overriding**:

```java
// Superclass (Parent class)
class Animal {
    // Method in superclass
    void sound() {
        System.out.println("Animal makes a sound");
    }
}

// Subclass (Child class) overriding the method
class Dog extends Animal {
    // Method in subclass (overrides the method in Animal)
    @Override
    void sound() {
        System.out.println("Dog barks");
    }
}

// Another subclass (Child class) overriding the method
class Cat extends Animal {
    // Method in subclass (overrides the method in Animal)
    @Override
    void sound() {
        System.out.println("Cat meows");
    }
}

public class Main {
    public static void main(String[] args) {
        // Create objects of Animal, Dog, and Cat
        Animal myAnimal = new Animal();
        Animal myDog = new Dog();
        Animal myCat = new Cat();
        
        // Call the sound() method
        myAnimal.sound();  // Calls Animal's sound() method
        myDog.sound();     // Calls Dog's sound() method (runtime polymorphism)
        myCat.sound();     // Calls Cat's sound() method (runtime polymorphism)
    }
}
```

**Output:**

```
Animal makes a sound
Dog barks
Cat meows
```

---

### **Differences Between Method Overloading and Method Overriding**:

|**Feature**|**Method Overloading**|**Method Overriding**|
|---|---|---|
|**Definition**|Multiple methods with the same name but different parameters.|A subclass provides its own implementation of a method defined in the superclass.|
|**Binding Time**|Compile-time (static binding).|Runtime (dynamic binding).|
|**Parameters**|Methods must have different parameter lists.|Methods must have the same parameters.|
|**Return Type**|Return type can be the same or different.|Return type must be the same (or subclass of the superclass method's return type).|
|**Method Signature**|Differ in the number or types of parameters.|Same method name, return type, and parameters.|

---

### **Summary**:

- **Method Overloading** allows a class to have multiple methods with the same name, differentiated by the number or types of their parameters.
- **Method Overriding** allows a subclass to provide a specific implementation for a method already defined in its superclass, and the method is invoked based on the actual object type at runtime.
---
## Q24. What is dynamic method dispatch? Write a program to demonstrate its use in runtime polymorphism.
### Dynamic Method Dispatch (Runtime Polymorphism) in Java

**Dynamic Method Dispatch** refers to the process by which a call to an overridden method is resolved at runtime, rather than at compile-time. This is a key feature of **runtime polymorphism** in Java. It allows Java to determine the method to be invoked based on the **actual object type** that is referred to by a reference variable, not the type of the reference variable itself.

In simple terms, the decision of which method to call is made during **runtime**. This is made possible by **method overriding**.

### Key Points:

1. **Method overriding** allows the subclass to provide a specific implementation for a method defined in the superclass.
2. At runtime, Java uses the **actual object type** (not the reference type) to determine which method to call.
3. This process is called **dynamic method dispatch** or **runtime polymorphism**.

### Example to Demonstrate Dynamic Method Dispatch:

Consider the following classes that demonstrate method overriding and dynamic method dispatch.

```java
// Superclass (Parent class)
class Animal {
    void sound() {
        System.out.println("Animal makes a sound");
    }
}

// Subclass 1 (Child class)
class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Dog barks");
    }
}

// Subclass 2 (Child class)
class Cat extends Animal {
    @Override
    void sound() {
        System.out.println("Cat meows");
    }
}

public class Main {
    public static void main(String[] args) {
        // Reference of Animal type
        Animal myAnimal;

        // Assigning Dog object to Animal reference
        myAnimal = new Dog();
        myAnimal.sound(); // Calls Dog's sound() method (runtime polymorphism)

        // Assigning Cat object to Animal reference
        myAnimal = new Cat();
        myAnimal.sound(); // Calls Cat's sound() method (runtime polymorphism)
    }
}
```

### Explanation:

- We have a superclass `Animal` with a method `sound()`.
- We have two subclasses `Dog` and `Cat`, each overriding the `sound()` method.
- In the `main()` method, we have an `Animal` reference (`myAnimal`), but it points to objects of `Dog` and `Cat`.
- The method call `myAnimal.sound()` will invoke the appropriate method based on the actual object type, i.e., if `myAnimal` is pointing to a `Dog` object, the `Dog` class's `sound()` method is called, and if it points to a `Cat` object, the `Cat` class's `sound()` method is called.

### **Output:**

```
Dog barks
Cat meows
```

### How Dynamic Method Dispatch Works:

4. When `myAnimal` references a `Dog` object, Java dynamically resolves the call to `sound()` to the `Dog`'s overridden method at runtime.
5. When `myAnimal` references a `Cat` object, Java dynamically resolves the call to `sound()` to the `Cat`'s overridden method at runtime.

### Why is it Called "Dynamic Method Dispatch"?

- The method that gets executed is not determined at **compile-time** (which would be static binding) but rather at **runtime** based on the actual object (`Dog` or `Cat`).
- This is why it’s called **dynamic** method dispatch.

### Advantages of Dynamic Method Dispatch:

- **Flexibility**: You can write more flexible and reusable code. The same reference can be used to refer to objects of different classes, and the correct method will be invoked at runtime.
- **Extensibility**: You can add new subclasses with different implementations without modifying the existing code.

### **Summary**:

- **Dynamic Method Dispatch** is the mechanism by which Java resolves method calls at runtime based on the actual object type.
- It is a feature of **runtime polymorphism**, which allows Java to call the overridden method in the appropriate subclass based on the object being referenced at runtime.
---
## Q25. Explain the concept of abstract classes and methods. Write a program to demonstrate their use in inheritance.
### Abstract Classes and Methods in Java

**Abstract Classes** and **Abstract Methods** are key concepts in Java that help achieve **abstraction**—a core principle of object-oriented programming (OOP). Abstraction is the process of hiding implementation details and exposing only essential functionality.

### 1. **Abstract Class**

An **abstract class** is a class that cannot be instantiated directly. It serves as a base for other classes to inherit from. An abstract class can have:

- **Abstract methods** (methods without a body).
- **Non-abstract methods** (methods with an implementation).

The purpose of an abstract class is to provide a common structure and common behavior that subclasses can inherit or override.

### 2. **Abstract Method**

An **abstract method** is a method declared in an abstract class that does not have a body. It must be implemented by any concrete (non-abstract) subclass. If a subclass fails to implement an abstract method, it must also be declared abstract.

### Key Points:

- An abstract class can have both abstract and non-abstract methods.
- You cannot create an instance of an abstract class directly.
- A subclass must implement all abstract methods of the abstract class, unless the subclass is also abstract.

---

### Example: Demonstrating Abstract Classes and Methods

Here’s a simple program that demonstrates the use of abstract classes and methods in inheritance.

```java
// Abstract Class (Parent Class)
abstract class Animal {
    // Abstract method (no implementation)
    abstract void sound();

    // Regular method with implementation
    void eat() {
        System.out.println("This animal is eating.");
    }
}

// Concrete Subclass 1 (Child Class)
class Dog extends Animal {
    // Implementing the abstract method sound()
    @Override
    void sound() {
        System.out.println("Dog barks");
    }
}

// Concrete Subclass 2 (Child Class)
class Cat extends Animal {
    // Implementing the abstract method sound()
    @Override
    void sound() {
        System.out.println("Cat meows");
    }
}

public class Main {
    public static void main(String[] args) {
        // Creating objects of the concrete subclasses
        Animal myDog = new Dog();
        Animal myCat = new Cat();

        // Calling the methods
        myDog.sound();  // Dog barks
        myDog.eat();    // This animal is eating.

        myCat.sound();  // Cat meows
        myCat.eat();    // This animal is eating.
    }
}
```

### Explanation:

1. **Abstract Class**: The class `Animal` is abstract. It contains one abstract method `sound()`, which has no implementation, and one non-abstract method `eat()`, which has a defined implementation.
2. **Concrete Subclasses**: Both `Dog` and `Cat` are concrete subclasses of `Animal`. Each subclass provides its own implementation of the `sound()` method.
3. **Instantiation**: In the `main()` method, we create objects of the `Dog` and `Cat` classes (which are concrete). We cannot directly instantiate an object of the abstract class `Animal`.

### **Output:**

```
Dog barks
This animal is eating.
Cat meows
This animal is eating.
```

### Key Concepts in the Example:

4. **Abstract Method**: The `sound()` method in the `Animal` class is abstract, meaning it has no implementation in the `Animal` class itself. Each subclass (`Dog` and `Cat`) provides its own version of `sound()`.
5. **Concrete Classes**: `Dog` and `Cat` are concrete classes that inherit from `Animal`. They provide specific implementations of the abstract method `sound()`.
6. **Inheritance**: The concrete classes inherit the `eat()` method from `Animal`, so they can use it without needing to override it.

### When to Use Abstract Classes and Methods:

- Use an **abstract class** when you want to provide a base class with some common behavior but also want to leave some methods to be implemented by subclasses.
- Use **abstract methods** when you want to enforce that every subclass provides its own implementation of certain methods.

### Advantages of Abstract Classes:

- **Code Reusability**: You can provide common functionality in an abstract class, and concrete subclasses can reuse it.
- **Flexibility**: Subclasses can implement abstract methods according to their specific needs while still benefiting from inherited methods.
- **Encapsulation**: Abstract classes allow you to define a common interface while keeping certain details hidden from the user.

---

### **Summary**:

- **Abstract Class**: Cannot be instantiated directly and may contain both abstract and non-abstract methods. It serves as a base for other classes.
- **Abstract Method**: A method in an abstract class that must be implemented by concrete subclasses.
- **Inheritance**: Subclasses can inherit and override methods from an abstract class.
---
## Q26. Write a Java program to create a class Shape with methods area() and perimeter(). Derive classes Circle and Rectangle from Shape and override the methods to calculate area and perimeter.
Here’s a Java program that demonstrates inheritance with method overriding to calculate the area and perimeter of different shapes. The program consists of an abstract base class `Shape` and two derived classes `Circle` and `Rectangle`, each overriding the methods `area()` and `perimeter()`.

### Java Program:

```java
// Abstract class Shape (Base class)
abstract class Shape {
    // Abstract method to calculate area
    abstract double area();

    // Abstract method to calculate perimeter
    abstract double perimeter();
}

// Circle class derived from Shape
class Circle extends Shape {
    double radius;

    // Constructor for Circle
    Circle(double radius) {
        this.radius = radius;
    }

    // Overriding the area() method
    @Override
    double area() {
        return Math.PI * radius * radius;  // Area of a circle: πr²
    }

    // Overriding the perimeter() method
    @Override
    double perimeter() {
        return 2 * Math.PI * radius;  // Perimeter (circumference) of a circle: 2πr
    }
}

// Rectangle class derived from Shape
class Rectangle extends Shape {
    double length;
    double width;

    // Constructor for Rectangle
    Rectangle(double length, double width) {
        this.length = length;
        this.width = width;
    }

    // Overriding the area() method
    @Override
    double area() {
        return length * width;  // Area of a rectangle: length × width
    }

    // Overriding the perimeter() method
    @Override
    double perimeter() {
        return 2 * (length + width);  // Perimeter of a rectangle: 2(length + width)
    }
}

public class Main {
    public static void main(String[] args) {
        // Creating objects of Circle and Rectangle
        Shape myCircle = new Circle(5);  // Circle with radius 5
        Shape myRectangle = new Rectangle(4, 6);  // Rectangle with length 4 and width 6

        // Displaying area and perimeter for Circle
        System.out.println("Circle:");
        System.out.println("Area: " + myCircle.area());
        System.out.println("Perimeter: " + myCircle.perimeter());

        // Displaying area and perimeter for Rectangle
        System.out.println("\nRectangle:");
        System.out.println("Area: " + myRectangle.area());
        System.out.println("Perimeter: " + myRectangle.perimeter());
    }
}
```

### Explanation:

1. **Abstract Class `Shape`**:
    
    - This class serves as the base class with two abstract methods `area()` and `perimeter()`. These methods must be implemented by any class that extends `Shape`.
2. **Class `Circle`**:
    
    - In this class, the `area()` method calculates the area of the circle using the formula **πr²**.
    - The `perimeter()` method calculates the circumference of the circle using the formula **2πr**.
    - The radius of the circle is passed through the constructor.
3. **Class `Rectangle`**:
    
    - In this class, the `area()` method calculates the area of the rectangle using the formula **length × width**.
    - The `perimeter()` method calculates the perimeter of the rectangle using the formula **2(length + width)**.
    - The length and width are passed through the constructor.
4. **Main Class**:
    
    - In the `main()` method, we create objects of the `Circle` and `Rectangle` classes, and then call the overridden methods `area()` and `perimeter()` for each shape.

### **Output:**

```
Circle:
Area: 78.53981633974483
Perimeter: 31.41592653589793

Rectangle:
Area: 24.0
Perimeter: 20.0
```

### Key Concepts:

- **Abstract Methods**: The methods `area()` and `perimeter()` in the `Shape` class are abstract, meaning they do not have any implementation and must be overridden in the derived classes (`Circle` and `Rectangle`).
- **Method Overriding**: The `Circle` and `Rectangle` classes override the `area()` and `perimeter()` methods to provide specific implementations for each shape.
- **Inheritance**: `Circle` and `Rectangle` are derived from the `Shape` class, which allows us to treat different shapes as objects of the base `Shape` type, but still call their respective overridden methods.
---
## Q27. Explain the concept of constructor chaining in inheritance. Write a program to demonstrate how constructors are called in a subclass.
### Constructor Chaining in Inheritance

**Constructor Chaining** refers to the practice of calling one constructor from another, either within the same class or between parent and child classes. In Java, constructor chaining in inheritance involves calling a constructor from the superclass (parent class) from a subclass (child class) constructor. This helps ensure that the superclass is properly initialized before the subclass.

### Key Concepts:

1. **`super()` keyword**: Used to call the constructor of the superclass.
    
    - It is used in the subclass constructor to call a constructor of the superclass.
    - `super()` must be the first statement in the subclass constructor.
2. **Constructor Chaining Between Superclass and Subclass**:
    
    - If the subclass does not explicitly call a constructor of the superclass, the Java compiler automatically inserts a call to the no-argument constructor of the superclass (`super()`).
    - If the superclass has no default constructor, the subclass must explicitly call one of the constructors of the superclass using `super(parameters)`.
3. **Constructor Chaining Within the Same Class**:
    
    - You can also call one constructor from another constructor in the same class using `this()` (constructor chaining within the same class).

---

### Program to Demonstrate Constructor Chaining in Inheritance

In this example, we will create a superclass `Vehicle` and a subclass `Car`. The `Vehicle` class has a constructor that initializes some basic vehicle properties, while the `Car` class will call the constructor of `Vehicle` using the `super()` keyword to initialize its properties.

```java
// Superclass (Parent class)
class Vehicle {
    String brand;
    int year;

    // Constructor of the Vehicle class
    Vehicle(String brand, int year) {
        this.brand = brand;
        this.year = year;
        System.out.println("Vehicle Constructor called");
    }

    // Method to display vehicle details
    void displayDetails() {
        System.out.println("Brand: " + brand + ", Year: " + year);
    }
}

// Subclass (Child class)
class Car extends Vehicle {
    String model;

    // Constructor of the Car class
    Car(String brand, int year, String model) {
        // Calling the constructor of the superclass (Vehicle)
        super(brand, year);
        this.model = model;
        System.out.println("Car Constructor called");
    }

    // Method to display car details
    void displayCarDetails() {
        displayDetails();
        System.out.println("Model: " + model);
    }
}

public class Main {
    public static void main(String[] args) {
        // Creating an object of the Car class
        Car myCar = new Car("Toyota", 2021, "Corolla");

        // Displaying details of the car
        myCar.displayCarDetails();
    }
}
```

### **Explanation of the Code:**

4. **`Vehicle` Class (Superclass)**:
    
    - The `Vehicle` class has a constructor that takes `brand` and `year` as parameters. This constructor initializes the brand and year fields and prints a message indicating that the `Vehicle` constructor is called.
5. **`Car` Class (Subclass)**:
    
    - The `Car` class extends `Vehicle` and has its own constructor. This constructor takes `brand`, `year`, and `model` as parameters.
    - The `Car` constructor uses `super(brand, year)` to call the constructor of the `Vehicle` class and pass `brand` and `year` to it.
    - The `Car` constructor also initializes the `model` property and prints a message indicating that the `Car` constructor is called.
6. **In the `main()` method**:
    
    - We create an object `myCar` of the `Car` class, passing values for `brand`, `year`, and `model`. This automatically invokes the `Car` constructor, which first calls the `Vehicle` constructor (via `super(brand, year)`) and then initializes the `model` property of the `Car` class.
    - The `displayCarDetails()` method is then called to display the details of the car, including the brand, year, and model.

### **Output:**

```
Vehicle Constructor called
Car Constructor called
Brand: Toyota, Year: 2021
Model: Corolla
```

### **How Constructor Chaining Works Here**:

7. When an object of the `Car` class is created, the constructor of `Car` is called.
8. The first statement in the `Car` constructor is `super(brand, year)`, which calls the constructor of the `Vehicle` class and passes the `brand` and `year` to it.
9. The `Vehicle` constructor is executed, and the brand and year are initialized. A message "Vehicle Constructor called" is printed.
10. Once the `Vehicle` constructor finishes execution, control returns to the `Car` constructor, which continues to initialize the `model` and prints "Car Constructor called".
11. Finally, the `displayCarDetails()` method prints the details of the `Car` object.

### **Key Points**:

- **`super()`** is used to call the superclass constructor from the subclass constructor.
- Constructor chaining ensures that the superclass is initialized before the subclass.
- If a constructor in the subclass does not explicitly call `super()`, Java will automatically call the no-argument constructor of the superclass.

### Constructor Chaining Summary:

- **Constructor chaining** helps maintain proper initialization of both the superclass and subclass in an inheritance hierarchy.
- It ensures that the superclass's properties are initialized before the subclass properties.
- The **`super()`** keyword is used to call the superclass constructor, while **`this()`** is used to call another constructor within the same class.
---
## Q28. What is the difference between super and this keywords? Write a program to demonstrate their use in inheritance.
### Difference Between `super` and `this` Keywords in Java

In Java, both `super` and `this` are special keywords that are used to refer to objects. They are commonly used in inheritance and constructor chaining, but they serve different purposes.

#### **1. `super` Keyword**

The `super` keyword is used to refer to:

- The **superclass** (parent class) of the current object.
- It is used to:
    - Call the superclass constructor.
    - Access superclass methods and fields.
    - Invoke a method or variable from the superclass that is hidden or overridden by the subclass.

#### **2. `this` Keyword**

The `this` keyword is used to refer to:

- The **current instance** (object) of the class.
- It is used to:
    - Access instance variables and methods of the current object.
    - Refer to the current class’s constructor (i.e., constructor chaining within the same class).
    - Distinguish between instance variables and parameters with the same name.

---

### Key Differences:

|**Feature**|**`super`**|**`this`**|
|---|---|---|
|**Refers to**|The superclass (parent class).|The current instance of the class.|
|**Usage**|- To call superclass methods.|- To access current object’s methods/fields.|
||- To call the superclass constructor.|- To refer to the current class constructor.|
|**Constructor Chaining**|Calls constructor of the superclass.|Calls another constructor in the current class.|
|**Scope**|Can be used only in a subclass.|Can be used in any non-static method.|

---

### Example Program to Demonstrate `super` and `this`

In this program, we will use both `super` and `this` to demonstrate their usage in inheritance and constructor chaining.

```java
// Superclass (Parent class)
class Animal {
    String name;

    // Constructor of Animal class
    Animal(String name) {
        this.name = name;  // 'this' refers to the current object's instance variable
        System.out.println("Animal constructor called");
    }

    // Method in Animal class
    void sound() {
        System.out.println("Animal makes a sound");
    }
}

// Subclass (Child class)
class Dog extends Animal {
    String breed;

    // Constructor of Dog class
    Dog(String name, String breed) {
        super(name);  // 'super' calls the constructor of the parent class (Animal)
        this.breed = breed;  // 'this' refers to the current object’s breed variable
        System.out.println("Dog constructor called");
    }

    // Overriding the sound method from Animal class
    @Override
    void sound() {
        super.sound();  // 'super' calls the superclass version of sound()
        System.out.println("Dog barks");  // Dog-specific sound
    }

    // Method to display details of the dog
    void displayDetails() {
        System.out.println("Name: " + name);  // 'name' is inherited from Animal
        System.out.println("Breed: " + breed);
    }
}

public class Main {
    public static void main(String[] args) {
        // Creating an object of Dog class
        Dog myDog = new Dog("Buddy", "Golden Retriever");

        // Calling the method that uses 'super' and 'this'
        myDog.sound();  // Calling overridden method in Dog class
        myDog.displayDetails();  // Calling method to display details of Dog
    }
}
```

### **Explanation of the Code:**

1. **Superclass (`Animal`)**:
    
    - It has a constructor that initializes the `name` of the animal using the `this` keyword to refer to the current object’s `name` field.
    - It has a method `sound()` which is overridden by the subclass `Dog`.
2. **Subclass (`Dog`)**:
    
    - The `Dog` class extends `Animal` and has its own constructor. It uses `super(name)` to call the constructor of the `Animal` class to initialize the `name` field inherited from the superclass.
    - The `this.breed` refers to the `breed` field in the `Dog` class.
    - The `sound()` method is overridden in `Dog` to add specific functionality (barking). It uses `super.sound()` to call the `sound()` method of the `Animal` class before printing "Dog barks".
3. **Main Class**:
    
    - We create an instance of the `Dog` class, passing "Buddy" (name) and "Golden Retriever" (breed) to the constructor. This triggers constructor chaining, where the `Dog` constructor calls the `Animal` constructor using `super(name)`.

### **Output:**

```
Animal constructor called
Dog constructor called
Animal makes a sound
Dog barks
Name: Buddy
Breed: Golden Retriever
```

### **Key Points to Note**:

- **`super(name)`** is used in the `Dog` constructor to call the constructor of the `Animal` class. This ensures that the `name` field in the `Animal` class is initialized before the `breed` field in the `Dog` class.
- **`this.name = name`** is used in the `Animal` class constructor to refer to the current instance's `name` field.
- **`super.sound()`** is used in the `Dog` class to call the `sound()` method from the `Animal` class before adding the dog-specific behavior (`"Dog barks"`).
- **`this.breed = breed`** is used to refer to the `breed` field of the current `Dog` object.

### Summary of `super` and `this` Usage:

- **`super`**: Refers to the superclass (parent class). It is used to call superclass methods or constructors.
- **`this`**: Refers to the current instance of the class. It is used to differentiate instance variables from method parameters, call the current class’s constructor, and refer to the current object’s methods and fields.
---
## Q29. Write a Java program to create a class Animal with a method sound(). Derive classes Dog, Cat, and Cow from Animal and override the sound() method to produce specific sounds.
Certainly! Here's a Java program where we create a superclass `Animal` with a method `sound()`, and then derive subclasses `Dog`, `Cat`, and `Cow` that override the `sound()` method to produce specific sounds for each animal.

### Java Program:

```java
// Superclass (Parent class)
class Animal {
    // Method to be overridden in subclasses
    void sound() {
        System.out.println("Some animal makes a sound");
    }
}

// Subclass Dog
class Dog extends Animal {
    // Overriding the sound() method to provide Dog-specific sound
    @Override
    void sound() {
        System.out.println("Dog barks");
    }
}

// Subclass Cat
class Cat extends Animal {
    // Overriding the sound() method to provide Cat-specific sound
    @Override
    void sound() {
        System.out.println("Cat meows");
    }
}

// Subclass Cow
class Cow extends Animal {
    // Overriding the sound() method to provide Cow-specific sound
    @Override
    void sound() {
        System.out.println("Cow moos");
    }
}

public class Main {
    public static void main(String[] args) {
        // Creating objects of Dog, Cat, and Cow
        Animal myDog = new Dog();
        Animal myCat = new Cat();
        Animal myCow = new Cow();

        // Calling the sound() method on each object
        myDog.sound();  // Should print "Dog barks"
        myCat.sound();  // Should print "Cat meows"
        myCow.sound();  // Should print "Cow moos"
    }
}
```

### **Explanation:**

1. **Superclass `Animal`**:
    
    - The `Animal` class defines a method `sound()`, which is meant to be overridden by subclasses. By default, it prints a general message, "Some animal makes a sound".
2. **Subclass `Dog`**:
    
    - The `Dog` class extends `Animal` and overrides the `sound()` method to print "Dog barks".
3. **Subclass `Cat`**:
    
    - The `Cat` class also extends `Animal` and overrides the `sound()` method to print "Cat meows".
4. **Subclass `Cow`**:
    
    - The `Cow` class extends `Animal` and overrides the `sound()` method to print "Cow moos".
5. **Main Class**:
    
    - In the `main()` method, we create objects of the subclasses `Dog`, `Cat`, and `Cow`, but reference them using the `Animal` type.
    - We then call the `sound()` method on each of these objects, and Java will invoke the overridden version of the `sound()` method for each specific subclass.

### **Output:**

```
Dog barks
Cat meows
Cow moos
```

### **Key Concepts:**

- **Method Overriding**: In the subclasses `Dog`, `Cat`, and `Cow`, the `sound()` method is overridden to provide the specific sound for each animal. This is an example of runtime polymorphism.
- **Polymorphism**: The `sound()` method behaves differently depending on the object type (`Dog`, `Cat`, or `Cow`) that is calling it, even though the method is invoked on an `Animal` reference.
---
## Q30. Explain the concept of interfaces in Java. Write a program to demonstrate how interfaces can be used to achieve multiple inheritance-like behavior.
### **Concept of Interfaces in Java**

In Java, an **interface** is a reference type, similar to a class, that can contain only constants, method signatures, default methods, static methods, and nested types. **Interfaces cannot have instance variables** or constructors, and all methods are implicitly **abstract** (except default and static methods introduced in Java 8).

#### **Key Characteristics of Interfaces:**

1. **Abstract Methods**: An interface can declare abstract methods, which must be implemented by any class that implements the interface.
2. **No Method Body**: All methods in an interface are abstract by default (except default and static methods) and do not have a body.
3. **Multiple Inheritance**: In Java, a class can implement multiple interfaces, thus achieving **multiple inheritance**-like behavior (which is not possible with classes).
4. **Default and Static Methods**: Java 8 introduced **default methods** (methods with a body) and **static methods** in interfaces. These allow the interface to have method implementations.

#### **Why Use Interfaces?**

- **Multiple Inheritance**: Java does not support multiple inheritance with classes (a class cannot inherit from more than one class). However, an interface allows a class to implement multiple interfaces, thus enabling a form of multiple inheritance.
- **Loose Coupling**: Interfaces help achieve loose coupling between different parts of the system, which improves flexibility and maintainability.

---

### **Program to Demonstrate Multiple Inheritance-like Behavior Using Interfaces**

In this example, we will define two interfaces, `Flyable` and `Swimmable`, and then implement both interfaces in a class `Duck`. This will demonstrate how Java achieves multiple inheritance-like behavior through interfaces.

```java
// First interface Flyable
interface Flyable {
    void fly();  // Abstract method
}

// Second interface Swimmable
interface Swimmable {
    void swim();  // Abstract method
}

// Class Duck implements both Flyable and Swimmable interfaces
class Duck implements Flyable, Swimmable {
    // Implementing the fly method from Flyable interface
    @Override
    public void fly() {
        System.out.println("Duck is flying");
    }

    // Implementing the swim method from Swimmable interface
    @Override
    public void swim() {
        System.out.println("Duck is swimming");
    }
}

public class Main {
    public static void main(String[] args) {
        // Creating an object of Duck class
        Duck myDuck = new Duck();

        // Calling the methods from both interfaces
        myDuck.fly();   // Calling fly method from Flyable
        myDuck.swim();  // Calling swim method from Swimmable
    }
}
```

### **Explanation of the Code:**

1. **Interfaces `Flyable` and `Swimmable`**:
    - The `Flyable` interface declares an abstract method `fly()`, which will be implemented by any class that "can fly."
    - The `Swimmable` interface declares an abstract method `swim()`, which will be implemented by any class that "can swim."
2. **Class `Duck`**:
    - The `Duck` class implements both the `Flyable` and `Swimmable` interfaces. This is how Java achieves multiple inheritance-like behavior.
    - The `Duck` class provides specific implementations for both `fly()` and `swim()`, which are required by the interfaces.
3. **In the `main()` method**:
    - We create an object of the `Duck` class and call the `fly()` method (from `Flyable`) and the `swim()` method (from `Swimmable`), demonstrating that a class can implement multiple interfaces and provide behavior for methods declared in both.

### **Output:**

```
Duck is flying
Duck is swimming
```

### **Key Points:**

4. **Multiple Interface Implementation**: The `Duck` class implements both the `Flyable` and `Swimmable` interfaces, which allows it to inherit methods from multiple sources, simulating multiple inheritance.
5. **Loose Coupling**: The `Duck` class does not depend on specific implementations in the interfaces but only provides its own behavior for the methods it must implement. This allows the interfaces to be reused by any other class that needs similar behaviors (e.g., `Bird` could also implement `Flyable`, and `Fish` could implement `Swimmable`).
6. **Polymorphism**: You can use the `Flyable` and `Swimmable` types as references and pass them around, which is useful for loose coupling and flexibility.

---

### **Advantages of Interfaces in Java**:

7. **Achieving Multiple Inheritance**: A class can implement multiple interfaces, thus enabling multiple inheritance-like behavior, which is not allowed with classes.
8. **Code Reusability**: Interfaces can be reused by any class, allowing for flexible and reusable design.
9. **Loose Coupling**: By using interfaces, classes do not need to directly depend on concrete implementations, promoting flexibility and easier maintenance.

---

### **Summary**:

- **Interfaces** in Java are used to define methods that must be implemented by classes. They help achieve multiple inheritance-like behavior by allowing a class to implement more than one interface.
- **Multiple Inheritance**: A class in Java cannot inherit from multiple classes, but it can implement multiple interfaces, making it possible to inherit behavior from more than one source.
- **Polymorphism and Flexibility**: Interfaces promote flexibility and loose coupling by allowing classes to implement various behaviors defined by different interfaces.
---
