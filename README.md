# ✅ Java Comparable vs Comparator 

---

## 🔷 1. Comparable Interface

### ▶️ When to Use
Use `Comparable<T>` when a class **has a natural order** (e.g. `String`, `Integer`, `Date`), and that order is consistent across the application.

---

### 📌 Method Signature

```java
int compareTo(T other);
```

### Example (Comparable)
```java
import java.util.*;

class Product implements Comparable<Product> {
    int id;
    String name;

    Product(int id, String name) {
        this.id = id;
        this.name = name;
    }

    @Override
    public int compareTo(Product other) {
        return this.id - other.id; // Natural order by ID
    }

    @Override
    public String toString() {
        return "(" + id + ", " + name + ")";
    }
    
    

    public static void main(String[] args) {
        List<Product> products = new ArrayList<>();
        products.add(new Product(5, "Apple"));
        products.add(new Product(2, "Banana"));
        products.add(new Product(8, "Orange"));

        Collections.sort(products); // uses compareTo of the Product class

        System.out.println(products);
        //[(2, Banana), (5, Apple), (8, Orange)]

    }
}

```

#### 📌 Explanation
- compareTo() returns a negative number if this < other, positive if this > other, and 0 if equal.
- Collections.sort() uses compareTo() internally.

## 🔷 1. Comparator Interface

### ▶️ When to Use
Use `Comparator<T>` when:

- You don't want the class to implement Comparable
- You don’t want to (or can’t) modify the class.
- You need multiple different orderings.

### Some important Methods

1) `int compare(T o1, T o2)`

##### Example
```java
import java.util.*;

class Product {
    int id;
    String name;

    Product(int id, String name) {
        this.id = id;
        this.name = name;
    }

    @Override
    public String toString() {
        return "(" + id + ", " + name + ")";
    }

    public static void main(String[] args) {
        List<Product> products = List.of(
            new Product(3, "Banana"),
            new Product(1, "Apple"),
            new Product(2, "Orange")
        );

        List<Product> sorted = new ArrayList<>(products);
        sorted.sort((p1, p2) -> p1.name.compareTo(p2.name));

        System.out.println(sorted);
        //[(1, Apple), (3, Banana), (2, Orange)]
    }
}

```

2)  `Comparator.comparing()`
```java
//Signature
static <T, U extends Comparable<? super U>> Comparator<T> comparing(Function<T,U> keyExtractor);
```

#### Example
```java
import java.util.*;

class Product {
    int id;
    String name;

    Product(int id, String name) {
        this.id = id;
        this.name = name;
    }

    @Override
    public String toString() {
        return "(" + id + ", " + name + ")";
    }

    public static void main(String[] args) {
        List<Product> products = new ArrayList<>(List.of(
            new Product(2, "Banana"),
            new Product(1, "Apple"),
            new Product(3, "Orange")
        ));

        products.sort(Comparator.comparing(p -> p.name));

        System.out.println(products);
        //[(1, Apple), (2, Banana), (3, Orange)]

    }
}

```

3) `thenComparing()`
   The thenComparing() method in Java is used to chain multiple comparators together — that is, to provide a secondary sorting condition if two elements are equal under the first comparator.
```java
//Signature
default Comparator<T> thenComparing(Comparator<? super T> other);
```

#### Example
```java
List<Product> products = Arrays.asList(
            new Product("Apple", 1.20),
            new Product("Banana", 0.80),
            new Product("Apple", 1.00),
            new Product("Orange", 0.90)
        );

        products.sort(
            Comparator.comparing((Product p) -> p.name)
                      .thenComparing(p -> p.price)
        );

        products.forEach(System.out::println);
        //Apple - $1.00, Apple - $1.20, Banana - $0.8, Orange - $0.9

```

###### Explanation
- First, the list is sorted by name (Apple, Banana, Orange).
- For entries with the same name (two Apples), thenComparing() kicks in and sorts them by price.

4) `reversed()`
```java
//Signature
default Comparator<T> reversed()
```

##### Example
```java
products.sort(Comparator.comparing((Product p) -> p.name).reversed());
//[(3, Orange), (2, Banana), (1, Apple)]

```

###### Explanation
- Sort the products by name, then reverse the sorted list

4) `nullsFirst() / nullsLast()`
```java
//Signature
static <T> Comparator<T> nullsFirst(Comparator<? super T> comparator)
static <T> Comparator<T> nullsLast(Comparator<? super T> comparator)

```

##### Example
```java
List<String> items = Arrays.asList("Zebra", null, "Apple");

items.sort(Comparator.nullsFirst(Comparator.naturalOrder()));
System.out.println(items);
//[null, Apple, Zebra]


```

## Note
We also have:
- ComparingInt
- ComparingLong
- ComparingDouble
- and many others


# ✅ Final Tips
- Use Comparable for one consistent, natural order.

- Use Comparator for custom or multiple sorting needs.

- Prefer Comparator.comparing() with lambdas/method refs instead of `int compare(T o1, T o2)`.

- Chain sort logic with thenComparing().
