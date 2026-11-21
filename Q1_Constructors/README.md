1. Aim
To implement a C++ program using default, parameterized and overloaded constructors to create library book records. To understand constructor delegation, member initializer lists and validation of object data.
2. Theory
A constructor is a special member function that is automatically called when an object is created.

Types of constructors used:
1. Default Constructor – Initializes an object with default values.
2. Parameterized Constructor – Initializes an object with user-provided values.
3. Overloaded Constructors – Multiple constructors with different parameter lists.
4. Constructor Delegation – One constructor calls another constructor in the same class.
A member initializer list initializes data members before the constructor body executes.
3. Algorithm
1. Start the program.
2. Define a class named Book.
3. Declare title, author and number of copies as private members.
4. Create a default constructor.
5. Create a title-only constructor.
6. Create a parameterized constructor.
7. Use constructor delegation to avoid repeated initialization.
8. Validate the number of copies to prevent negative values.
9. Create objects using different constructors.
10. Display the book details.
11. Stop the program.

  ```cpp
#include <iostream>
#include <string>
#include <stdexcept>
using namespace std;

class Book {
private:
    string title;
    string author;
    int copies;

public:
    // Parameterized constructor
    Book(string t, string a, int c)
        : title(t), author(a), copies(c) {
        if (copies < 0) {
            throw invalid_argument("Copies cannot be negative");
        }
    }

    // Default constructor using delegation
    Book() : Book("Untitled", "Unknown", 0) {}

    // Title-only constructor using delegation
    explicit Book(string t) : Book(t, "Unknown", 0) {}

    void display() const {
        cout << "Title: " << title << endl;
        cout << "Author: " << author << endl;
        cout << "Copies: " << copies << endl;
        cout << "-------------------" << endl;
    }
};

int main() {
    try {
        Book b1;
        Book b2("C++ Programming");
        Book b3("Object Oriented Programming",
                "Bjarne Stroustrup", 10);

        cout << "Default Constructor:" << endl;
        b1.display();

        cout << "Title-only Constructor:" << endl;
        b2.display();

        cout << "Parameterized Constructor:" << endl;
        b3.display();

        // Testing negative copies
        // Book b4("Invalid Book", "Unknown", -5);

    }
    catch (const invalid_argument& e) {
        cout << "Error: " << e.what() << endl;
    }

    return 0;
}
```

### 5. Sample Output

```text
Default Constructor:
Title: Untitled
Author: Unknown
Copies: 0
-------------------
Title-only Constructor:
Title: C++ Programming
Author: Unknown
Copies: 0
-------------------
Parameterized Constructor:
Title: Object Oriented Programming
Author: Bjarne Stroustrup
Copies: 10
-------------------
```

### 6. Design and Analysis

**a) When does the compiler supply a default constructor?**

The compiler automatically provides a default constructor when no constructor is declared by the programmer. If a parameterized constructor is declared, the compiler does not automatically provide a default constructor.

**b) Why are member initializer lists preferred?**

- They initialize data members directly.
- They avoid unnecessary assignment.
- They are required for initializing const members and references.
- They improve initialization efficiency.

**c) Order of member initialization**

Members are initialized in the order in which they are declared inside the class, not in the order written in the initializer list.

**d) State of three objects after construction**

| Object | Constructor | Title | Author | Copies |
|---|---|---|---|---:|
| b1 | Default | Untitled | Unknown | 0 |
| b2 | Title-only | C++ Programming | Unknown | 0 |
| b3 | Parameterized | Object Oriented Programming | Bjarne Stroustrup | 10 |

### 7. Experimental Investigation

**Experiment 1: Removing the default constructor**

If the default constructor is removed, the following declaration will produce a compilation error:

```cpp
Book books[3];
```

Reason: The compiler cannot find a constructor that can create each array element without arguments.

**Experiment 2: Member initialization order**

```cpp
class Test {
    int a;
    int b;

public:
    Test() : b(20), a(10) {}
};
```

Compile using:

```bash
g++ -std=c++17 -Wall -Wextra -Wreorder test.cpp -o test
```

Expected warning: The compiler reports that the initialization order differs from the declaration order.

**Experiment 3: Negative copies**

Uncomment the following statement in the main program:

```cpp
Book b4("Invalid Book", "Unknown", -5);
```

Expected output:

```text
Error: Copies cannot be negative
```

The exception prevents the creation of a Book object with a negative number of copies.

### 8. Design Question – Explicit Constructor

The `explicit` keyword prevents unwanted implicit type conversion.

Without `explicit`:

```cpp
Book b = string("C++ Programming");
```

This implicit conversion is allowed if the title-only constructor is not marked explicit.

With `explicit`:

```cpp
explicit Book(string t) : Book(t, "Unknown", 0) {}
```

The same statement produces a compilation error. Direct initialization is still allowed:

```cpp
Book b("C++ Programming");
```

### 9. Alternative Concept and Complexity Analysis

Instead of using multiple overloaded constructors, one constructor with default arguments can be used.

```cpp
Book(string t = "Untitled",
     string a = "Unknown",
     int c = 0)
    : title(t), author(a), copies(c) {
    if (c < 0)
        throw invalid_argument("Invalid copies");
}
```

| Parameter | Overloaded Constructors | Default Arguments |
|---|---|---|
| Code structure | Multiple constructors | One constructor |
| Flexibility | High | High |
| Validation | Centralized through delegation | Centralized in one constructor |
| Time complexity | O(1) | O(1) |
| Space complexity | O(1) | O(1) |

Both approaches require constant time and space for creating one Book object. Constructor delegation avoids repeating validation and initialization logic.

### 10. Result

Thus, the C++ program was successfully implemented using default, parameterized and overloaded constructors. Constructor delegation, member initializer lists and validation were demonstrated. Invalid negative copy values were prevented.
