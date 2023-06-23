# Exception Handling in C++
A brief overview of exception handling and how it is used in C++ 
## Exception Handling
- What is an exception?
- We want to handle these exceptions because they can cause crashes
- Exception handling allows recovery from errors and for execution to continue
## Exception Handling in C++
Here is what exception handling generally looks like in C++:
```cpp
try 
{
    // Code that might throw an exception
    // ...
    if (/* condition */) 
    {
        throw Exception();  // Explicitly throw an exception
    }
    // ...
}
catch (const std::exception& e) 
{
    // Handle exception e
    // ...
}
catch (...) 
{
    // Catch-all block for any other exception types
    // ...
}
```
The important keywords here are `try`, `throw`, and `catch`.

- `try` - this block contains the code that may throw an exception
- `throw` - explicitly throw an exception in the `try` block
- `catch` - this block specifices the type of exception to be caught

### The std::exception class

These are some of the exceptions included in `std::exception`:

![image](https://github.com/goutham-st/brown-bag/assets/78628850/304be370-bfca-407c-8f41-04adcb6419f8)

### Division by 0 example
```cpp
#include <iostream>

int main() 
{
    int numerator, denominator;
    std::cout << "Enter numerator: ";
    std::cin >> numerator;
    std::cout << "Enter denominator: ";
    std::cin >> denominator;

    try 
    {
        if (denominator == 0) 
        {
            throw std::runtime_error("Division by zero is undefined.");
        }
        double result = static_cast<double>(numerator) / denominator;
        std::cout << "Result: " << result << std::endl;
    }
    catch (const std::exception& ex) 
    {
        std::cerr << "Exception caught: " << ex.what() << std::endl;
    }

    return 0;
}
```
Running the above code with the numerator, 5, and the denominator, 0, gives:
```
Enter numerator: 5
Enter denominator: 0
Exception caught: Division by zero is not allowed.
```
### File I/O example
``` cpp
#include <iostream>
#include <fstream>
#include <string>

int main() 
    {
    std::string filename;
    std::cout << "Enter filename: ";
    std::cin >> filename;

    try 
    {
        std::ifstream file(filename);
        if (!file) 
        {
            throw std::runtime_error("Failed to open file: " + filename);
        }
        // Process the file...
        std::cout << "File opened successfully!" << std::endl;
    }
    catch (const std::exception& ex) 
    {
        std::cerr << "Exception caught: " << ex.what() << std::endl;
    }

    return 0;
}
```
### User-defined Exceptions
It also might be useful to define new exceptions which can be done by inheriting and overriding from `std::exception`.
```cpp
#include <iostream>

class MyException : public std::exception 
{
public:
    const char* what() const throw()
    {
        return "Custom exception.";
    }
};

int main() 
{
    try 
    {
        throw MyException();
    }
    catch (const std::exception& ex) 
    {
        std::cerr << "Exception caught: " << ex.what() << std::endl;
    }

    return 0;
}
```
### Best Practices
- Only handle exceptional conditions such as runtime errors
- Throw exceptions by value and catch exceptions by const reference
- Catch specific exceptions
- Use standard exception types
- Use RAII (Resource Acquisition Is Initialization)

### Considerations
- Performance overhead (runtime checks and stack unwinding)
- Resource leaks
- Multithreading

## Sources

- https://www.programiz.com/cpp-programming/exception-handling
- https://learn.microsoft.com/en-us/cpp/cpp/errors-and-exception-handling-modern-cpp?view=msvc-170
- https://en.cppreference.com/w/cpp/error/exception
