- A **default constructor** is a constructor that accepts no arguments. Typically, this is a constructor that has been defined with no parameters.

```cpp
#include <iostream>

class Foo
{
public:
    Foo() // default constructor
    {
        std::cout << "Foo default constructed\n";
    }
};

int main()
{
    Foo foo{}; // No initialization values, calls Foo's default constructor

    return 0;
}
```

- If a class type has a default constructor, both value initialization and default initialization will call the default constructor. Thus, for such a class such as the `Foo` class in the example above, the following are essentially equivalent:

```cpp
Foo foo{}; // value initialization, calls Foo() default constructor
Foo foo2;  // default initialization, calls Foo() default constructor
```

```ad-important
If all of the parameters in a constructor have default arguments, the constructor is a default constructor (because it can be called with no arguments).
```

```ad-note
If a non-aggregate class type object has no user-declared constructors, the compiler will generate a public default constructor (so that the class can be value or default initialized). This constructor is called an **implicit default constructor**.
```

---

### Using `= default` to generate an explicitly defaulted default constructor

- In cases where we would write a default constructor that is equivalent to the implicitly generated default constructor, we can instead tell the compiler to generate a default constructor for us. This constructor is called an **explicitly defaulted default constructor**, and it can be generated by using the `= default` syntax:

```cpp
#include <iostream>

class Foo
{
private:
    int m_x {};
    int m_y {};

public:
    Foo() = default; // generates an explicitly defaulted default constructor

    Foo(int x, int y)
        : m_x { x }, m_y { y }
    {
        std::cout << "Foo(" << m_x << ", " << m_y << ") constructed\n";
    }
};

int main()
{
    Foo foo{}; // calls Foo() default constructor

    return 0;
}
```
