These notes cover specific items from "Effective Java" related to object-oriented programming best practices in Java.

## Item 11: When you override `equals`, you must override `hashCode`

When you override the `equals` method, you _must_ also override the `hashCode` method. Failure to do so will violate the general contract for `Object.hashCode`, which can lead to unpredictable behavior in collections like `HashMap` and `HashSet`.

It is good practice to use tools like AutoValue to generate your `equals` and `hashCode` methods automatically.

```java
// Typical hashCode method
@Override public int hashCode() {
    int result = Short.hashCode(areaCode);
    result = 31 * result + Short.hashCode(prefix);
    result = 31 * result + Short.hashCode(lineNum);
    return result;
}
```

This method returns the result of a simple deterministic computation whose only inputs are the three significant fields in a `PhoneNumber` instance. It is clear that equal `PhoneNumber` instances have equal hash codes.

## Item 12: Always override `toString`

The default `toString` implementation in `Object` provides a generally unhelpful string representation. It consists of the class name followed by an “at” sign (`@`) and the unsigned hexadecimal representation of the hash code, for example, `PhoneNumber@163b91`.

Overriding `toString` provides a concise, informative, and human-readable representation of an object, which is invaluable for logging, debugging, and general understanding of objects.

## Item 13: Override `clone` judiciously

**In effect, the `clone` method functions as a constructor; you must ensure that it does no harm to the original object and that it properly establishes invariants on the clone.** In order for the `clone` method on `Stack` to work properly, it must copy the internals of the stack.

As a rule, copy functionality is best provided by **constructors** or **factories**. A notable exception to this rule is arrays, which are best copied with the `clone` method.

- **Realization**: Copy constructors are generally the preferred approach for object copying, despite the complexity of `clone`.

## Item 14: Consider implementing `Comparable`

Implementing the `Comparable` interface allows instances of your class to be sorted naturally.

When comparing field values in the implementations of `compareTo` methods, avoid the use of the `<` and `>` operators. Instead, use:

- The `static compare` methods in the boxed primitive classes (e.g., `Integer.compare(a, b)`).
- The comparator construction methods in the `Comparator` interface (e.g., `Comparator.comparing(ClassName::getField)`).

---

## Classes and Interfaces

### Item 15: Minimize the accessibility of classes and members

The rule of thumb is simple: **make each class or member as inaccessible as possible.** In other words, use the lowest possible access level consistent with the proper functioning of the software that you are writing.

If a package-private top-level class or interface is used by only one class, consider making the top-level class a `private static` nested class of the sole class that uses it (Item 24).

**Access Modifiers:**

- **`private`**: The member is accessible only from the top-level class where it is declared.
- **`package-private`**: The member is accessible from any class in the package where it is declared. Technically known as _default_ access, this is the access level you get if no access modifier is specified (except for interface members, which are `public` by default).
- **`protected`**: The member is accessible from subclasses of the class where it is declared (subject to a few restrictions [JLS, 6.6.2]) and from any class in the package where it is declared.
- **`public`**: The member is accessible from anywhere.

There is a key rule restricting your ability to reduce the accessibility of methods: If a method overrides a superclass method, it cannot have a **more restrictive access level** in the subclass than in the superclass [JLS, 8.4.8.3]. This is necessary to ensure that an instance of the subclass is usable anywhere that an instance of the superclass is usable (_Liskov substitution principle_, see Item 15). If you violate this rule, the compiler will generate an an error. A special case applies to interfaces: if a class implements an interface, all of the class methods that are in the interface must be declared `public` in the class.

Instance fields of public classes should **rarely be public** (Item 16).

- If an instance field is nonfinal or is a reference to a mutable object, making it public gives up the ability to limit the values that can be stored in the field. This means you give up the ability to enforce invariants involving the field.
- You also give up the ability to take any action when the field is modified, so **classes with public mutable fields are not generally thread-safe.**
- Even if a field is `final` and refers to an immutable object, making it public gives up the flexibility to switch to a new internal data representation in which the field does not exist.

Note that a nonzero-length array is always mutable, so **it is wrong for a class to have a public static final array field, or an accessor that returns such a field.** If a class has such a field or accessor, clients will be able to modify the contents of the array. This is a frequent source of security holes:

```java
// Potential security hole!
public static final Thing[] VALUES = { ... };
```

### To Research:

- What is the difference between a module, a package, a class, and a directory in Java?

### Item 16: In public classes, use accessor methods, not public fields

Avoid exposing mutable public fields in public classes.

```java
// Degenerate class: should not be public!
class Point {
    public double x;
    public double y;
}
```

Instead, use accessor methods (getters) and mutator methods (setters) to control access to fields and enforce encapsulation.

```java
// Encapsulation of data by accessor methods and mutators
class Point {
    private double x;
    private double y;

    public Point(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double getX() { return x; }
    public double getY() { return y; }
    public void setX(double x) { this.x = x; }
    public void setY(double y) { this.y = y; }
}
```

Certainly, the hard-liners are correct when it comes to public classes: **if a class is accessible outside its package, provide accessor methods** to preserve the flexibility to change the class’s internal representation.

However, **if a class is package-private or is a private nested class, there is nothing inherently wrong with exposing its data fields**.

### Item 17: Minimize mutability

An immutable class is simply a class whose instances cannot be modified after they are constructed. The canonical example is `String`.

To make a class immutable, follow these five rules:

1. **Don’t provide methods that modify the object’s state** (known as _mutators_, e.g., `set` methods).
2. **Ensure that the class can’t be extended.** This prevents careless or malicious subclasses from compromising the immutable behavior of the class. This is generally accomplished by making the class `final`.
3. **Make all fields `final`.** This clearly expresses your intent in a manner that is enforced by the system. Also, it is necessary to ensure correct behavior if a reference to a newly created instance is passed from one thread to another without synchronization, as spelled out in the _memory model_ [JLS, 17.5; Goetz06, 16].
4. **Make all fields `private`.** This prevents clients from obtaining access to mutable objects referred to by fields and modifying these objects directly. While it is technically permissible for immutable classes to have public final fields containing primitive values or references to immutable objects, it is not recommended because it precludes changing the internal representation in a later release (Items 15 and 16).
5. **Ensure exclusive access to any mutable components.** If your class has any fields that refer to mutable objects, ensure that clients of the class cannot obtain references to these objects. Never initialize such a field to a client-provided object reference or return the field from an accessor. Always make _defensive copies_ (Item 50) in constructors, accessors, and `readObject` methods (Item 88).

This pattern is known as the _functional_ approach because methods return the result of applying a function to their operand, without modifying it. Contrast it to the _procedural_ or _imperative_ approach in which methods apply a procedure to their operand, causing its state to change.

**Immutable objects are inherently thread-safe; they require no synchronization.** They cannot be corrupted by multiple threads accessing them concurrently. This is far and away the easiest approach to achieve thread safety. Since no thread can ever observe any effect of another thread on an immutable object, **immutable objects can be shared freely**.

### Action Item:

- Figure out how Sidekick is built and our MCP agent code.

The major disadvantage of immutable classes is that they require a **separate object for each distinct value.** Creating these objects can be costly, especially if they are large. For example, suppose that you have a million-bit `BigInteger` and you want to change its low-order bit:

```java
BigInteger moby = ...;
moby = moby.flipBit(0);
```

The `flipBit` method creates a new `BigInteger` instance, also a million bits long, that differs from the original in only one bit. The operation requires time and space proportional to the size of the `BigInteger`. Contrast this to `java.util.BitSet`. Like `BigInteger`, `BitSet` represents an arbitrarily long sequence of bits, but unlike `BigInteger`, `BitSet` is mutable. The `BitSet` class provides a method that allows you to change the state of a single bit of a million-bit instance in constant time:

```java
BitSet moby = ...;
moby.flip(0); // Modifies the existing instance
```

The package-private mutable companion class approach works fine if you can accurately predict which complex operations clients will want to perform on your immutable class. If not, then your best bet is to provide a _public_ mutable companion class. The main example of this approach in the Java platform libraries is the `String` class, whose mutable companion is `StringBuilder` (and its obsolete predecessor, `StringBuffer`).

**Classes should be immutable unless there’s a very good reason to make them mutable.** Immutable classes provide many advantages, and their only disadvantage is the potential for performance problems under certain circumstances. You should always make small value objects, such as `PhoneNumber` and `Complex`, immutable. (There are several classes in the Java platform libraries, such as `java.util.Date` and `java.awt.Point`, that should have been immutable but aren’t.) You should seriously consider making larger value objects, such as `String` and `BigInteger`, immutable as well. You should provide a public mutable companion class for your immutable class _only_ once you’ve confirmed that it’s necessary to achieve satisfactory performance (Item 67).

### Item 18: Favor composition over inheritance

It is safe to use inheritance within a package, where the subclass and the superclass implementations are under the control of the same programmers. It is also safe to use inheritance when extending classes specifically designed and documented for extension (Item 19). Inheriting from ordinary concrete classes across package boundaries, however, is dangerous.

If you are tempted to have a class _B_ extend a class _A_, ask yourself the question: Is every _B_ really an _A_? If you cannot truthfully answer yes to this question, _B_ should not extend _A_. If the answer is no, it is often the case that _B_ should contain a private instance of _A_ and expose a different API: _A_ is not an essential part of _B_, merely a detail of its implementation.

To summarize, inheritance is powerful, but it is problematic because it violates encapsulation.

### To Research:

- What is encapsulation?

### Item 19: Design and document for inheritance or else prohibit it