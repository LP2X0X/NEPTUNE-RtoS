- [[Variable]] defined inside the body of a [[Function|function]] are called **local variable**. [[Parameter]] is also generally considered to be a local variable.

### Local variable lifetime
- A local variable’s scope begins at the point of variable definition, and stops at the end of the set of curly braces in which it is defined (or for function parameters, at the end of the function).

```cpp
#include <iostream>

// x is not in scope anywhere in this function
void doSomething()
{
    std::cout << "Hello!\n";
}

int main()
{
    // x can not be used here because it's not in scope yet

    int x{ 0 }; // x enters scope here and can now be used within this function

    doSomething();

    return 0;
} // x goes out of scope here and can no longer be used
```

```ad-info
Best practice: Define your local variables as close to their first use as reasonable.
```