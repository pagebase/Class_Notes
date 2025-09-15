# Table of contents

- [Installation](#installation)

---
# Installation

To **run C# in Visual Studio Code (VS Code)**, follow these steps:

---

## ✅ 1. Prerequisites

### 🔹 Install .NET SDK

- Go to: [https://dotnet.microsoft.com/download](https://dotnet.microsoft.com/download)
    
- Download and install the **latest .NET SDK (not just the runtime)**.
    
- After installation, verify it works:
    

```bash
dotnet --version
```

### 🔹 Install Visual Studio Code

- Download from: [https://code.visualstudio.com](https://code.visualstudio.com/)
    

### 🔹 Install C# Extension

- Open VS Code
    
- Go to Extensions (`Ctrl+Shift+X`)
    
- Search for **"C#"** by **Microsoft** and install it (part of **OmniSharp**).
    

---

## ✅ 2. Create and Run a C# Program

### 🔹 Step 1: Create a Console App

Open a terminal (either inside VS Code or separately) and run:

```bash
dotnet new console -o MyFirstApp
cd MyFirstApp
```

This creates a new folder with a simple C# project (`Program.cs`).

### 🔹 Step 2: Open the Folder in VS Code

```bash
code .
```

Or manually open the `MyFirstApp` folder in VS Code.

---

### 🔹 Step 3: Run the Code

#### Option 1: Using Terminal

In VS Code, open the integrated terminal (`Ctrl+``) and run:

```bash
dotnet run
```

This compiles and runs `Program.cs`.

#### Option 2: Use Run/Debug

- Open `Program.cs`
    
- Click on **Run > Start Debugging (F5)** or the green play button
    
- This works if the C# extension is installed and `.vscode/launch.json` is configured (usually auto-generated)
    

---

## ✅ 3. Sample Program

Here’s a very basic C# program:

```csharp
using System;

class Program
{
    static void Main()
    {
        Console.WriteLine("Hello, world!");
    }
}
```

Run it with:

```bash
dotnet run
```

---

## 💡 Optional: C# Interactive or Script Mode

If you want to run C# **scripts** (`.csx` files) without creating a full project, install the **.NET Script** tool:

```bash
dotnet tool install -g dotnet-script
```

Then create a file like `hello.csx`:

```csharp
Console.WriteLine("Hello from script!");
```

Run it with:

```bash
dotnet script hello.csx
```

---

# Variables and data types

## 🔹 What is a Variable?

A **variable** is a named storage location in memory that holds a value.  
In C#, you **declare a variable** by specifying the **data type**, followed by the **variable name**, and optionally assign a **value**.

### 📌 Syntax:

```csharp
<data_type> <variable_name> = <value>;
```

### ✅ Example:

```csharp
int age = 25;
string name = "Alice";
```

---

## 🔹 Basic Data Types in C#

C# has two broad categories of data types:

### 1. **Value Types** (store data directly in memory)

|Data Type|Example Value|Description|
|---|---|---|
|`int`|`42`|Integer numbers|
|`double`|`3.14`|Double-precision floating-point number|
|`float`|`2.5f`|Single-precision float (requires `f` suffix)|
|`bool`|`true`, `false`|Boolean (true/false)|
|`char`|`'A'`|A single character (in single quotes)|
|`byte`|`255`|8-bit unsigned integer|
|`short`|`-32000`|16-bit signed integer|
|`long`|`1234567890L`|64-bit signed integer (use `L` suffix)|
|`decimal`|`10.99m`|High-precision decimal (e.g., for money)|

### 2. **Reference Types** (store a reference to the memory location)

|Data Type|Example Value|Description|
|---|---|---|
|`string`|`"Hello"`|A sequence of characters (text)|
|`object`|`any value`|The base type of all other types|
|`dynamic`|varies at runtime|Type resolved at runtime|

---

## 🔹 Variable Declaration Examples

```csharp
int age = 30;
double pi = 3.14159;
float temperature = 36.6f;
bool isAlive = true;
char initial = 'C';
string greeting = "Hello, world!";
decimal price = 19.99m;
```

---

## 🔹 Implicit Typing (`var`)

You can use `var` when the compiler can infer the type from the right-hand side:

```csharp
var count = 100;        // inferred as int
var name = "Charlie";   // inferred as string
```

> ⚠️ Once assigned, `var` is strongly typed — you can't change its type later.

---

## 🔹 Constants

Use `const` for variables that **never change**:

```csharp
const double Pi = 3.14159;
```

---

## 🔹 Nullable Types

Value types (like `int`) can’t be null by default. To allow `null`, use `?`:

```csharp
int? age = null;
bool? isActive = null;
```

---

## 🧪 Quick Practice

What do you think the output will be?

```csharp
int a = 10;
double b = 5.5;
Console.WriteLine(a + b);
```

✅ Output:

```
15.5
```

---

# Elvis operator

The **Elvis operator** in C# is shorthand for handling **null values**, and it's represented as:

```csharp
?: 
```

However, in modern C#, the more commonly referred "Elvis operator" is:

### 🔹 `??` (Null-Coalescing Operator)

This operator returns the **left-hand operand if it's not null**, otherwise it returns the **right-hand operand**.

---

## ✅ Syntax:

```csharp
var result = value ?? fallbackValue;
```

---

### 📌 Example:

```csharp
string name = null;
string displayName = name ?? "Guest";

Console.WriteLine(displayName);  // Output: Guest
```

---

### 🔹 Also Related: Null-Conditional Operator (`?.`)

This is another operator often grouped under the "Elvis operator" nickname because of the way it resembles Elvis' hair (`?.`).

It safely **accesses a member or method only if the object is not null**.

### 📌 Example:

```csharp
Person person = null;
string city = person?.Address?.City; // returns null safely instead of throwing exception
```

Without `?.`, that would throw a `NullReferenceException`.

---

## 🔹 Combine `?.` and `??`

You can chain both for safe access with a default:

```csharp
string city = person?.Address?.City ?? "Unknown";
```

---

## Summary of Elvis-Like Operators

|Operator|Purpose|Example|Meaning|
|---|---|---|---|
|`??`|Null-coalescing|`x ?? y`|If `x` is null, use `y`|
|`?.`|Null-conditional access|`obj?.Prop`|If `obj` is not null, access `Prop`|
|`??=`|Null-coalescing assignment (C# 8+)|`x ??= y`|Assign `y` to `x` only if `x` is null|

---

# Boxing and unboxing

## 🔹 What is **Boxing**?

> **Boxing** is the process of **converting a value type to an object type** (or any reference type that the value type can be assigned to).

### ✅ Example:

```csharp
int num = 42;
object obj = num;  // Boxing
```

- The integer `num` is boxed — it's wrapped inside an `object` and stored on the **heap** instead of the stack.
    

---

## 🔹 What is **Unboxing**?

> **Unboxing** is the process of **extracting the value type from an object**.

### ✅ Example:

```csharp
object obj = 42;
int num = (int)obj;  // Unboxing
```

- You must **explicitly cast** when unboxing.
    
- If the cast fails (e.g., wrong type), a `InvalidCastException` is thrown.
    

---

## 🔄 Full Boxing and Unboxing Example

```csharp
using System;

class Program
{
    static void Main()
    {
        int value = 100;            // value type
        object boxed = value;       // boxing

        Console.WriteLine(boxed);   // Output: 100

        int unboxed = (int)boxed;   // unboxing
        Console.WriteLine(unboxed); // Output: 100
    }
}
```

---

## ⚠️ Performance Consideration

- **Boxing** causes heap allocation.
    
- Frequent boxing/unboxing can cause **performance issues** and **garbage collection overhead**.
    
- It’s better to avoid boxing in **performance-critical code** (like inside loops).
    

---

## 🔍 When Does Boxing Happen?

- Assigning a value type to an `object` or `System.ValueType`
    
- Passing a value type as a parameter to a method that takes `object`
    
- Using value types with **non-generic collections** like `ArrayList`
    

---

## ✅ Avoid Boxing with Generics

```csharp
ArrayList list = new ArrayList(); // causes boxing
list.Add(123); // boxing occurs

List<int> list2 = new List<int>(); // no boxing
list2.Add(123); // no boxing
```

---

## 🧠 Summary

| Term         | Meaning                                 | Example             |
| ------------ | --------------------------------------- | ------------------- |
| **Boxing**   | Value type → Reference type             | `object obj = 10;`  |
| **Unboxing** | Reference type → Value type (with cast) | `int x = (int)obj;` |

---

# Keywords

## 🔹 What Are Keywords?

**Keywords** in C# are **reserved words** that have **special meaning** in the language. You **cannot use them as variable names** (unless prefixed with `@`). They define the syntax and structure of C# code.

---

## ✅ Categories of C# Keywords

### 1. **Control Flow**

Used for decisions, loops, and branching:

```csharp
if, else, switch, case, default,
for, foreach, while, do,
break, continue, return, goto
```

---

### 2. **Data Types**

Built-in data types:

```csharp
int, float, double, decimal,
bool, char, string, object,
byte, sbyte, short, ushort,
long, ulong, uint, nint, nuint
```

---

### 3. **Access Modifiers**

Control visibility/scope:

```csharp
public, private, protected, internal, protected internal, private protected
```

---

### 4. **Class and Object**

Used for OOP and type definitions:

```csharp
class, struct, interface, enum, record,
new, this, base, null, typeof, nameof, object, var
```

---

### 5. **Exception Handling**

Used for error handling:

```csharp
try, catch, finally, throw
```

---

### 6. **Method and Member Modifiers**

Control how methods and members behave:

```csharp
static, virtual, override, abstract, sealed, async, extern, unsafe, partial, readonly, const, ref, out, in, params, yield
```

---

### 7. **Inheritance and Polymorphism**

For object-oriented programming:

```csharp
is, as, typeof, override, new
```

---

### 8. **Others**

Various functional keywords:

```csharp
namespace, using, return, void, get, set, add, remove, operator, implicit, explicit, checked, unchecked, lock, fixed, delegate, event
```

---

### 9. **Contextual Keywords** (Used in certain contexts only)

These are **not reserved** globally, but act like keywords in certain scenarios:

```csharp
async, await, dynamic, var, yield, partial, where, when, global
```

---

## 🔹 Reserved Keywords (Cannot Be Used As Identifiers)

Full list of C# reserved keywords:

```python
abstract, as, base, bool, break, byte, case, catch, char, checked,
class, const, continue, decimal, default, delegate, do, double, else,
enum, event, explicit, extern, false, finally, fixed, float, for,
foreach, goto, if, implicit, in, int, interface, internal, is, lock,
long, namespace, new, null, object, operator, out, override, params,
private, protected, public, readonly, ref, return, sbyte, sealed, short,
sizeof, stackalloc, static, string, struct, switch, this, throw, true,
try, typeof, uint, ulong, unchecked, unsafe, ushort, using, virtual,
void, volatile, while
```

---

## 🔹 Using Keywords as Variable Names

If you really want to use a keyword as a name, you can prefix it with `@`:

```csharp
int @class = 5;
Console.WriteLine(@class);  // Output: 5
```

> ⚠️ This is legal, but discouraged — it hurts code readability.

---

# Initialization

## 🔹 What Is Initialization?

**Initialization** means **assigning a value to a variable at the time of declaration** (or shortly after). You must initialize a variable **before** using it.

---

## ✅ Syntax

```csharp
<data_type> variableName = value;
```

### 📌 Example:

```csharp
int number = 10;            // initialized to 10
string name = "Alice";      // initialized to "Alice"
bool isReady = true;        // initialized to true
```

---

## 🔹 Types of Initialization

### 1. **Direct Initialization**

Assigning a value directly:

```csharp
int age = 25;
```

---

### 2. **Default Initialization**

When variables are not explicitly initialized, C# assigns **default values** to:

- **Fields in classes and structs**
    
- **Array elements**
    

### 📌 Default values:

|Type|Default Value|
|---|---|
|`int`|`0`|
|`bool`|`false`|
|`string`|`null`|
|`float`|`0.0f`|

```csharp
class Example
{
    int number;   // defaults to 0
    bool isOn;    // defaults to false
}
```

> ⚠️ Local variables **must** be initialized before use — they don’t get default values automatically.

---

### 3. **Object Initialization**

Initialize an object using **object initializer syntax**:

```csharp
class Person
{
    public string Name;
    public int Age;
}

Person p = new Person { Name = "Bob", Age = 30 };
```

---

### 4. **Array Initialization**

```csharp
int[] numbers = new int[] { 1, 2, 3 };
string[] names = { "Alice", "Bob" };
```

---

### 5. **Implicit Initialization (`var`)**

```csharp
var message = "Hello";  // inferred as string
var score = 90;         // inferred as int
```

> Must be initialized immediately — you can't declare `var` without a value.

---

## 🔹 Lazy Initialization

Sometimes you don’t want to initialize something until it's actually needed:

```csharp
Lazy<MyClass> obj = new Lazy<MyClass>(() => new MyClass());
```

---

## 🧠 Summary

|Type|Example|
|---|---|
|Direct|`int x = 5;`|
|Default (fields)|`class MyClass { int x; }`|
|Object initializer|`new Person { Name = "John" }`|
|Array initializer|`int[] arr = { 1, 2, 3 };`|
|Implicit (`var`)|`var name = "ChatGPT";`|

---

# Type inference

## 🔹 What is **Type Inference**?

**Type inference** means the compiler **automatically determines the type of a variable** based on the value you assign to it — so you don't have to explicitly declare the type.

---

## ✅ How Does It Work?

You use the keyword `var` to declare a variable without specifying its type explicitly.

### Syntax:

```csharp
var variableName = value;
```

The compiler infers `variableName`’s type from the type of `value`.

---

## 🔹 Examples

```csharp
var number = 10;           // Compiler infers int
var name = "Alice";        // Compiler infers string
var price = 19.99;         // Compiler infers double
var isValid = true;        // Compiler infers bool
```

---

## Important Notes

- The variable **must be initialized at declaration** when using `var`.
    
- Once inferred, the type is **fixed** and can't be changed.
    
- `var` is **not dynamic typing**; it is strongly typed — just the type is inferred at compile time.
    
- It improves code readability by reducing redundancy but should be used when the type is obvious.
    

---

## 🔹 When to Use Type Inference?

- When the type is clear from the right-hand side (e.g., `var stream = new FileStream(...)`)
    
- To avoid verbose declarations (`Dictionary<string, List<int>>` can become `var`)
    

---

## 🔹 What You **Cannot** Do with `var`

```csharp
var x;          // Error! Must initialize when declaring
var x = null;   // Error! Cannot infer type from null alone
```

---

## 🔹 Example with Collections

```csharp
var numbers = new List<int> { 1, 2, 3 };
var dict = new Dictionary<string, int>();
```

---

## 🧠 Summary

| Explicit Type          | Type Inference Equivalent |
| ---------------------- | ------------------------- |
| `int count = 5;`       | `var count = 5;`          |
| `string name = "Bob";` | `var name = "Bob";`       |

---

# Console input and output

## 🔹 Console Output

You use the **`Console.WriteLine`** and **`Console.Write`** methods to display text or values in the console.

### 1. `Console.WriteLine()`

- Writes text **followed by a newline** (moves to the next line).
    

```csharp
Console.WriteLine("Hello, world!");
Console.WriteLine(123);
```

### 2. `Console.Write()`

- Writes text **without a newline**, so the cursor stays on the same line.
    

```csharp
Console.Write("Hello, ");
Console.Write("world!");
// Output: Hello, world!
```

### 3. String Interpolation (C# 6+)

Easier way to format output:

```csharp
int age = 25;
Console.WriteLine($"I am {age} years old.");
```

---

## 🔹 Console Input

Use **`Console.ReadLine()`** to read a full line of input as a string from the user.

### Example:

```csharp
Console.Write("Enter your name: ");
string name = Console.ReadLine();
Console.WriteLine($"Hello, {name}!");
```

---

## 🔹 Converting Input to Other Types

Since `Console.ReadLine()` returns a string, you often need to convert it:

### Example: Reading an integer

```csharp
Console.Write("Enter your age: ");
string input = Console.ReadLine();
int age = int.Parse(input);  // Converts string to int

Console.WriteLine($"You are {age} years old.");
```

### Safe conversion using `TryParse`:

```csharp
Console.Write("Enter your age: ");
string input = Console.ReadLine();

if (int.TryParse(input, out int age))
{
    Console.WriteLine($"You are {age} years old.");
}
else
{
    Console.WriteLine("Invalid number entered.");
}
```

---

## 🔹 Complete Example: Input and Output

```csharp
using System;

class Program
{
    static void Main()
    {
        Console.Write("What is your favorite color? ");
        string color = Console.ReadLine();

        Console.WriteLine($"Wow! {color} is a great color!");
    }
}
```

---

## Summary

| Task                  | Method                            |
| --------------------- | --------------------------------- |
| Write with newline    | `Console.WriteLine()`             |
| Write without newline | `Console.Write()`                 |
| Read string input     | `Console.ReadLine()`              |
| Convert string to int | `int.Parse()` or `int.TryParse()` |

---

# Operators

## 🔹 **1. Arithmetic Operators**

Used for basic mathematical operations.

| Operator | Description         | Example        |
| -------- | ------------------- | -------------- |
| `+`      | Addition            | `a + b`        |
| `-`      | Subtraction         | `a - b`        |
| `*`      | Multiplication      | `a * b`        |
| `/`      | Division            | `a / b`        |
| `%`      | Modulus (remainder) | `a % b`        |
| `++`     | Increment           | `a++` or `++a` |
| `--`     | Decrement           | `a--` or `--a` |

---

## 🔹 **2. Relational (Comparison) Operators**

Used to compare two values.

| Operator | Description      | Example  |
| -------- | ---------------- | -------- |
| `==`     | Equal to         | `a == b` |
| `!=`     | Not equal to     | `a != b` |
| `>`      | Greater than     | `a > b`  |
| `<`      | Less than        | `a < b`  |
| `>=`     | Greater or equal | `a >= b` |
| `<=`     | Less or equal    | `a <= b` |

---

## 🔹 **3. Logical Operators**

Used to combine multiple boolean expressions.

| Operator | Description | Example  |
| -------- | ----------- | -------- |
| `&&`     | Logical AND | `a && b` |
| `        |             | `        |
| `!`      | Logical NOT | `!a`     |

---

## 🔹 **4. Assignment Operators**

Used to assign values to variables.

| Operator | Description         | Example  |
| -------- | ------------------- | -------- |
| `=`      | Assign              | `a = b`  |
| `+=`     | Add and assign      | `a += b` |
| `-=`     | Subtract and assign | `a -= b` |
| `*=`     | Multiply and assign | `a *= b` |
| `/=`     | Divide and assign   | `a /= b` |
| `%=`     | Modulus and assign  | `a %= b` |

---

## 🔹 **5. Bitwise Operators**

Operate on bits (binary representation).

|Operator|Description|Example|
|---|---|---|
|`&`|AND|`a & b`|
|`|`|OR|
|`^`|XOR|`a ^ b`|
|`~`|NOT (1's complement)|`~a`|
|`<<`|Left shift|`a << 1`|
|`>>`|Right shift|`a >> 1`|

---

## 🔹 **6. Conditional Operator (Ternary)**

Shortcut for `if-else`.

```csharp
condition ? trueResult : falseResult
```

**Example:**

```csharp
int max = (a > b) ? a : b;
```

---

## 🔹 **7. Null-Coalescing Operators**

Used for null checking.

|Operator|Description|Example|
|---|---|---|
|`??`|Returns left if not null, else right|`a ?? b`|
|`??=`|Assign right if left is null|`a ??= b`|

---

## 🔹 **8. Type Testing and Casting Operators**

|Operator|Description|Example|
|---|---|---|
|`is`|Check type|`obj is string`|
|`as`|Safe cast (returns null)|`obj as string`|
|`typeof`|Get type|`typeof(string)`|
|`sizeof`|Get size in bytes|`sizeof(int)`|

---

## 🔹 **9. Other Operators**

|Operator|Description|Example|
|---|---|---|
|`new`|Create object|`new MyClass()`|
|`.`|Member access|`obj.Property`|
|`[]`|Indexer|`arr[0]`|
|`()`|Method invocation|`Method()`|
|`=>`|Lambda expression|`x => x * 2`|
|`$""`|String interpolation|`$"Hello {name}"`|

---

# Operator precedence

## 🔹 **C# Operator Precedence Table**

Here's a **summary table** of C# operators, from **highest** to **lowest precedence**:

|Precedence|Operator(s)|Category|Associativity|
|---|---|---|---|
|1|`()`, `[]`, `.`, `->`, `++`, `--`|Primary|Left to right|
|2|`++`, `--`, `+`, `-`, `!`, `~`, `(type)`, `await`|Unary|Right to left|
|3|`*`, `/`, `%`|Multiplicative|Left to right|
|4|`+`, `-`|Additive|Left to right|
|5|`<<`, `>>`|Shift|Left to right|
|6|`<`, `<=`, `>`, `>=`, `is`, `as`|Relational / type testing|Left to right|
|7|`==`, `!=`|Equality|Left to right|
|8|`&`|Bitwise AND|Left to right|
|9|`^`|Bitwise XOR|Left to right|
|10|`|`|Bitwise OR|
|11|`&&`|Logical AND|Left to right|
|12|`||`|
|13|`??`|Null-coalescing|Right to left|
|14|`?:`|Conditional (ternary)|Right to left|
|15|`=`, `+=`, `-=`, `*=`, `/=`, `%=` ...|Assignment|Right to left|
|16|`=>`|Lambda|Right to left|

---

## 🔸 **Example:**

```csharp
int result = 5 + 3 * 2;
```

- `*` has **higher precedence** than `+`, so:
    
    - `3 * 2 = 6`
        
    - `5 + 6 = 11`
        

---

## 🔸 **Use Parentheses to Control Order**

You can use parentheses `()` to **override** the default precedence:

```csharp
int result = (5 + 3) * 2;  // result = 16
```

---

## 🔸 **Associativity Example**

```csharp
int a = 5;
int b = 10;
int c = 15;

int result = a = b = c;
```

- `=` has **right-to-left** associativity.
    
- So it's evaluated as: `a = (b = c);`
    
- Both `a` and `b` end up with value `15`.
    

---

# Type conversion

## 🔹 **1. Implicit Conversion (Safe)**

- **Automatic** by the compiler.
    
- No data loss.
    
- Only between **compatible types** (e.g., smaller to larger numeric types).
    

### ✅ Example:

```csharp
int a = 100;
long b = a; // Implicit conversion from int to long
```

---

## 🔹 **2. Explicit Conversion (Casting)**

- **Manual** conversion using a cast.
    
- May cause **data loss** or **runtime exceptions**.
    
- Required when converting between **incompatible types** or from **larger to smaller** types.
    

### 🔁 Example:

```csharp
double x = 9.78;
int y = (int)x;  // Explicit conversion, value becomes 9
```

---

## 🔹 **3. Using Conversion Methods**

C# provides several methods for converting between types safely or conveniently.

### ✅ **Convert Class**

```csharp
string str = "123";
int num = Convert.ToInt32(str);
```

### ✅ **Parse() Method**

```csharp
string str = "456";
int val = int.Parse(str);  // Throws exception if input is invalid
```

### ✅ **TryParse() Method (Recommended)**

```csharp
string input = "789";
bool success = int.TryParse(input, out int result);
if (success)
{
    Console.WriteLine(result);
}
else
{
    Console.WriteLine("Invalid number");
}
```

---

## 🔹 **4. Type Conversion Between Reference Types**

### 🔸 Upcasting (Implicit)

- From derived to base class.
    

```csharp
Dog dog = new Dog();
Animal animal = dog;  // Implicit upcast
```

### 🔸 Downcasting (Explicit)

- From base to derived class.
    

```csharp
Animal animal = new Dog();
Dog dog = (Dog)animal;  // Must cast explicitly
```

You can use `is`, `as`, or pattern matching to safely downcast:

```csharp
if (animal is Dog d)
{
    // safe cast
}
```

---

## 🔹 **5. Boxing and Unboxing**

### 📦 **Boxing** = value type → object (implicit)

```csharp
int num = 42;
object obj = num;
```

### 📤 **Unboxing** = object → value type (explicit)

```csharp
object obj = 42;
int num = (int)obj;
```

---

## 🔹 Summary Table

|Conversion Type|Direction|Example|
|---|---|---|
|Implicit|Smaller → larger|`int → long`|
|Explicit (cast)|Larger → smaller|`double → int`|
|`Convert` class|Any supported types|`Convert.ToInt32("123")`|
|`Parse()`|String → value type|`int.Parse("100")`|
|`TryParse()`|Safe parsing|`int.TryParse("100", out x)`|
|Boxing / Unboxing|Value ↔ Object|`(int)obj`|

---

# Statement

## 🔹 **Types of C# Statements**

Here’s a categorized list of the most common **C# statement types**:

---

### 🔸 1. **Declaration Statements**

Used to declare variables or constants.

```csharp
int x = 5;
const double PI = 3.14;
string name = "Alice";
```

---

### 🔸 2. **Expression Statements**

Usually perform actions like assignments, method calls, or object creation.

```csharp
x = x + 1;
Console.WriteLine("Hello");
new MyClass();
```

---

### 🔸 3. **Selection Statements** (Conditional)

| Statement | Purpose                       | Example                      |
| --------- | ----------------------------- | ---------------------------- |
| `if`      | Execute if condition is true  | `if (x > 0) { ... }`         |
| `else`    | Execute if condition is false | `else { ... }`               |
| `else if` | Chain multiple conditions     | `else if (x < 10) { ... }`   |
| `switch`  | Select between multiple cases | `switch (x) { case 1: ... }` |

---

### 🔸 4. **Iteration Statements** (Loops)

| Statement  | Description                                 | Example                        |
| ---------- | ------------------------------------------- | ------------------------------ |
| `for`      | Loop with initializer, condition, increment | `for (int i = 0; i < 10; i++)` |
| `while`    | Loop while condition is true                | `while (x < 10) { ... }`       |
| `do-while` | Always executes once, then checks           | `do { ... } while (x < 10);`   |
| `foreach`  | Loop through collections/arrays             | `foreach (var item in list)`   |

---

### 🔸 5. **Jump Statements**

| Statement  | Purpose                                        | Example       |
| ---------- | ---------------------------------------------- | ------------- |
| `break`    | Exit loop or switch                            | `break;`      |
| `continue` | Skip current iteration of loop                 | `continue;`   |
| `return`   | Exit a method and optionally return a value    | `return x;`   |
| `goto`     | Jump to a labeled statement (⚠️ use sparingly) | `goto label;` |

---

### 🔸 6. **Exception Handling Statements**

| Statement | Purpose                         | Example                   |
| --------- | ------------------------------- | ------------------------- |
| `try`     | Block to monitor for exceptions | `try { ... }`             |
| `catch`   | Handle the exception            | `catch (Exception e) { }` |
| `finally` | Always executes                 | `finally { ... }`         |
| `throw`   | Raise an exception              | `throw new Exception();`  |

---

### 🔸 7. **Lock, Using, and Yield Statements**

| Statement | Purpose                                          | Example                            |
| --------- | ------------------------------------------------ | ---------------------------------- |
| `lock`    | Prevents concurrent access to a block of code    | `lock(obj) { ... }`                |
| `using`   | Automatically disposes an object after use       | `using (var reader = ...) { ... }` |
| `yield`   | Used in iterators to return values one at a time | `yield return item;`               |

---

## 🔹 Example: Putting it Together

```csharp
using System;

class Program
{
    static void Main()
    {
        int sum = 0;

        for (int i = 1; i <= 5; i++)
        {
            sum += i;
        }

        Console.WriteLine("Sum is: " + sum);
    }
}
```

---

## 🔹 Summary Table

| Category           | Example Keyword(s)                    |
| ------------------ | ------------------------------------- |
| Declaration        | `int`, `string`, `var`, `const`       |
| Expression         | `=`, method calls                     |
| Selection          | `if`, `else`, `switch`                |
| Iteration          | `for`, `while`, `foreach`, `do-while` |
| Jump               | `break`, `continue`, `return`, `goto` |
| Exception Handling | `try`, `catch`, `finally`, `throw`    |
| Others             | `lock`, `using`, `yield`              |

---

# Complex data types

In **C#**, **complex data types** (also called **reference types**) are types that store **references** to their data, not the data itself. Unlike simple (or primitive) types like `int` or `bool`, complex types can contain **multiple values**, **methods**, or **structures**.

---

## 🔹 Categories of Complex Data Types in C#

|Category|Examples|
|---|---|
|Classes|`class`, e.g., `Person`, `Car`|
|Structs|`struct`, e.g., `Point`, `Vector`|
|Arrays|`int[]`, `string[]`|
|Interfaces|`interface`, e.g., `IDisposable`|
|Delegates|`delegate`, `Func<>`, `Action<>`|
|Enums|`enum`, e.g., `DayOfWeek`|
|Collections|`List<T>`, `Dictionary<K,V>`|
|Records|`record`, introduced in C# 9|
|Objects|Base type of all reference types|

---

## 🔸 1. **Class**

A **class** is a blueprint for creating objects.

```csharp
class Person
{
    public string Name;
    public int Age;

    public void Greet()
    {
        Console.WriteLine($"Hi, I'm {Name}!");
    }
}
```

Usage:

```csharp
Person p = new Person();
p.Name = "Alice";
p.Greet();
```

---

## 🔸 2. **Struct**

A **struct** is a value type but still considered complex because it can contain multiple members.

```csharp
struct Point
{
    public int X;
    public int Y;
}
```

> Use structs for **small**, **immutable**, and **lightweight** data.

---

## 🔸 3. **Array**

An array holds multiple values of the **same type**.

```csharp
int[] numbers = new int[5] {1, 2, 3, 4, 5};
Console.WriteLine(numbers[2]); // Outputs 3
```

---

## 🔸 4. **Collections (Generic and Non-Generic)**

### ✅ Generic (type-safe, preferred):

- `List<T>`
    
- `Dictionary<TKey, TValue>`
    
- `Queue<T>`, `Stack<T>`
    

```csharp
List<string> names = new List<string> { "Alice", "Bob" };
Dictionary<int, string> users = new Dictionary<int, string>();
users[1] = "Admin";
```

### ⚠️ Non-Generic (older):

- `ArrayList`, `Hashtable` (less type-safe)
    

---

## 🔸 5. **Interface**

Defines a **contract** for what a class must implement.

```csharp
interface IAnimal
{
    void Speak();
}

class Dog : IAnimal
{
    public void Speak() => Console.WriteLine("Woof!");
}
```

---

## 🔸 6. **Delegate**

A **delegate** is a type that holds a reference to a method.

```csharp
delegate void MyDelegate(string message);

MyDelegate d = Console.WriteLine;
d("Hello from delegate!");
```

Or use built-in types:

```csharp
Action<string> sayHi = name => Console.WriteLine($"Hi, {name}");
sayHi("John");
```

---

## 🔸 7. **Record (C# 9+)**

An immutable **reference type** primarily for **data models**.

```csharp
public record Person(string Name, int Age);

var p1 = new Person("Alice", 30);
Console.WriteLine(p1.Name);
```

---

## 🔸 8. **Object**

The **base type** for all reference types.

```csharp
object obj = "Hello";
Console.WriteLine(obj.ToString());
```

---

## 🔸 9. **Enum**

Defines a named set of constants. Technically a value type, but often used in complex types.

```csharp
enum Day { Sunday, Monday, Tuesday }

Day today = Day.Monday;
```

---

## 🔹 Key Differences: Value vs. Reference Types

|Feature|Value Type|Reference Type (Complex)|
|---|---|---|
|Stored in|Stack|Heap|
|Stores|Actual data|Reference to data|
|Example types|`int`, `float`, `bool`, `struct`|`class`, `array`, `string`, `List<T>`|
|Copy behavior|Copies value|Copies reference|

---

## ✅ Example: Using Complex Data Types Together

```csharp
class Student
{
    public string Name;
    public int[] Scores;

    public void PrintScores()
    {
        foreach (var score in Scores)
        {
            Console.WriteLine(score);
        }
    }
}

Student s = new Student();
s.Name = "Emma";
s.Scores = new int[] { 90, 85, 78 };
s.PrintScores();
```

---

# Array

In **C#**, an **array** is a **collection of elements** of the **same type**, stored in **contiguous memory locations**. Arrays allow you to store multiple values in a single variable, instead of declaring separate variables for each value.

---

## 🔹 **Types of Arrays in C#**

|Type|Description|Example|
|---|---|---|
|**Single-dimensional**|A basic linear array (1D)|`int[] arr = new int[5];`|
|**Multi-dimensional**|Rectangular grid (2D or more)|`int[,] matrix = new int[2,3];`|
|**Jagged array**|Array of arrays (non-uniform sizes)|`int[][] jagged = new int[3][];`|

---

## 🔸 1. **Single-Dimensional Array**

```csharp
int[] numbers = new int[5]; // declares an array of 5 integers

// Assign values
numbers[0] = 10;
numbers[1] = 20;

// Initialize with values
int[] scores = new int[] { 90, 80, 70, 60, 50 };

// Access
Console.WriteLine(scores[1]);  // Outputs: 80
```

---

## 🔸 2. **Multi-Dimensional Array** (Rectangular)

```csharp
int[,] grid = new int[2, 3] {
    {1, 2, 3},
    {4, 5, 6}
};

Console.WriteLine(grid[1, 2]);  // Outputs: 6
```

---

## 🔸 3. **Jagged Array** (Array of Arrays)

```csharp
int[][] jagged = new int[3][];

jagged[0] = new int[] { 1, 2 };
jagged[1] = new int[] { 3, 4, 5 };
jagged[2] = new int[] { 6 };

// Access
Console.WriteLine(jagged[1][2]); // Outputs: 5
```

---

## 🔸 4. **Iterating Over Arrays**

### 🔹 `for` loop:

```csharp
for (int i = 0; i < scores.Length; i++)
{
    Console.WriteLine(scores[i]);
}
```

### 🔹 `foreach` loop:

```csharp
foreach (int score in scores)
{
    Console.WriteLine(score);
}
```

---

## 🔸 5. **Array Properties and Methods**

|Property/Method|Description|
|---|---|
|`Length`|Total number of elements|
|`Rank`|Number of dimensions|
|`GetLength(d)`|Size of a specific dimension|
|`Array.Sort(arr)`|Sorts the array|
|`Array.Reverse()`|Reverses the order|

```csharp
int[] nums = { 3, 1, 4 };
Array.Sort(nums);     // nums = {1, 3, 4}
Array.Reverse(nums);  // nums = {4, 3, 1}
```

---

## 🔸 6. **Default Values in Arrays**

|Type|Default value|
|---|---|
|`int`|`0`|
|`bool`|`false`|
|`string`|`null`|
|`object`|`null`|

---

## 🔸 7. **Array vs List**

|Feature|`Array`|`List<T>`|
|---|---|---|
|Size|Fixed|Dynamic|
|Type|Static|Generic|
|Flexibility|Less|More|
|Namespace|System|System.Collections.Generic|

---

## ✅ Example Program: Simple Array

```csharp
using System;

class Program
{
    static void Main()
    {
        string[] names = { "Alice", "Bob", "Charlie" };

        foreach (string name in names)
        {
            Console.WriteLine(name);
        }
    }
}
```

---

# Tuples

In **C#**, a **tuple** is a data structure that lets you group multiple values of **different types** into a **single object** — without creating a custom class or struct. Tuples are useful for returning **multiple values** from a method or for grouping temporary data together.

---

## 🔹 **Types of Tuples in C#**

C# supports two main kinds of tuples:

|Tuple Type|Introduced In|Namespace|Description|
|---|---|---|---|
|**Classic Tuple**|.NET Framework 4.0|`System`|Immutable, older syntax, no named fields|
|**ValueTuple**|C# 7.0+|`System.ValueTuple`|Lightweight, supports named elements, recommended|

---

## 🔸 1. **Creating Tuples (ValueTuple – C# 7.0+)**

### ✅ **With Type Inference:**

```csharp
var person = ("Alice", 25); // Tuple<string, int>
Console.WriteLine(person.Item1); // Alice
Console.WriteLine(person.Item2); // 25
```

---

### ✅ **With Named Elements:**

```csharp
var person = (Name: "Bob", Age: 30);
Console.WriteLine(person.Name); // Bob
Console.WriteLine(person.Age);  // 30
```

> 💡 Naming the elements makes code more readable.

---

## 🔸 2. **Returning Multiple Values from a Method**

Tuples are great for returning more than one value without needing a custom class.

```csharp
(string Name, int Age) GetPerson()
{
    return ("Charlie", 40);
}

// Usage
var person = GetPerson();
Console.WriteLine($"{person.Name} is {person.Age} years old.");
```

---

## 🔸 3. **Deconstructing Tuples**

You can **split** a tuple into individual variables.

```csharp
var (name, age) = GetPerson();
Console.WriteLine($"Name: {name}, Age: {age}");
```

---

## 🔸 4. **Classic Tuple (Before C# 7.0)**

```csharp
Tuple<string, int> person = Tuple.Create("Diana", 28);
Console.WriteLine(person.Item1); // Diana
Console.WriteLine(person.Item2); // 28
```

> ⚠️ Classic `Tuple<...>` types are **reference types** and less efficient. Prefer `ValueTuple` when possible.

---

## 🔸 5. **Tuples vs Custom Classes**

|Feature|Tuple|Class/Struct|
|---|---|---|
|Use Case|Temporary group of values|Complex or long-term structure|
|Naming|Optional (C# 7+)|Always defined|
|Mutability|Mutable (`ValueTuple`)|Depends on design|
|Inheritance|Not supported|Supported|

---

## 🔸 6. **Tuple Limitations**

- A `ValueTuple` can have **up to 8 items** — the 8th can be another tuple (nested).
    
- No **method overloading** based on tuple names.
    
- Naming is for developer convenience, not enforced by the type system.
    

---

## ✅ Full Example

```csharp
using System;

class Program
{
    static (string Name, double GPA) GetStudent()
    {
        return ("Alice", 3.9);
    }

    static void Main()
    {
        var (name, gpa) = GetStudent();
        Console.WriteLine($"{name} has a GPA of {gpa}");
    }
}
```

---

## 🔸 Summary: When to Use Tuples

Use **tuples** when:

- You need to return multiple values **temporarily**.
    
- You want to avoid creating a new class just for simple grouping.
    
- Performance is important (use `ValueTuple` for value-type behavior).
    

---

