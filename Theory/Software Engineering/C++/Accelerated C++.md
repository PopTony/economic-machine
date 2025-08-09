# Accelerated C++ Notes

## Core C++ Concepts

### Default Parameters
In C++, you cannot specify the default parameter value both in the function declaration and the function definition (implementation). The default value can only be specified once—and it must be in the function declaration.

### Function Definitions

#### Regular (Concrete) Function Definition
```cpp
function(){}; 
```
This syntax is a concrete function definition. It defines a function with an empty body (or with actual implementation if provided). This function can be called by objects of the class it belongs to, and it will execute the code inside the braces `{}`.

#### Pure Virtual Function (Abstract Function)
```cpp
function() = 0; 
```
This syntax is used to define a pure virtual function in a class, which is also called an abstract function. A pure virtual function has no implementation in the base class and must be overridden in any derived class. The `= 0` marks the function as pure virtual, meaning it's meant to be overridden in subclasses.

### Access Control

#### Private Access Meaning
```cpp
class Widget {
public:
    void swap(Widget& other) {
        using std::swap;
        swap(pImpl, other.pImpl);  // This is allowed even though pImpl is private
    }

private:
    int* pImpl;  // Private member
};
```
Private access is within the class, not within object. The above code works because private members are accessible to all instances of the same class.

### Header Guards
```cpp
#ifndef Guard_header_name  
// Header content here
#endif
```
Use header guards to avoid multiple inclusion - wrap the entire header file in such directive.

## Standard Library Components

### Containers

#### Vector
- Dynamic array optimized for random access
- Expensive for delete and insert operations
- Even `push_back()` may reallocate the entire vector to make space for new elements, invalidating all existing pointers
- **const_iterator**: Creates a pointer to the container, but cannot modify the container
- **Erase usage**: `iter = students.erase(iter);` - after erase, the iterator returns the pointer to the next element

#### List
- Fast for deletion and insertion in the middle
- More complex structure than vector
- Doesn't support indexing
- Deletion and insertion only invalidate the targeted index pointer
- Standard sort library doesn't support list sort. List needs to use `list_example.sort()`

#### Const References
```cpp
const vector<double>& chw = homework;
```
The `const` refers to the vector object, meaning the pointer `chw` cannot modify the object.

### Iterators

#### Types of Iterators
- **Bidirectional iterator**: Double-linked list (`std::list`), binary search trees (`std::map`, `std::set`)
- **Random access iterator**: The only option to use sort generic function. List iterator is not supported as it only has bidirectional iterators
- **Iterator philosophy**: Algorithms achieve data-structure independence by using iterators as the glue between algorithms and containers

#### Iterator Operations
```cpp
v.rbegin()          // Right beginning, move from right to left in the container
back_inserter(v)    // Automatically handles resizing when new elements are added
```

### Generic Algorithms

#### Find and Remove Operations
```cpp
find_if(first, last, condition)
remove_if(a.begin(), a.end(), condition)  // Relative order preserved, physical size unchanged
remove_copy_if(students.begin(), students.end(), back_inserter(fail), pgrade)
vec.erase(remove_if(vec.begin(), vec.end(), condition), vec.end())  // Complete removal
```

**Important**: Algorithms act on container elements, they do not act on containers. `vec.erase()` acts on the container because erase is a member of the container.

#### Partitioning
Similar functions: `partition()`, `stable_partition()`

### Associated Containers

#### Map - Associated Array
- Good for key-value lookup
- Support iterator for iteration: `it->first`, `it->second`
- Uses bidirectional iterators

## Memory Management

### Static vs Dynamic Allocation

#### Static Allocation
```cpp
int* pointer_to_static() { 
    static int x; 
    return &x; 
}
```
**Disadvantage**: Every call returns a pointer to the same object.

#### Dynamic Allocation
```cpp
new T        // Allocates single object of type T
new T[n]     // Allocates array of n objects, runs default constructor for T
```

### The Rule of Three
If your class needs a destructor, it probably needs a copy constructor and an assignment operator too.

### Custom Memory Management
```cpp
// Vec implementation strategy:
// - When push_back needs more space, allocate twice as much storage
// - Keep track of first element and two "end" pointers:
//   1. Pointer to first available element
//   2. Pointer to last allocated element
```

#### Memory Allocation with `allocator<T>`
The `<memory>` header provides `allocator<T>` class that:
- Allocates uninitialized memory intended for objects of type T
- Returns pointer to initial element
- Allows custom initialization (unlike `new` which always initializes)

#### Memory Management Operations
- **Uncreate**: destroy objects, then deallocate memory (two steps)
- **Grow**: allocate new doubled-sized memory space, uncreate old space

## Object-Oriented Programming

### Const Member Functions
```cpp
double Student_info::grade() const { ... }
```
Const member functions may not change the internal state of the object on which they are executing.

### Function Pointers
```cpp
int (*fp)(int)  // Pointer to function taking int, returning int
typedef double (*analysis_fp)(const vector<Student_info>&);  // Typedef for function pointer
analysis_fp get_analysis_ptr();  // Function returning function pointer
```

### Explicit Constructors
```cpp
class MyClass {
public:
    explicit MyClass(int value) { /* constructor implementation */ }
};

void functionTakingMyClass(MyClass obj) { /* function implementation */ }

int main() {
    functionTakingMyClass(5);           // Error: cannot convert 'int' to 'MyClass'
    functionTakingMyClass(MyClass(5));  // Correct: explicit conversion
}
```

### Operators

#### Index Operator
```cpp
T& operator[](size_type i) { return data[i]; }
const T& operator[](size_type i) const { return data[i]; }  // Const version
```
The index operator must be a member function. Need both const and non-const versions.

#### Size Function
```cpp
size_type size() const { return limit - data; }
```

### Conversions
Conversions are defined by:
1. Non-explicit constructors that take a single argument
2. Conversion operators: `operator type-name() { return ...; }`

**Note**: Conversion operators must be members. If two classes define conversions to each other's types, ambiguities can result.

### Friend Functions
```cpp
class Box {
private:
    double width;
public:
    Box(): width(0) {}
    void setWidth(double wid) { width = wid; }
    
    // Friend function declaration
    friend void printWidth(Box box);
};

// Friend function definition
void printWidth(Box box) {
    std::cout << "Width of box : " << box.width << std::endl;
}
```

### Virtual Functions and Inheritance

#### Virtual Function Rules
- **Static binding**: When calling virtual function on an object (compile time)
- **Dynamic binding**: When calling virtual function through pointer or reference (runtime)
- Virtual keyword only needed in base class declaration, not in derived class
- Virtual functions must be defined

#### Class Structure
```cpp
class base {
public:
    virtual ~base() { }  // Virtual destructor
    // Common interface 
protected: 
    // Implementation members accessible to derived classes 
private: 
    // Implementation accessible to only the base class 
};
```

### Stream Operations

#### Stream Types
- **clog**: Intended for logging, buffered like cout
- **cerr**: Always writes output immediately, guarantees visibility but with overhead

#### Array Operations
```cpp
sizeof(numbers) / sizeof(*numbers)  // Number of elements in array
```

### Member Initialization
Member variables that are not primitive types or don't have default constructors must be initialized in the constructor initializer list rather than in the constructor body.

```cpp
// Correct initialization in initializer list
MyClass::MyClass() : m_orderValidator(params), m_tradeValidator(params) {
    // Constructor body
}
```

### String Operations
```cpp
// When writing: s = "hello";
// Compiler uses Str(const char*) constructor to create unnamed temporary
// Then calls assignment operator to assign temporary to s
```

## Practical C++ Libraries

### UML Class Relationships
- **Inheritance**: Extend functionality of one class to another class
- **Realization/Implementation**: Class implements an interface
- **Composition**: One class object contains another class object with same lifetime
- **Aggregation**: One class contains another, but objects can exist independently
- **Association**: Simple connections, one class holds reference to another

### Pure Virtual Functions
```cpp
virtual void setSymbols(std::vector<std::string> symbols) = 0;
```
With `= 0`, it's a pure virtual function that must be implemented in derived class.

**Note**: Virtual functions can be implemented in the base class. Virtual functions enable dynamic binding.

### Build Tools
- **SConstruct**: Build configuration files
- **GDB Core Dump**: Debugging crashed processes
- **Logger Flush**: Logging info in buffer may be lost on crash if not flushed

### Smart Pointers
```cpp
std::unique_ptr<int> ptr = std::make_unique<int>(42);
// Automatically freed when ptr goes out of scope
```

### Atomic Operations
- **Lock-free programming**: Algorithms that operate correctly in concurrent environment without traditional locking
- **C++ atomic support**: `std::atomic` template and associated atomic operations
- **Implementation**: Uses hardware support (compare-and-swap, fetch-and-add, load-linked/store-conditional)
- **Constraints**: Atomic data types don't allow virtual functions and must be in continuous memory
- **Memory ordering**: `std::memory_order_relaxed` reduces synchronization overhead

### Threading
```cpp
// Create two threads
std::thread thread1(threadFunction1);
std::thread thread2(threadFunction2);

// Let threads run concurrently
// Main thread can optionally do other work

// Wait for both threads to finish
thread1.join();  // Block current thread until thread1 finishes
thread2.join();  // Block current thread until thread2 finishes
```

**join()**: Blocks current thread until the calling thread is finished.

## Network Programming

### ZeroMQ
#### Bind vs Connect
- **Bind**: From the most stable points in your architecture
- **Connect**: From dynamic components with volatile endpoints

#### Subscriber vs Publisher
- The address publisher binds to is typically the same as the address subscriber connects to (unless there's intermediary routing)
- Publisher sends messages to the address that subscriber connects to

#### Asynchronous I/O
**Downside**: It takes time for subscribers to connect to publisher. During that time, any messages the publisher sends will be lost.

## JSON Processing

### RapidJSON
```cpp
Value a; 
a = { "key": "value" };
a["key"].GetString();  // Returns character string

// For comparison with string literals
strcmp(a["key"].GetString(), "ABC");  // Use strcmp for C-style strings

// Or convert to std::string
std::string x = a["key"].GetString();  // Now can use == for comparison
```

