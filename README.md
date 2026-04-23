# Create class definitions and instantiate objects

MS Learn And GitHub exercise on Classes: Create a console app that uses class definitions to instantiate objects.

1. Create a console app and a class.
1. Update the Program.cs file to create instances of the class.
1. Add public fields and updated constructors to the class.
1. Update the class using static members.
1. Create another class that implements private, public, and static members.

<br>

## Prerequisites

- [.NET SDK 10.0](https://dotnet.microsoft.com/en-us/download)
- [Visual Studio Code](https://code.visualstudio.com/) with [C# Dev Kit](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csdevkit)

<br>

## Installation & Usage

1. Clone this repository and switch into project folder

   ```sh
   git clone https://github.com/Kernix13/csharp-class-definitions-objects
   cd csharp-class-definitions-objects
   ```

2. Run the application

   ```bash
   dotnet run
   ```

3. Build the application

   ```bash
   dotnet build
   ```

### <span aria-hidden="true">⚡</span> Quick Start

<!-- I t hink h3 elements should have an emoji/icon at the beginning -->

```sh
git clone https://github.com/Kernix13/csharp-class-definitions-objects
cd csharp-class-definitions-objects
dotnet run
```

<br>

## Code Examples

### Important notes:

- The `new` operator is used to create objects based on a class constructor.
- The `new` operator is used to create new instances of the class. The constructors are called when the new instances are created.
- Each object has its own data fields, and the values assigned to the fields can be different for each object.
- Public fields are accessible from outside the class.
- They can be read from and written to by any code that has access to the object. Public fields are often used to expose object data to other classes.
- Static fields are initialized before an instance of the class is created.
- Static fields are accessed using the class name, not an instance of the class, and are shared among all instances of a class
- The value of a static field is initialized before an instance of the class is created
- When an object is created, static fields are accessed using the class name, not an instance of the class.
- The same value is shared by all instances of the class.
- Static constructors are called when a class is loaded into memory.
- A static constructor is called only once, regardless of how many instances of the class are created.
- Static constructors are used to initialize static fields.
- The `readonly` keyword is used to declare a field that can be assigned a value only when it’s declared or in a constructor
- Readonly fields can be assigned a value only when they’re declared or in a constructor
- `static` field: means that it's shared among all instances of the class
- `static` constructor: initializes the `static` field - it is called when the class is loaded into memory, and before any instances of the class are created
- The `this` keyword refers to the current instance of the class. It's used to access fields, properties, and methods of the current instance
- `private` field: can't be assigned a value from outside the class

Namespaces are used in two ways:

1. The .NET class library uses namespaces to organize its many classes.
2. Developers declare their own namespaces to help control the scope of class and method names in larger programming projects.

- The `using` directive eliminates the requirement to specify the name of the namespace for every class.
- The global namespace is the "root" namespace: `global::System` always refers to the .NET System namespace.
- Using namespaces to group related types together makes it easier to find and use them.
- Implicit `global using` directives are added to new C# projects. This means that you can use types defined in these namespaces without having to specify their fully qualified name or manually add a `using` directive. The implicit aspect refers to the fact that the global using directives are added to a generated file in the project's `obj` directory.

### <span aria-hidden="true">1️⃣</span> BankCustomer Class

```cs
using System;

namespace Classes_M1;

public class BankCustomer
{
    private static int s_nextCustomerId;

    // public fields: FirstName, LastName, and CustomerId
    // initializes data fields using default values
    public string FirstName = "Tim";
    public string LastName = "Shao";

    // readonly: assign a unique value in the constructors (no value is assigned)
    public readonly string CustomerId;

    // 1. parameterless constructor:
    // static constructor
    static BankCustomer()
    {
        Random random = new Random();
        s_nextCustomerId = random.Next(10000000, 20000000);
    }

    // 2. Second constructor that accepts parameters
    public BankCustomer()
    {
        this.CustomerId = (s_nextCustomerId++).ToString("D10");
    }

    // 3. Third constructor that accepts parameters
    public BankCustomer(string firstName, string lastName)
    {
        FirstName = firstName;
        LastName = lastName;
        this.CustomerId = (s_nextCustomerId++).ToString("D10");
    }

}
```

### <span aria-hidden="true">2️⃣</span> BankAccount Class

```cs
using System;

namespace Classes_M1;

public class BankAccount
{

    private static int s_nextAccountNumber;

    // public fields
    // readonly examples:
    public readonly string CustomerId;
    public readonly int AccountNumber;

    // static example:
    public static double InterestRate;

    // initialized with default values:
    public double Balance = 0;
    public string AccountType = "Checking";

    // static constructor: initializes s_nextAccountNumber & InterestRate
    static BankAccount()
    {
        Random random = new Random();
        s_nextAccountNumber = random.Next(10000000, 20000000);
        InterestRate = 0;
    }

    // constructor that initializes the other fields with default values
    public BankAccount(string customerIdNumber)
    {
        this.AccountNumber = s_nextAccountNumber++;
        this.CustomerId = customerIdNumber;
    }

    public BankAccount(string customerIdNumber, double balance, string accountType)
    {
        this.AccountNumber = s_nextAccountNumber++;
        this.CustomerId = customerIdNumber;
        this.Balance = balance;
        this.AccountType = accountType;
    }

}
```

### <span aria-hidden="true">3️⃣</span> Instances in Program.cs

```cs

using Classes_M1;

string firstName = "Tim";
string lastName = "Shao";

// 'new' operator to create objects based on a class constructor
// Create instances of the class BankCustomer
BankCustomer customer1 = new BankCustomer();

firstName = "Lisa";
BankCustomer customer2 = new BankCustomer(firstName, lastName);

firstName = "Sandy";
lastName = "Zoeng";
BankCustomer customer3 = new BankCustomer(firstName, lastName);

Console.WriteLine($"BankCustomer 1: {customer1.FirstName} {customer1.LastName} {customer1.CustomerId}");
Console.WriteLine($"BankCustomer 2: {customer2.FirstName} {customer2.LastName} {customer2.CustomerId}");
Console.WriteLine($"BankCustomer 3: {customer3.FirstName} {customer3.LastName} {customer3.CustomerId}");

// Create instances of the class BankAccount
BankAccount account1 = new BankAccount(customer1.CustomerId);
BankAccount account2 = new BankAccount(customer2.CustomerId, 1500, "Checking");
BankAccount account3 = new BankAccount(customer3.CustomerId, 2500, "Checking");

Console.WriteLine($"Account 1: Account # {account1.AccountNumber}, type {account1.AccountType}, balance {account1.Balance}, rate {BankAccount.InterestRate}, customer ID {account1.CustomerId}");
Console.WriteLine($"Account 2: Account # {account2.AccountNumber}, type {account2.AccountType}, balance {account2.Balance}, rate {BankAccount.InterestRate}, customer ID {account2.CustomerId}");
Console.WriteLine($"Account 3: Account # {account3.AccountNumber}, type {account3.AccountType}, balance {account3.Balance}, rate {BankAccount.InterestRate}, customer ID {account3.CustomerId}");

```

<!-- https://microsoftlearning.github.io/mslearn-develop-oop-csharp/Instructions/Labs/l2p2-lp1-m1-exercise-create-classes-and-objects-in-csharp.html -->
