# C++ Programming Guide

## 1. Variables and Constants

### Why Use Variables and Constants?

- **Variables**: Store data that can change during program execution
- **Constants**: Store immutable values, preventing accidental modification and improving code safety
- **Type Safety**: C++ is statically typed, catching errors at compile-time

### Variable Declaration

```cpp
// Basic types
int age = 25;                    // Integer
double price = 19.99;            // Floating-point
char grade = 'A';                // Single character
bool isActive = true;            // Boolean
std::string name = "John";       // String (requires <string>)

// Type inference (C++11)
auto count = 42;                 // Compiler deduces int
auto pi = 3.14159;               // Compiler deduces double
```

### Constants

```cpp
// const keyword - runtime constant
const int MAX_USERS = 100;
const double PI = 3.14159;

// constexpr - compile-time constant (C++11)
constexpr int BUFFER_SIZE = 1024;
constexpr double calculate() { return 2.5 * 4; }

// #define (legacy, avoid in modern C++)
#define MAX_SIZE 100             // Preprocessor macro (no type safety)
```

**When to use:**

- `const`: Value known at runtime, type-safe
- `constexpr`: Value known at compile-time, enables optimizations
- `#define`: Rarely needed in modern C++

---

## 2. Arrays and Vectors

### Why Arrays vs Vectors?

- **Arrays**: Fixed-size, stack-allocated, faster access, less flexible
- **Vectors**: Dynamic-size, heap-allocated, flexible, STL container with safety features

### C-Style Arrays

```cpp
// Fixed size determined at compile-time
int numbers[5] = {1, 2, 3, 4, 5};
int matrix[3][3];                // 2D array

// Access
numbers[0] = 10;
int first = numbers[0];

// Size must be constant
const int SIZE = 10;
int buffer[SIZE];
```

**Limitations**:

- No bounds checking
- Size is fixed
- Doesn't know its own size
- Decays to pointer when passed to functions

### std::array (C++11)

```cpp
#include <array>

// Modern alternative to C-style arrays
std::array<int, 5> numbers = {1, 2, 3, 4, 5};

// Benefits
numbers.size();                  // Knows its size
numbers.at(0);                   // Bounds-checked access
```

### std::vector

```cpp
#include <vector>

// Dynamic size
std::vector<int> numbers;        // Empty vector
std::vector<int> nums = {1, 2, 3, 4, 5};
std::vector<int> data(10);       // 10 elements, default-initialized
std::vector<int> filled(10, 5);  // 10 elements, all set to 5

// Dynamic operations
nums.push_back(6);               // Add to end
nums.pop_back();                 // Remove from end
nums.insert(nums.begin(), 0);    // Insert at beginning
nums.clear();                    // Remove all elements
int size = nums.size();          // Get size

// Access
nums[2] = 100;                   // Unchecked access (fast)
nums.at(2) = 100;                // Checked access (throws exception)

// Iteration
for (int num : nums) {           // Range-based for (C++11)
    std::cout << num << " ";
}
```

**Use vectors when:**

- Size is unknown or changes
- Need automatic memory management
- Want bounds checking and safety features

---

## 3. Statements and Operators

### Why Understand Operators?

Operators define how data is manipulated. Understanding precedence and types prevents bugs and enables expressive code.

### Arithmetic Operators

```cpp
int a = 10, b = 3;

int sum = a + b;         // 13 - Addition
int diff = a - b;        // 7  - Subtraction
int prod = a * b;        // 30 - Multiplication
int quot = a / b;        // 3  - Integer division (truncates)
int rem = a % b;         // 1  - Modulo (remainder)

double x = 10.0, y = 3.0;
double result = x / y;   // 3.333... - Floating-point division

// Compound assignment
a += 5;                  // a = a + 5
a *= 2;                  // a = a * 2

// Increment/Decrement
int i = 5;
i++;                     // Post-increment: use then increment
++i;                     // Pre-increment: increment then use
```

### Comparison Operators

```cpp
int x = 10, y = 20;

bool equal = (x == y);        // false - Equal to
bool notEqual = (x != y);     // true  - Not equal to
bool less = (x < y);          // true  - Less than
bool lessEq = (x <= y);       // true  - Less than or equal
bool greater = (x > y);       // false - Greater than
bool greaterEq = (x >= y);    // false - Greater than or equal
```

### Logical Operators

```cpp
bool a = true, b = false;

bool andResult = a && b;      // false - Logical AND
bool orResult = a || b;       // true  - Logical OR
bool notResult = !a;          // false - Logical NOT

// Short-circuit evaluation
if (ptr != nullptr && ptr->value > 0) {
    // Second condition only evaluated if ptr is not null
}
```

### Bitwise Operators

```cpp
int a = 5;    // Binary: 0101
int b = 3;    // Binary: 0011

int andBit = a & b;     // 1 (0001) - Bitwise AND
int orBit = a | b;      // 7 (0111) - Bitwise OR
int xorBit = a ^ b;     // 6 (0110) - Bitwise XOR
int notBit = ~a;        // -6 (inverts all bits)
int leftShift = a << 1; // 10 (1010) - Left shift
int rightShift = a >> 1;// 2 (0010) - Right shift
```

**Use bitwise operators for:**

- Performance-critical code
- Flag management
- Low-level hardware programming

### Ternary Operator

```cpp
// condition ? value_if_true : value_if_false
int max = (a > b) ? a : b;
std::string status = (age >= 18) ? "Adult" : "Minor";
```

---

## 4. Controlling Program Flow

### Why Control Flow?

Control structures determine the order of execution, enabling decision-making, repetition, and non-linear program flow.

### Conditional Statements

#### if-else

```cpp
int score = 85;

if (score >= 90) {
    std::cout << "Grade: A";
} else if (score >= 80) {
    std::cout << "Grade: B";
} else if (score >= 70) {
    std::cout << "Grade: C";
} else {
    std::cout << "Grade: F";
}

// Single line (braces optional but recommended)
if (isActive)
    processData();
```

#### switch Statement

```cpp
char operation = '+';

switch (operation) {
    case '+':
        result = a + b;
        break;                // Prevents fall-through
    case '-':
        result = a - b;
        break;
    case '*':
        result = a * b;
        break;
    case '/':
        if (b != 0)
            result = a / b;
        break;
    default:
        std::cout << "Invalid operation";
        break;
}
```

**Use switch when:**

- Multiple discrete values to check
- Better readability than many if-else
- Only works with integral/enum types

### Loops

#### for Loop

```cpp
// Traditional for loop - when iteration count is known
for (int i = 0; i < 10; i++) {
    std::cout << i << " ";
}

// Range-based for loop (C++11) - iterate over containers
std::vector<int> numbers = {1, 2, 3, 4, 5};
for (int num : numbers) {
    std::cout << num << " ";
}

// With reference (modify elements)
for (int& num : numbers) {
    num *= 2;
}

// With const reference (read-only, no copy)
for (const auto& item : collection) {
    process(item);
}
```

#### while Loop

```cpp
// Condition checked before execution
int count = 0;
while (count < 5) {
    std::cout << count << " ";
    count++;
}

// Useful when iteration count unknown
while (std::cin >> input && input != "quit") {
    processInput(input);
}
```

#### do-while Loop

```cpp
// Executes at least once, then checks condition
int choice;
do {
    std::cout << "Menu:\n1. Option 1\n2. Option 2\n3. Exit\n";
    std::cin >> choice;
    processChoice(choice);
} while (choice != 3);
```

**Use do-while when:** Body must execute at least once (e.g., menus, validation)

### Loop Control

```cpp
// break - exit loop immediately
for (int i = 0; i < 100; i++) {
    if (i == 50)
        break;              // Exit loop when i is 50
}

// continue - skip to next iteration
for (int i = 0; i < 10; i++) {
    if (i % 2 == 0)
        continue;           // Skip even numbers
    std::cout << i << " "; // Only prints odd numbers
}
```

### Modern Control Flow (C++17)

#### if with initializer

```cpp
// Declare variable in if statement scope
if (auto result = calculate(); result > 0) {
    // result is only accessible here
    std::cout << result;
}
```

---

## 5. Characters and Strings

### Why Different String Types?

- **char**: Single character (1 byte)
- **C-strings**: Legacy, null-terminated character arrays
- **std::string**: Modern, safe, feature-rich string class

### Characters

```cpp
#include <cctype>

char ch = 'A';
char digit = '5';
char newline = '\n';      // Escape sequence
char tab = '\t';

// Character functions (cctype)
bool isLetter = std::isalpha(ch);      // true
bool isDigit = std::isdigit(ch);       // false
bool isUpper = std::isupper(ch);       // true
char lower = std::tolower(ch);         // 'a'
char upper = std::toupper('b');        // 'B'
bool isSpace = std::isspace(' ');      // true
```

### C-Style Strings (Character Arrays)

```cpp
#include <cstring>

// Null-terminated character array
char str1[20] = "Hello";
char str2[] = "World";    // Size calculated automatically

// String functions (cstring)
int len = std::strlen(str1);           // Length
std::strcpy(str1, str2);               // Copy str2 to str1
std::strcat(str1, str2);               // Concatenate
int cmp = std::strcmp(str1, str2);     // Compare (0 if equal)
```

**Problems with C-strings:**

- Buffer overflow risks
- Manual memory management
- No bounds checking
- Tedious manipulation

### std::string (Modern C++)

```cpp
#include <string>

// Creation and initialization
std::string empty;
std::string name = "Alice";
std::string repeated(5, 'x');          // "xxxxx"
std::string copy = name;               // Deep copy

// Basic operations
int length = name.length();            // or name.size()
bool isEmpty = name.empty();
name.clear();                          // Empty the string

// Access
char first = name[0];                  // Unchecked
char last = name.at(name.length()-1);  // Checked (throws if out of range)
char front = name.front();             // First character
char back = name.back();               // Last character

// Modification
name += " Smith";                      // Append
name.append(" Jr.");                   // Append
name.insert(0, "Dr. ");                // Insert at position
name.erase(0, 4);                      // Erase from position
name.replace(0, 5, "Bob");             // Replace substring

// Searching
size_t pos = name.find("ice");         // Find substring (returns position)
if (pos != std::string::npos) {
    // Found
}
size_t last = name.rfind('a');         // Find from end

// Substring
std::string sub = name.substr(0, 5);   // Extract substring

// Comparison
bool equal = (name == "Alice");
bool less = (name < "Bob");            // Lexicographic comparison

// Conversion
std::string num = std::to_string(42);  // Number to string
int value = std::stoi("123");          // String to int
double d = std::stod("3.14");          // String to double

// Input/Output
std::cin >> name;                      // Reads until whitespace
std::getline(std::cin, name);          // Reads entire line
```

### String Streams

```cpp
#include <sstream>

// Parse strings
std::string data = "123 45.6 hello";
std::istringstream iss(data);
int a;
double b;
std::string c;
iss >> a >> b >> c;                    // a=123, b=45.6, c="hello"

// Build strings
std::ostringstream oss;
oss << "Value: " << 42 << ", Pi: " << 3.14;
std::string result = oss.str();
```

### String Iteration

```cpp
std::string text = "Hello";

// Index-based
for (size_t i = 0; i < text.length(); i++) {
    std::cout << text[i] << " ";
}

// Iterator-based
for (auto it = text.begin(); it != text.end(); ++it) {
    std::cout << *it << " ";
}

// Range-based (modern, preferred)
for (char c : text) {
    std::cout << c << " ";
}

// Modify with reference
for (char& c : text) {
    c = std::toupper(c);               // Convert to uppercase
}
```

### Why Use std::string?

- Automatic memory management
- Safe operations with bounds checking
- Rich set of member functions
- Intuitive operators (+, ==, <, etc.)
- Compatible with C++ STL algorithms
- Better performance than manual C-string manipulation

---

## 6. Functions

### Why Use Functions?

Functions are fundamental building blocks that:

- **Modularity**: Break complex problems into manageable pieces
- **Reusability**: Write once, use many times
- **Abstraction**: Hide implementation details
- **Testing**: Easier to test isolated units
- **Maintainability**: Changes in one place affect all usages

### Function Structure

```cpp
// return_type function_name(parameter_type parameter_name) {
//     function body
//     return value;
// }

int add(int a, int b) {
    return a + b;
}

void printMessage(std::string msg) {
    std::cout << msg << std::endl;
    // void functions don't return a value
}
```

### Function Declaration vs Definition

```cpp
// Declaration (prototype) - tells compiler function exists
int multiply(int a, int b);

int main() {
    int result = multiply(5, 3);  // Can use before definition
    return 0;
}

// Definition - actual implementation
int multiply(int a, int b) {
    return a * b;
}
```

**Why separate declaration and definition?**

- Allows functions to call each other (circular dependencies)
- Header files contain declarations, source files contain definitions
- Improves compilation time (header changes don't require recompiling everything)

### Parameter Passing

#### Pass by Value (Copy)

```cpp
void increment(int x) {
    x++;  // Modifies local copy only
}

int main() {
    int num = 5;
    increment(num);
    // num is still 5 - original unchanged
}
```

**Use when:**

- Value is small (int, char, bool)
- Don't want to modify original
- Function needs its own copy

**Cost**: Creates copy - expensive for large objects

#### Pass by Reference

```cpp
void increment(int& x) {
    x++;  // Modifies original
}

int main() {
    int num = 5;
    increment(num);
    // num is now 6 - original modified
}
```

**Use when:**

- Need to modify original
- Avoid copying large objects
- Want to return multiple values through parameters

**Benefits**: No copy, direct access to original

#### Pass by Const Reference

```cpp
void display(const std::string& str) {
    std::cout << str;  // Can read but not modify
    // str += "!";     // ERROR: can't modify const
}

int main() {
    std::string msg = "Hello";
    display(msg);  // No copy made, efficient
}
```

**Use when (VERY COMMON):**

- Don't need to modify parameter
- Object is large (string, vector, custom objects)
- Want efficiency without side effects

**Best Practice**: Default choice for non-primitive types as parameters

#### Pass by Pointer

```cpp
void increment(int* ptr) {
    if (ptr != nullptr) {  // Must check for null
        (*ptr)++;
    }
}

int main() {
    int num = 5;
    increment(&num);  // Pass address
    // num is now 6
}
```

**Use when:**

- Optional parameter (can be nullptr)
- Working with C APIs
- Need pointer arithmetic
- Array parameters

### Function Overloading

```cpp
// Same name, different parameters
int add(int a, int b) {
    return a + b;
}

double add(double a, double b) {
    return a + b;
}

int add(int a, int b, int c) {
    return a + b + c;
}

// Compiler chooses based on arguments
int x = add(5, 3);           // Calls int version
double y = add(5.5, 3.2);    // Calls double version
int z = add(1, 2, 3);        // Calls three-parameter version
```

**Why overload?**

- Same operation on different types
- Intuitive interface
- Type safety maintained

**Cannot overload based on return type alone!**

### Default Arguments

```cpp
void greet(std::string name, std::string greeting = "Hello") {
    std::cout << greeting << ", " << name << "!" << std::endl;
}

int main() {
    greet("Alice");              // Uses default: "Hello, Alice!"
    greet("Bob", "Hi");          // Uses provided: "Hi, Bob!"
}

// Multiple defaults - must be rightmost parameters
void configure(int x, int y = 0, int z = 0);
configure(5);       // x=5, y=0, z=0
configure(5, 10);   // x=5, y=10, z=0
```

### Inline Functions

```cpp
inline int square(int x) {
    return x * x;
}

// Suggests compiler replace call with function body
// int result = square(5); becomes int result = 5 * 5;
```

**Use for:**

- Very small, frequently called functions
- Performance-critical code
- Similar to macros but type-safe

**Note**: Modern compilers auto-inline when beneficial, so `inline` is often just a hint

### Lambda Functions (C++11)

```cpp
// Anonymous functions - functions without names
auto add = [](int a, int b) {
    return a + b;
};

int result = add(5, 3);  // 8

// Capture variables from surrounding scope
int multiplier = 3;
auto multiply = [multiplier](int x) {
    return x * multiplier;
};

int value = multiply(5);  // 15

// Common with STL algorithms
std::vector<int> nums = {1, 2, 3, 4, 5};
std::for_each(nums.begin(), nums.end(), [](int n) {
    std::cout << n * 2 << " ";  // Doubles each number
});

// Capture modes:
// [] - capture nothing
// [=] - capture all by value
// [&] - capture all by reference
// [x] - capture x by value
// [&x] - capture x by reference
// [x, &y] - x by value, y by reference
```

**Use lambdas when:**

- Short, one-off functions
- Callbacks and event handlers
- STL algorithms (sort, find_if, transform)
- Don't need function name

### Return Values

```cpp
// Single return value
int calculate() {
    return 42;
}

// Return reference (BE CAREFUL!)
int& getElement(std::vector<int>& vec, int index) {
    return vec[index];  // Safe - vec exists outside function
}

// DANGER: Don't return reference to local variable!
int& dangerous() {
    int local = 5;
    return local;  // BAD! local destroyed after return
}

// Return by value (modern C++ optimizes this)
std::vector<int> createVector() {
    std::vector<int> result = {1, 2, 3};
    return result;  // Move semantics - efficient in C++11+
}

// Multiple return values (C++17 structured bindings)
std::tuple<int, double, std::string> getMultiple() {
    return {42, 3.14, "hello"};
}

auto [num, pi, text] = getMultiple();  // Unpack all three
```

### Function Pointers (Advanced)

```cpp
// Pointer to function
int add(int a, int b) { return a + b; }
int subtract(int a, int b) { return a - b; }

int (*operation)(int, int);  // Declare function pointer

operation = add;
int result1 = operation(5, 3);  // 8

operation = subtract;
int result2 = operation(5, 3);  // 2

// Used for callbacks and strategy patterns
void applyOperation(int a, int b, int (*op)(int, int)) {
    std::cout << op(a, b) << std::endl;
}

applyOperation(10, 5, add);       // 15
applyOperation(10, 5, subtract);  // 5
```

---

## 7. Pointers and References

### Why Pointers and References?

This is one of C++'s most powerful but dangerous features:

- **Direct Memory Access**: Manipulate memory addresses directly
- **Efficiency**: Avoid copying large data structures
- **Dynamic Memory**: Allocate memory at runtime
- **Data Structures**: Build linked lists, trees, graphs
- **Polymorphism**: Enable dynamic dispatch in OOP

**The Trade-off**: Great power, great responsibility - memory leaks, dangling pointers, and segmentation faults are common pitfalls.

### Understanding Memory

```cpp
int x = 42;

// x stores the value 42
// x has a memory address (location in RAM)
// Example: if x is at address 0x7ffee4, that's where 42 is stored
```

### Pointers - The Basics

#### What is a Pointer?

A pointer is a variable that **stores a memory address**.

```cpp
int value = 42;
int* ptr = &value;  // ptr holds the address of value

// Breakdown:
// int*  - "pointer to int" type
// ptr   - pointer variable name
// &     - address-of operator
// &value - "address of value"
```

#### Key Operators

```cpp
int x = 10;
int* ptr = &x;

// & (address-of) - get address of variable
std::cout << &x;     // Prints memory address (e.g., 0x7ffee4)

// * (dereference) - access value at address
std::cout << *ptr;   // Prints 10 (value at address ptr points to)

// Modify through pointer
*ptr = 20;           // x is now 20
```

**Mental Model:**

```bash
Memory:
Address:  0x100    0x104
Value:    20       0x100
Variable: x        ptr

ptr stores the address of x (0x100)
*ptr accesses the value at address 0x100 (which is 20)
```

### Pointer Declaration and Initialization

```cpp
// Declaration
int* ptr1;           // Uninitialized - DANGEROUS!
int* ptr2 = nullptr; // C++11 - proper null pointer (ALWAYS initialize!)

// Good practice - initialize to nullptr
int* ptr = nullptr;

// Later assign
int x = 5;
ptr = &x;

// Multiple pointers (be careful with syntax)
int* p1, *p2;        // Both pointers
int *p3, p4;         // p3 is pointer, p4 is int (confusing!)
int* p5; int* p6;    // Clearer - separate lines
```

**CRITICAL**: Always initialize pointers! Uninitialized pointers contain garbage addresses and cause crashes.

### Null Pointers and Safety

```cpp
int* ptr = nullptr;

// ALWAYS check before dereferencing
if (ptr != nullptr) {
    std::cout << *ptr;  // Safe
}

// Dereferencing null pointer = CRASH
int* bad = nullptr;
*bad = 5;              // Segmentation fault / Access violation
```

**nullptr vs NULL vs 0:**

- `nullptr` (C++11+): Type-safe, always use this
- `NULL`: Macro, often defined as 0, can cause issues
- `0`: Plain integer, confusing

### Pointers and Arrays

```cpp
int arr[5] = {10, 20, 30, 40, 50};
int* ptr = arr;  // Array name is pointer to first element

// Pointer arithmetic
std::cout << *ptr;       // 10 (first element)
std::cout << *(ptr + 1); // 20 (second element)
std::cout << *(ptr + 2); // 30 (third element)

// Equivalent notations
arr[2] == *(arr + 2) == *(ptr + 2) == ptr[2]  // All access third element

// Incrementing pointer
ptr++;                   // Points to next element
std::cout << *ptr;       // 20
```

**Warning**: Pointer arithmetic doesn't check bounds! Easy to access invalid memory.

### Dynamic Memory Allocation

#### new and delete

```cpp
// Allocate single int on heap
int* ptr = new int;        // Uninitialized
int* ptr2 = new int(42);   // Initialized to 42

*ptr = 10;
std::cout << *ptr;

// MUST delete to avoid memory leak
delete ptr;
ptr = nullptr;  // Good practice - prevent dangling pointer

// Allocate array
int* arr = new int[100];   // Array of 100 ints

// Use the array
for (int i = 0; i < 100; i++) {
    arr[i] = i;
}

// MUST use delete[] for arrays
delete[] arr;
arr = nullptr;
```

**Memory Leak Example:**

```cpp
void leaky() {
    int* ptr = new int(42);
    // Function ends, ptr goes away, but memory not freed
    // Lost that memory forever (until program exits)
}
```

**Dangling Pointer Example:**

```cpp
int* ptr = new int(42);
delete ptr;
// ptr still holds the address, but memory is freed
*ptr = 10;  // DANGER! Accessing freed memory
```

#### Smart Pointers (C++11) - PREFER THESE

```cpp
#include <memory>

// unique_ptr - single owner
std::unique_ptr<int> ptr1 = std::make_unique<int>(42);
std::cout << *ptr1;  // Use like regular pointer
// Automatically deleted when ptr1 goes out of scope - NO memory leak!

// Cannot copy, can only move ownership
// std::unique_ptr<int> ptr2 = ptr1;  // ERROR
std::unique_ptr<int> ptr2 = std::move(ptr1);  // OK - transfer ownership
// ptr1 is now nullptr

// shared_ptr - multiple owners, reference counted
std::shared_ptr<int> sp1 = std::make_shared<int>(100);
std::shared_ptr<int> sp2 = sp1;  // Both own the same int
// Memory freed when last shared_ptr is destroyed

std::cout << sp1.use_count();  // 2 - two owners

// weak_ptr - non-owning reference (breaks circular references)
std::weak_ptr<int> wp = sp1;
```

**When to use:**

- `unique_ptr`: Single ownership, most common case (default choice)
- `shared_ptr`: Multiple owners, reference counting overhead
- `weak_ptr`: Break circular references in shared_ptr
- Raw pointers: Only when necessary (C APIs, non-owning pointers)

### References - The Safe Alternative

#### What is a Reference?

A reference is an **alias** (another name) for an existing variable.

```cpp
int x = 10;
int& ref = x;  // ref is another name for x

ref = 20;      // x is now 20
x = 30;        // ref is now 30

// They are the SAME variable
std::cout << &x;    // Memory address
std::cout << &ref;  // SAME memory address
```

#### References vs Pointers

| Feature | Pointer | Reference |
|---------|---------|-----------|
| Can be null? | Yes (nullptr) | No (must bind to object) |
| Can be reassigned? | Yes | No (binds once) |
| Syntax to use | `*ptr` (dereference) | `ref` (direct) |
| Declaration | `int* ptr` | `int& ref` |
| Must initialize? | No (but should!) | Yes (always) |
| Can have array of? | Yes | No |
| Pointer arithmetic? | Yes | No |

```cpp
// Pointer can change what it points to
int x = 10, y = 20;
int* ptr = &x;
ptr = &y;  // Now points to y

// Reference cannot be rebound
int& ref = x;
ref = y;   // This copies y's value to x, doesn't rebind ref!
```

#### When to Use References

```cpp
// Function parameters (avoid copying)
void process(const std::string& str) {
    // Efficient - no copy, can't modify
}

// Return reference to class member
class Container {
    int data[100];
public:
    int& at(int index) {
        return data[index];  // Return reference
    }
};

Container c;
c.at(5) = 42;  // Can modify through reference

// Range-based for loops
std::vector<std::string> names = {"Alice", "Bob"};
for (const auto& name : names) {  // Reference avoids copying strings
    std::cout << name;
}
```

**Use references for:**

- Function parameters (when not nullptr)
- Return values from functions (if object outlives function)
- Aliases to avoid copies
- Operator overloading

**Use pointers for:**

- Optional parameters (can be nullptr)
- Pointer arithmetic (arrays)
- Dynamic memory (prefer smart pointers)
- Data structures (linked lists, trees)
- Reassignment needed

### Const with Pointers and References

```cpp
// Pointer to const - can't modify what it points to
const int* ptr1 = &x;
*ptr1 = 10;  // ERROR
ptr1 = &y;   // OK - can point elsewhere

// Const pointer - can't change what it points to
int* const ptr2 = &x;
*ptr2 = 10;  // OK - can modify value
ptr2 = &y;   // ERROR - can't point elsewhere

// Const pointer to const - can't change either
const int* const ptr3 = &x;
*ptr3 = 10;  // ERROR
ptr3 = &y;   // ERROR

// Const reference (very common in parameters)
const int& ref = x;
// ref = 10;  // ERROR - can't modify

// Memory aid: read right to left
// const int* ptr     = pointer to const int
// int* const ptr     = const pointer to int
// const int* const   = const pointer to const int
```

### Double Pointers (Pointers to Pointers)

```cpp
int x = 42;
int* ptr = &x;      // Pointer to int
int** pptr = &ptr;  // Pointer to pointer to int

std::cout << x;        // 42
std::cout << *ptr;     // 42
std::cout << **pptr;   // 42

// Modify through double pointer
**pptr = 100;          // x is now 100
```

**Use cases:**

- 2D arrays with dynamic allocation
- Modifying pointer in function
- Low-level C interfaces

### Common Pitfalls and Best Practices

#### 1. Memory Leaks

```cpp
// BAD
void leak() {
    int* ptr = new int(42);
    // Forgot to delete
}

// GOOD - Use smart pointers
void noLeak() {
    auto ptr = std::make_unique<int>(42);
    // Automatically cleaned up
}
```

#### 2. Dangling Pointers

```cpp
// BAD
int* getDangling() {
    int local = 42;
    return &local;  // local destroyed after return
}

// GOOD
int* getSafe() {
    int* ptr = new int(42);
    return ptr;  // Caller responsible for deletion
}

// BETTER
std::unique_ptr<int> getBest() {
    return std::make_unique<int>(42);  // Ownership clear
}
```

#### 3. Null Pointer Dereference

```cpp
// BAD
int* ptr = nullptr;
std::cout << *ptr;  // CRASH

// GOOD
if (ptr != nullptr) {
    std::cout << *ptr;
}
```

#### 4. Mismatched new/delete

```cpp
// BAD
int* single = new int(5);
delete[] single;  // Wrong! Should be delete

int* arr = new int[10];
delete arr;       // Wrong! Should be delete[]

// GOOD - use smart pointers to avoid this entirely
auto single = std::make_unique<int>(5);
auto arr = std::make_unique<int[]>(10);
```

### Summary: Pointers vs References

**Use References when:**

- Parameter passing (non-modifiable with const)
- Return values for class members
- Operator overloading
- Value cannot be null
- Don't need to reassign

**Use Smart Pointers when:**

- Dynamic memory allocation
- Optional values (can be null)
- Ownership semantics matter
- Modern C++ (C++11+)

**Use Raw Pointers when:**

- Non-owning pointers (observing only)
- Interfacing with C APIs
- Performance-critical code (rarely needed)
- Array manipulation

**Avoid Raw Pointers + new/delete:** Use smart pointers instead to prevent memory leaks and dangling pointers.

---

## 8. Object-Oriented Programming (OOP)

### Why OOP?

Object-Oriented Programming organizes code around **objects** that combine data and behavior:

- **Encapsulation**: Bundle data and methods, hide implementation details
- **Abstraction**: Simplify complex systems by modeling classes appropriate to the problem
- **Inheritance**: Reuse code by creating hierarchies of classes
- **Polymorphism**: Single interface for different types, runtime flexibility

**The Goal**: Model real-world entities, improve maintainability, and enable code reuse.

### Classes and Objects

#### Basic Class Structure

```cpp
// Class definition
class Rectangle {
private:              // Access specifier
    double width;     // Member variables (data)
    double height;
    
public:
    // Constructor
    Rectangle(double w, double h) : width(w), height(h) {
        // Initialization done in initializer list (preferred)
    }
    
    // Member functions (methods)
    double area() const {           // const = doesn't modify object
        return width * height;
    }
    
    double perimeter() const {
        return 2 * (width + height);
    }
    
    // Setter with validation
    void setWidth(double w) {
        if (w > 0) {
            width = w;
        }
    }
    
    // Getter
    double getWidth() const {
        return width;
    }
};

// Using the class
Rectangle rect(5.0, 3.0);           // Create object (instance)
double a = rect.area();             // Call method: 15.0
rect.setWidth(10.0);                // Modify object
```

**Key Points:**

- Class = blueprint/template
- Object = instance of a class
- Members can be data or functions
- Access specifiers control visibility

### Access Specifiers

```cpp
class Example {
private:
    int privateData;      // Only accessible within class
    void privateMethod(); // Only class methods can call
    
protected:
    int protectedData;    // Accessible in class and derived classes
    
public:
    int publicData;       // Accessible everywhere
    void publicMethod();  // Anyone can call
};
```

**Best Practice:**

- **private**: Default for data members (encapsulation)
- **public**: Interface methods users should access
- **protected**: Data/methods for derived classes

**Why encapsulate?** Control how data is accessed/modified, maintain invariants, allow implementation changes without breaking users.

### Constructors - CRITICAL TOPIC

#### Default Constructor

```cpp
class Point {
private:
    int x, y;
    
public:
    // Default constructor (no parameters)
    Point() : x(0), y(0) {
        // Initializer list preferred over assignment in body
    }
    
    // Can also use in-class initialization (C++11)
    // int x = 0;
    // int y = 0;
};

Point p;  // Calls default constructor
```

#### Parameterized Constructor

```cpp
class Point {
private:
    int x, y;
    
public:
    Point(int xVal, int yVal) : x(xVal), y(yVal) {
        // Initializer list is MORE EFFICIENT than:
        // x = xVal;  // This is assignment, not initialization
        // y = yVal;
    }
};

Point p(10, 20);     // Direct initialization
Point p2 = {5, 10};  // Uniform initialization (C++11)
```

**CRITICAL: Use initializer lists!**

- More efficient (direct initialization vs default + assignment)
- Required for const members and references
- Required for members without default constructors

#### Constructor Overloading

```cpp
class Rectangle {
private:
    double width, height;
    
public:
    // Default constructor
    Rectangle() : width(1.0), height(1.0) {}
    
    // Square constructor (one parameter)
    Rectangle(double side) : width(side), height(side) {}
    
    // Full constructor
    Rectangle(double w, double h) : width(w), height(h) {}
};

Rectangle r1;           // 1x1 rectangle
Rectangle r2(5);        // 5x5 square
Rectangle r3(4, 6);     // 4x6 rectangle
```

#### Copy Constructor

```cpp
class Array {
private:
    int* data;
    size_t size;
    
public:
    // Constructor
    Array(size_t s) : size(s), data(new int[s]) {}
    
    // Copy constructor - CRITICAL for classes with pointers!
    Array(const Array& other) : size(other.size), data(new int[other.size]) {
        std::copy(other.data, other.data + size, data);  // Deep copy
    }
    
    // Destructor
    ~Array() {
        delete[] data;
    }
};

Array arr1(10);
Array arr2 = arr1;  // Calls copy constructor
Array arr3(arr1);   // Also calls copy constructor
```

**Why needed?** Default copy constructor does **shallow copy** (copies pointer, not data). With dynamic memory, this causes:

- Double deletion (both objects delete same memory)
- Dangling pointers
- Memory corruption

#### Move Constructor (C++11)

```cpp
class Array {
private:
    int* data;
    size_t size;
    
public:
    // Move constructor - "steal" resources from temporary
    Array(Array&& other) noexcept 
        : data(other.data), size(other.size) {
        other.data = nullptr;  // Leave other in valid state
        other.size = 0;
    }
};

Array createArray() {
    return Array(1000);  // Temporary returned
}

Array arr = createArray();  // Move constructor called (efficient!)
```

**Why important?** Avoid expensive copies of temporary objects. Move is O(1), copy can be O(n).

### The Rule of Three/Five/Zero - ESSENTIAL

**Rule of Three (C++98):** If you define any of these, define all:

1. Destructor
2. Copy constructor
3. Copy assignment operator

**Rule of Five (C++11):** Add:

4. Move constructor
5. Move assignment operator

**Rule of Zero (Modern C++):** Use smart pointers and RAII (Resource Acquisition Is Initialization), avoid defining any!

```cpp
// BAD - Rule of Three violation
class Bad {
    int* data;
public:
    Bad(int val) : data(new int(val)) {}
    ~Bad() { delete data; }
    // Missing copy constructor and copy assignment!
    // Default shallow copy will cause double delete
};

// GOOD - Rule of Three
class Good {
    int* data;
public:
    Good(int val) : data(new int(val)) {}
    
    ~Good() { delete data; }
    
    Good(const Good& other) : data(new int(*other.data)) {}
    
    Good& operator=(const Good& other) {
        if (this != &other) {  // Self-assignment check
            delete data;
            data = new int(*other.data);
        }
        return *this;
    }
};

// BEST - Rule of Zero (use smart pointers)
class Best {
    std::unique_ptr<int> data;
public:
    Best(int val) : data(std::make_unique<int>(val)) {}
    // Compiler-generated destructor, copy, move all work correctly!
    // Or delete copy operations if move-only:
    Best(const Best&) = delete;
    Best& operator=(const Best&) = delete;
};
```

### Destructor - Memory Management

```cpp
class Resource {
private:
    int* data;
    FILE* file;
    
public:
    Resource() : data(new int[100]) {
        file = fopen("data.txt", "r");
    }
    
    // Destructor - cleanup resources
    ~Resource() {
        delete[] data;      // Free memory
        if (file) {
            fclose(file);   // Close file
        }
    }
};

// Destructor automatically called when object goes out of scope
void function() {
    Resource res;
    // ... use resource ...
}  // ~Resource() called here automatically
```

**CRITICAL:** Destructor is your chance to clean up resources (memory, files, sockets, etc.)

**Destructor rules:**

- No parameters, no return type
- One per class
- Called automatically when object destroyed
- Should never throw exceptions
- Virtual in base classes with virtual functions!

### Inheritance

#### Basic Inheritance

```cpp
// Base class (parent)
class Animal {
protected:
    std::string name;
    
public:
    Animal(const std::string& n) : name(n) {}
    
    void eat() const {
        std::cout << name << " is eating\n";
    }
    
    virtual void makeSound() const {  // virtual = can be overridden
        std::cout << "Some generic sound\n";
    }
    
    virtual ~Animal() {}  // CRITICAL: Virtual destructor!
};

// Derived class (child)
class Dog : public Animal {  // public inheritance
private:
    std::string breed;
    
public:
    Dog(const std::string& n, const std::string& b) 
        : Animal(n), breed(b) {}  // Call base constructor
    
    // Override base method
    void makeSound() const override {  // override keyword (C++11)
        std::cout << name << " barks: Woof!\n";
    }
    
    void wagTail() const {
        std::cout << name << " wags tail\n";
    }
};

// Using inheritance
Dog myDog("Buddy", "Golden Retriever");
myDog.eat();         // Inherited from Animal
myDog.makeSound();   // Overridden in Dog
myDog.wagTail();     // Dog-specific method
```

**Inheritance types:**

- `public`: is-a relationship (Dog **is an** Animal)
- `protected`: rare, used for implementation inheritance
- `private`: implementation detail, not is-a

### Polymorphism - Runtime Flexibility

```cpp
class Shape {
public:
    virtual double area() const = 0;  // Pure virtual (abstract)
    virtual ~Shape() {}
};

class Circle : public Shape {
private:
    double radius;
    
public:
    Circle(double r) : radius(r) {}
    
    double area() const override {
        return 3.14159 * radius * radius;
    }
};

class Rectangle : public Shape {
private:
    double width, height;
    
public:
    Rectangle(double w, double h) : width(w), height(h) {}
    
    double area() const override {
        return width * height;
    }
};

// Polymorphism in action
void printArea(const Shape& shape) {  // Base class reference
    std::cout << "Area: " << shape.area() << std::endl;
}

Circle c(5);
Rectangle r(4, 6);
printArea(c);  // Calls Circle::area()
printArea(r);  // Calls Rectangle::area()

// Also works with pointers
std::vector<std::unique_ptr<Shape>> shapes;
shapes.push_back(std::make_unique<Circle>(3));
shapes.push_back(std::make_unique<Rectangle>(5, 2));

for (const auto& shape : shapes) {
    std::cout << shape->area() << std::endl;  // Runtime polymorphism
}
```

**CRITICAL POINTS:**

1. **Virtual functions**: Enable runtime polymorphism (dynamic dispatch)
2. **Pure virtual** (`= 0`): Makes class abstract (cannot instantiate)
3. **override keyword**: Compiler checks you're actually overriding
4. **Virtual destructor**: MUST have in base classes with virtual functions!

### Virtual Destructor - ESSENTIAL

```cpp
class Base {
public:
    virtual ~Base() {  // Virtual destructor
        std::cout << "Base destructor\n";
    }
};

class Derived : public Base {
private:
    int* data;
    
public:
    Derived() : data(new int[100]) {}
    
    ~Derived() {
        delete[] data;  // Cleanup
        std::cout << "Derived destructor\n";
    }
};

// WHY VIRTUAL DESTRUCTOR IS CRITICAL:
Base* ptr = new Derived();
delete ptr;
// Without virtual destructor: Only ~Base() called - MEMORY LEAK!
// With virtual destructor: ~Derived() then ~Base() called - CORRECT!
```

**Rule:** If class has virtual functions, it MUST have virtual destructor!

### Abstract Classes and Interfaces

```cpp
// Abstract class (has at least one pure virtual function)
class Drawable {
public:
    virtual void draw() const = 0;  // Pure virtual
    virtual ~Drawable() {}
};

// Cannot instantiate abstract class
// Drawable d;  // ERROR

// Interface (all functions pure virtual, no data)
class ISerializable {
public:
    virtual std::string serialize() const = 0;
    virtual void deserialize(const std::string& data) = 0;
    virtual ~ISerializable() {}
};

// Concrete class must implement all pure virtual functions
class Document : public Drawable, public ISerializable {
public:
    void draw() const override {
        // Implementation
    }
    
    std::string serialize() const override {
        return "serialized data";
    }
    
    void deserialize(const std::string& data) override {
        // Implementation
    }
};
```

### Static Members

```cpp
class Counter {
private:
    static int count;  // Shared by ALL objects
    int id;
    
public:
    Counter() : id(++count) {}
    
    static int getCount() {  // Static method
        return count;
        // Can only access static members!
    }
    
    int getId() const {
        return id;
    }
};

// Must define static member outside class
int Counter::count = 0;

Counter c1, c2, c3;
std::cout << Counter::getCount();  // 3 - called on class, not object
```

**Use static for:**

- Data shared across all instances
- Utility functions that don't need object state
- Factory methods

### Const Member Functions

```cpp
class Point {
private:
    int x, y;
    
public:
    Point(int x, int y) : x(x), y(y) {}
    
    // Const member function - promises not to modify object
    int getX() const { return x; }
    int getY() const { return y; }
    
    double distance() const {
        return std::sqrt(x * x + y * y);
    }
    
    // Non-const - can modify
    void setX(int newX) { x = newX; }
    
    // Const overload
    int& at(int index) { return index == 0 ? x : y; }
    const int& at(int index) const { return index == 0 ? x : y; }
};

const Point p(3, 4);
int x = p.getX();     // OK - const function
// p.setX(5);         // ERROR - non-const function on const object
```

**Best practice:** Mark all functions that don't modify object as `const`.

---

## 9. Operator Overloading

### Why Overload Operators?

Enable intuitive syntax for custom types:

```cpp
// Instead of:
result = vector1.add(vector2);

// Write:
result = vector1 + vector2;  // Natural mathematical syntax
```

**Benefits:**

- Code reads like natural language
- Consistent with built-in types
- Enables use with generic algorithms

**Dangers:**

- Can be confusing if misused
- Performance implications if not careful
- Must maintain semantic consistency

### Basic Operator Overloading

```cpp
class Complex {
private:
    double real, imag;
    
public:
    Complex(double r = 0, double i = 0) : real(r), imag(i) {}
    
    // Arithmetic operators as member functions
    Complex operator+(const Complex& other) const {
        return Complex(real + other.real, imag + other.imag);
    }
    
    Complex operator-(const Complex& other) const {
        return Complex(real - other.real, imag - other.imag);
    }
    
    Complex operator*(const Complex& other) const {
        return Complex(
            real * other.real - imag * other.imag,
            real * other.imag + imag * other.real
        );
    }
    
    // Compound assignment
    Complex& operator+=(const Complex& other) {
        real += other.real;
        imag += other.imag;
        return *this;  // Return reference to self
    }
    
    // Unary minus
    Complex operator-() const {
        return Complex(-real, -imag);
    }
    
    // Getters for friend functions
    double getReal() const { return real; }
    double getImag() const { return imag; }
};

// Usage
Complex c1(3, 4);
Complex c2(1, 2);
Complex c3 = c1 + c2;      // (4, 6)
Complex c4 = c1 * c2;      // (-5, 10)
c1 += c2;                  // c1 is now (4, 6)
Complex c5 = -c1;          // (-4, -6)
```

### Member vs Non-Member Operators

#### Member Function (Left operand is *this)

```cpp
class Vector {
    double x, y;
public:
    Vector(double x, double y) : x(x), y(y) {}
    
    // Member: vector + vector
    Vector operator+(const Vector& other) const {
        return Vector(x + other.x, y + other.y);
    }
    
    // v1 + v2 translates to v1.operator+(v2)
};
```

#### Non-Member Friend Function (Symmetric operations)

```cpp
class Vector {
    double x, y;
public:
    Vector(double x, double y) : x(x), y(y) {}
    
    // Friend can access private members
    friend Vector operator+(const Vector& lhs, const Vector& rhs) {
        return Vector(lhs.x + rhs.x, lhs.y + rhs.y);
    }
    
    // Scalar multiplication needs non-member for both orders
    friend Vector operator*(const Vector& v, double scalar) {
        return Vector(v.x * scalar, v.y * scalar);
    }
    
    friend Vector operator*(double scalar, const Vector& v) {
        return v * scalar;  // Reuse above
    }
};

// Now both work:
Vector v(1, 2);
Vector v2 = v * 3;    // OK
Vector v3 = 3 * v;    // Also OK! (only with non-member)
```

**Rule:** Use non-member for symmetric binary operators (especially with different types)

### Comparison Operators (overloaded)

```cpp
class Fraction {
private:
    int numerator, denominator;
    
public:
    Fraction(int n, int d = 1) : numerator(n), denominator(d) {}
    
    // Implement == and <, others can be derived
    bool operator==(const Fraction& other) const {
        return numerator * other.denominator == 
               other.numerator * denominator;
    }
    
    bool operator!=(const Fraction& other) const {
        return !(*this == other);  // Reuse ==
    }
    
    bool operator<(const Fraction& other) const {
        return numerator * other.denominator < 
               other.numerator * denominator;
    }
    
    bool operator<=(const Fraction& other) const {
        return *this < other || *this == other;
    }
    
    bool operator>(const Fraction& other) const {
        return !(*this <= other);
    }
    
    bool operator>=(const Fraction& other) const {
        return !(*this < other);
    }
};

// C++20: Spaceship operator simplifies this!
// auto operator<=>(const Fraction& other) const = default;
```

### Assignment Operator - CRITICAL

```cpp
class String {
private:
    char* data;
    size_t length;
    
public:
    // Constructor
    String(const char* str = "") {
        length = std::strlen(str);
        data = new char[length + 1];
        std::strcpy(data, str);
    }
    
    // Copy constructor
    String(const String& other) : length(other.length) {
        data = new char[length + 1];
        std::strcpy(data, other.data);
    }
    
    // Copy assignment operator - CRITICAL PATTERN
    String& operator=(const String& other) {
        // 1. Check self-assignment
        if (this == &other) {
            return *this;
        }
        
        // 2. Free existing resource
        delete[] data;
        
        // 3. Copy from other
        length = other.length;
        data = new char[length + 1];
        std::strcpy(data, other.data);
        
        // 4. Return *this
        return *this;
    }
    
    // Move assignment (C++11)
    String& operator=(String&& other) noexcept {
        if (this != &other) {
            delete[] data;
            
            // Steal resources
            data = other.data;
            length = other.length;
            
            // Leave other in valid state
            other.data = nullptr;
            other.length = 0;
        }
        return *this;
    }
    
    // Destructor
    ~String() {
        delete[] data;
    }
};
```

**CRITICAL: Copy assignment must:**

1. Check self-assignment (`if (this == &other)`)
2. Free existing resources
3. Copy from source
4. Return `*this`

**Self-assignment matters:**

```cpp
String s("hello");
s = s;  // Without check, deletes data before copying from it!
```

### Stream Operators (<<, >>)

```cpp
class Point {
private:
    int x, y;
    
public:
    Point(int x = 0, int y = 0) : x(x), y(y) {}
    
    // Output operator - MUST be non-member
    friend std::ostream& operator<<(std::ostream& os, const Point& p) {
        os << "(" << p.x << ", " << p.y << ")";
        return os;  // Enable chaining
    }
    
    // Input operator
    friend std::istream& operator>>(std::istream& is, Point& p) {
        is >> p.x >> p.y;
        return is;  // Enable chaining
    }
};

// Usage
Point p(3, 4);
std::cout << "Point: " << p << std::endl;  // Point: (3, 4)

Point p2;
std::cin >> p2;  // Input: 5 6
std::cout << p2; // Output: (5, 6)
```

**Must be non-member** because left operand is stream, not your object.

### Subscript Operator []

```cpp
class Array {
private:
    int* data;
    size_t size;
    
public:
    Array(size_t s) : size(s), data(new int[s]) {}
    
    ~Array() { delete[] data; }
    
    // Non-const version - allows modification
    int& operator[](size_t index) {
        return data[index];  // No bounds checking (like built-in array)
    }
    
    // Const version - for const objects
    const int& operator[](size_t index) const {
        return data[index];
    }
    
    // Bounds-checked version (call this at() instead)
    int& at(size_t index) {
        if (index >= size) {
            throw std::out_of_range("Index out of bounds");
        }
        return data[index];
    }
};

Array arr(10);
arr[5] = 42;           // Calls non-const operator[]
int x = arr[5];        // Also non-const version

const Array carr(5);
int y = carr[2];       // Calls const operator[]
// carr[2] = 10;       // ERROR - returns const reference
```

### Function Call Operator () - Functors

```cpp
class Multiplier {
private:
    int factor;
    
public:
    Multiplier(int f) : factor(f) {}
    
    // Function call operator
    int operator()(int value) const {
        return value * factor;
    }
};

Multiplier times3(3);
int result = times3(10);  // 30 - looks like function call!

// Used with STL algorithms
std::vector<int> nums = {1, 2, 3, 4, 5};
std::transform(nums.begin(), nums.end(), nums.begin(), times3);
// nums is now {3, 6, 9, 12, 15}
```

**Use cases:**

- Predicates for algorithms
- Callbacks with state
- Custom comparators

### Increment/Decrement Operators

```cpp
class Counter {
private:
    int value;
    
public:
    Counter(int v = 0) : value(v) {}
    
    int getValue() const { return value; }
    
    // Prefix increment: ++counter
    Counter& operator++() {
        ++value;
        return *this;  // Return reference
    }
    
    // Postfix increment: counter++
    Counter operator++(int) {  // int is dummy parameter
        Counter temp(*this);   // Save old value
        ++value;               // Increment
        return temp;           // Return old value
    }
    
    // Prefix decrement: --counter
    Counter& operator--() {
        --value;
        return *this;
    }
    
    // Postfix decrement: counter--
    Counter operator--(int) {
        Counter temp(*this);
        --value;
        return temp;
    }
};

Counter c(5);
++c;           // c is 6, returns reference to c
Counter old = c++;  // old is 6, c is 7
```

**CRITICAL DIFFERENCE:**

- **Prefix** (`++x`): Increment then return - efficient (returns reference)
- **Postfix** (`x++`): Copy, increment, return copy - less efficient

**Best practice:** Prefer prefix for custom types (more efficient).

### Arrow Operator -> (Smart Pointers)

```cpp
class SmartPointer {
private:
    int* ptr;
    
public:
    SmartPointer(int* p) : ptr(p) {}
    
    ~SmartPointer() {
        delete ptr;
    }
    
    // Dereference operator
    int& operator*() const {
        return *ptr;
    }
    
    // Arrow operator
    int* operator->() const {
        return ptr;  // Return raw pointer
    }
};

class Data {
public:
    int value;
    void print() const { std::cout << value << std::endl; }
};

SmartPointer sp(new Data{42});
sp->value = 100;    // Uses operator->
sp->print();        // Uses operator->
(*sp).value = 200;  // Uses operator*
```

### Type Conversion Operators

```cpp
class Fraction {
private:
    int numerator, denominator;
    
public:
    Fraction(int n, int d = 1) : numerator(n), denominator(d) {}
    
    // Conversion to double
    operator double() const {
        return static_cast<double>(numerator) / denominator;
    }
    
    // Explicit conversion (C++11) - prevents implicit conversion
    explicit operator bool() const {
        return numerator != 0;
    }
};

Fraction f(3, 4);
double d = f;           // Implicitly converts to 0.75

if (f) {                // OK - explicit context
    // ...
}

// bool b = f;          // ERROR with explicit - must cast explicitly
bool b = static_cast<bool>(f);  // OK
```

### Operators You CANNOT Overload

- `::` (scope resolution)
- `.` (member access)
- `.*` (pointer-to-member access)
- `?:` (ternary conditional)
- `sizeof`
- `typeid`

### Best Practices and Pitfalls

#### **DO**

```cpp
// 1. Maintain semantic consistency
Complex a, b;
a + b;  // Should act like mathematical addition

// 2. Implement related operators together
// If you define ==, define !=
// If you define <, define >, <=, >=

// 3. Return appropriate types
// Arithmetic: return by value
// Compound assignment: return reference to *this
// Comparison: return bool

// 4. Mark as const when doesn't modify
const Complex operator+(const Complex& other) const;

// 5. Use non-member for symmetric operations
friend Complex operator+(const Complex& lhs, const Complex& rhs);
```

#### **DON'T**

```cpp
// 1. Don't violate expectations
// Don't make + do subtraction!

// 2. Don't overload && and || (loses short-circuit evaluation)

// 3. Don't return dangling references
String& operator+(const String& other) {
    String result = *this;
    result += other;
    return result;  // BAD! Returns reference to local variable
}

// 4. Don't forget self-assignment check in operator=

// 5. Don't make assignment return void
// Return *this for chaining: a = b = c;
```

### Complete Example - Vector Class

```cpp
class Vector2D {
private:
    double x, y;
    
public:
    // Constructor
    Vector2D(double x = 0, double y = 0) : x(x), y(y) {}
    
    // Arithmetic operators
    Vector2D operator+(const Vector2D& v) const {
        return Vector2D(x + v.x, y + v.y);
    }
    
    Vector2D operator-(const Vector2D& v) const {
        return Vector2D(x - v.x, y - v.y);
    }
    
    // Scalar multiplication
    Vector2D operator*(double scalar) const {
        return Vector2D(x * scalar, y * scalar);
    }
    
    friend Vector2D operator*(double scalar, const Vector2D& v) {
        return v * scalar;
    }
    
    // Dot product
    double operator*(const Vector2D& v) const {
        return x * v.x + y * v.y;
    }
    
    // Compound assignment
    Vector2D& operator+=(const Vector2D& v) {
        x += v.x;
        y += v.y;
        return *this;
    }
    
    // Comparison
    bool operator==(const Vector2D& v) const {
        return x == v.x && y == v.y;
    }
    
    bool operator!=(const Vector2D& v) const {
        return !(*this == v);
    }
    
    // Unary minus
    Vector2D operator-() const {
        return Vector2D(-x, -y);
    }
    
    // Stream operators
    friend std::ostream& operator<<(std::ostream& os, const Vector2D& v) {
        os << "(" << v.x << ", " << v.y << ")";
        return os;
    }
    
    // Subscript (0 = x, 1 = y)
    double& operator[](size_t index) {
        return index == 0 ? x : y;
    }
    
    const double& operator[](size_t index) const {
        return index == 0 ? x : y;
    }
};

// Usage
Vector2D v1(3, 4);
Vector2D v2(1, 2);
Vector2D v3 = v1 + v2;           // (4, 6)
Vector2D v4 = v1 * 2;            // (6, 8)
Vector2D v5 = 3 * v1;            // (9, 12)
double dot = v1 * v2;            // 11
std::cout << v1;                 // (3, 4)
v1[0] = 10;                      // x = 10
```

---

## Quick Reference

| Feature | Use When |
|---------|----------|
| `const` | Value won't change, type safety needed |
| `constexpr` | Compile-time constant, optimization |
| C-style array | Fixed size, performance-critical |
| `std::array` | Fixed size, want STL benefits |
| `std::vector` | Dynamic size, need flexibility |
| `if-else` | Complex conditions, ranges |
| `switch` | Multiple discrete values |
| `for` loop | Known iteration count |
| `while` loop | Unknown iteration count |
| `do-while` | Must execute at least once |
| C-string | Legacy code, low-level work |
| `std::string` | General string manipulation (preferred) |
| Pass by value | Small types, need copy |
| Pass by reference | Large types, need to modify |
| Pass by const ref | Large types, read-only (default choice) |
| Pass by pointer | Optional parameters, C APIs |
| Smart pointers | Dynamic memory (always prefer) |
| Raw pointers | Non-owning, C APIs, observing only |
| References | Aliases, parameters, cannot be null |
| `private` | Data members (default), implementation details |
| `public` | Interface methods, what users access |
| `protected` | For derived classes only |
| Virtual destructor | Base classes with virtual functions (CRITICAL) |
| Pure virtual | Abstract classes/interfaces |
| `override` | Mark overridden virtual functions (C++11) |
| Rule of Zero | Use smart pointers, avoid defining special members |
| Rule of Three | Define destructor, copy constructor, copy assignment |
| Rule of Five | Add move constructor and move assignment (C++11) |
| Member operator | Left operand is object (`a + b` → `a.operator+(b)`) |
| Non-member operator | Symmetric operations, different types |
| Friend function | Needs access to private members |
| `explicit` constructor | Prevent implicit conversions |
| Const member function | Mark all non-modifying methods |
