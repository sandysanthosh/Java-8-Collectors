# Java-8-Collectors




### Collectors

Java 8’s `Collectors` class provides several methods that make it easy to transform and aggregate data from streams. Here are some useful examples using different `Collectors` methods:

### 1. **Grouping and Counting Elements**

Suppose we have a list of words, and we want to count the frequency of each word.

```java
import java.util.*;
import java.util.stream.Collectors;

public class GroupAndCount {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "apple", "orange", "banana", "apple");

        Map<String, Long> wordCounts = words.stream()
                .collect(Collectors.groupingBy(word -> word, Collectors.counting()));

        System.out.println(wordCounts); // Output: {orange=1, banana=2, apple=3}
    }
}
```

### 2. **Joining Strings**

If you have a list of names and want to join them into a single string separated by commas:

```java
import java.util.*;
import java.util.stream.Collectors;

public class JoiningExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("John", "Jane", "Doe");

        String result = names.stream()
                .collect(Collectors.joining(", "));

        System.out.println(result); // Output: John, Jane, Doe
    }
}
```

### 3. **Grouping by Attribute and Collecting in Lists**

Imagine a list of employees, and we want to group them by their department.

```java
import java.util.*;
import java.util.stream.Collectors;

class Employee {
    String name;
    String department;

    Employee(String name, String department) {
        this.name = name;
        this.department = department;
    }

    @Override
    public String toString() {
        return name;
    }
}

public class GroupByExample {
    public static void main(String[] args) {
        List<Employee> employees = Arrays.asList(
                new Employee("Alice", "HR"),
                new Employee("Bob", "Engineering"),
                new Employee("Charlie", "Engineering"),
                new Employee("David", "HR")
        );

        Map<String, List<Employee>> employeesByDept = employees.stream()
                .collect(Collectors.groupingBy(emp -> emp.department));

        System.out.println(employeesByDept);
        // Output: {HR=[Alice, David], Engineering=[Bob, Charlie]}
    }
}
```

### 4. **Partitioning by a Condition**

Partition a list of integers into even and odd numbers.

```java
import java.util.*;
import java.util.stream.Collectors;

public class PartitioningExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        Map<Boolean, List<Integer>> partitioned = numbers.stream()
                .collect(Collectors.partitioningBy(num -> num % 2 == 0));

        System.out.println("Even numbers: " + partitioned.get(true));
        System.out.println("Odd numbers: " + partitioned.get(false));
        // Output: Even numbers: [2, 4, 6, 8, 10]
        //         Odd numbers: [1, 3, 5, 7, 9]
    }
}
```

### 5. **Mapping and Collecting to a Set**

Extract a list of names from a list of objects and store them in a `Set` to remove duplicates.

```java
import java.util.*;
import java.util.stream.Collectors;

class Person {
    String name;
    int age;

    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

public class MappingExample {
    public static void main(String[] args) {
        List<Person> people = Arrays.asList(
                new Person("Alice", 30),
                new Person("Bob", 25),
                new Person("Alice", 30),
                new Person("David", 28)
        );

        Set<String> uniqueNames = people.stream()
                .map(person -> person.name)
                .collect(Collectors.toSet());

        System.out.println(uniqueNames); // Output: [Alice, Bob, David]
    }
}
```

### 6. **Summing and Averaging with Collectors**

Calculate the total and average age from a list of people.

```java
import java.util.*;
import java.util.stream.Collectors;

class Person {
    String name;
    int age;

    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

public class SummingAndAveragingExample {
    public static void main(String[] args) {
        List<Person> people = Arrays.asList(
                new Person("Alice", 30),
                new Person("Bob", 25),
                new Person("Charlie", 32)
        );

        int totalAge = people.stream()
                .collect(Collectors.summingInt(person -> person.age));

        double averageAge = people.stream()
                .collect(Collectors.averagingInt(person -> person.age));

        System.out.println("Total age: " + totalAge);       // Output: Total age: 87
        System.out.println("Average age: " + averageAge);   // Output: Average age: 29.0
    }
}
```

### 7. **Collecting to an Unmodifiable List**

If you need an immutable list of transformed elements, `Collectors.toUnmodifiableList` can be used.

```java
import java.util.*;
import java.util.stream.Collectors;

public class UnmodifiableListExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("John", "Jane", "Doe");

        List<String> uppercaseNames = names.stream()
                .map(String::toUpperCase)
                .collect(Collectors.toUnmodifiableList());

        System.out.println(uppercaseNames); // Output: [JOHN, JANE, DOE]
    }
}
```

These examples demonstrate some of the powerful ways Java 8 `Collectors` can be used for various types of aggregations, transformations, and mappings in Java streams.


Certainly! Here are additional examples demonstrating the versatility of Java 8’s `Collectors`:

### 8. **Grouping by Multiple Levels (Nested Grouping)**

For complex grouping, such as grouping employees by department and then by age group.

```java
import java.util.*;
import java.util.stream.Collectors;

class Employee {
    String name;
    String department;
    int age;

    Employee(String name, String department, int age) {
        this.name = name;
        this.department = department;
        this.age = age;
    }
}

public class NestedGroupingExample {
    public static void main(String[] args) {
        List<Employee> employees = Arrays.asList(
                new Employee("Alice", "HR", 25),
                new Employee("Bob", "Engineering", 35),
                new Employee("Charlie", "Engineering", 28),
                new Employee("David", "HR", 45),
                new Employee("Eve", "Engineering", 50)
        );

        Map<String, Map<String, List<Employee>>> groupedByDeptAndAgeGroup = employees.stream()
                .collect(Collectors.groupingBy(emp -> emp.department,
                        Collectors.groupingBy(emp -> emp.age < 30 ? "Young" : "Experienced")));

        System.out.println(groupedByDeptAndAgeGroup);
        // Output example:
        // {HR={Young=[Alice], Experienced=[David]}, Engineering={Young=[Charlie], Experienced=[Bob, Eve]}}
    }
}
```

### 9. **Collecting to a Map with Custom Key and Value**

Create a map from a list of objects where keys and values are derived from object properties.

```java
import java.util.*;
import java.util.stream.Collectors;

class Student {
    int id;
    String name;

    Student(int id, String name) {
        this.id = id;
        this.name = name;
    }
}

public class CollectToMapExample {
    public static void main(String[] args) {
        List<Student> students = Arrays.asList(
                new Student(1, "Alice"),
                new Student(2, "Bob"),
                new Student(3, "Charlie")
        );

        Map<Integer, String> studentMap = students.stream()
                .collect(Collectors.toMap(student -> student.id, student -> student.name));

        System.out.println(studentMap); // Output: {1=Alice, 2=Bob, 3=Charlie}
    }
}
```

### 10. **Collecting to a Map with Handling Duplicate Keys**

Using `Collectors.toMap`, we can handle duplicate keys by merging values (e.g., summing or concatenating values).

```java
import java.util.*;
import java.util.stream.Collectors;

public class MergeDuplicateKeysExample {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "apple", "orange", "banana", "apple");

        Map<String, Integer> wordCountMap = words.stream()
                .collect(Collectors.toMap(
                        word -> word,
                        word -> 1,
                        Integer::sum // Merges by summing up counts
                ));

        System.out.println(wordCountMap); // Output: {orange=1, banana=2, apple=3}
    }
}
```

### 11. **Summing Elements with Summing Collector**

Suppose we have a list of products and we want to calculate the total price.

```java
import java.util.*;
import java.util.stream.Collectors;

class Product {
    String name;
    double price;

    Product(String name, double price) {
        this.name = name;
        this.price = price;
    }
}

public class SummingExample {
    public static void main(String[] args) {
        List<Product> products = Arrays.asList(
                new Product("Laptop", 999.99),
                new Product("Phone", 799.99),
                new Product("Tablet", 399.99)
        );

        double totalPrice = products.stream()
                .collect(Collectors.summingDouble(product -> product.price));

        System.out.println("Total Price: " + totalPrice); // Output: Total Price: 2199.97
    }
}
```

### 12. **Grouping and Reducing (Max, Min, Sum)**

Using `Collectors.groupingBy` and a reducing collector to find the highest-priced product in each category.

```java
import java.util.*;
import java.util.stream.Collectors;

class Item {
    String category;
    String name;
    double price;

    Item(String category, String name, double price) {
        this.category = category;
        this.name = name;
        this.price = price;
    }
}

public class GroupingAndReducingExample {
    public static void main(String[] args) {
        List<Item> items = Arrays.asList(
                new Item("Electronics", "Laptop", 1200),
                new Item("Electronics", "Phone", 800),
                new Item("Furniture", "Chair", 150),
                new Item("Furniture", "Table", 300)
        );

        Map<String, Optional<Item>> maxPriceByCategory = items.stream()
                .collect(Collectors.groupingBy(item -> item.category,
                        Collectors.maxBy(Comparator.comparing(item -> item.price))));

        System.out.println(maxPriceByCategory);
        // Output: {Electronics=Optional[Item@...], Furniture=Optional[Item@...]}
    }
}
```

### 13. **Partitioning by a Custom Condition with Statistics**

Partition a list of integers based on whether they’re above or below average, and collect statistics.

```java
import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class PartitioningWithStatisticsExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(5, 10, 15, 20, 25, 30, 35, 40);

        double average = numbers.stream()
                .collect(Collectors.averagingInt(num -> num));

        Map<Boolean, List<Integer>> partitionedByAverage = numbers.stream()
                .collect(Collectors.partitioningBy(num -> num > average));

        System.out.println("Above average: " + partitionedByAverage.get(true));
        System.out.println("Below or equal to average: " + partitionedByAverage.get(false));
        // Output example:
        // Above average: [25, 30, 35, 40]
        // Below or equal to average: [5, 10, 15, 20]
    }
}
```

### 14. **Grouping and Mapping to Custom Collection**

Collect a list of elements grouped by criteria into a `Set` instead of a `List`.

```java
import java.util.*;
import java.util.stream.Collectors;

class Book {
    String title;
    String genre;

    Book(String title, String genre) {
        this.title = title;
        this.genre = genre;
    }
}

public class GroupingToSetExample {
    public static void main(String[] args) {
        List<Book> books = Arrays.asList(
                new Book("Book A", "Fiction"),
                new Book("Book B", "Science"),
                new Book("Book C", "Fiction"),
                new Book("Book D", "Science")
        );

        Map<String, Set<String>> booksByGenre = books.stream()
                .collect(Collectors.groupingBy(book -> book.genre,
                        Collectors.mapping(book -> book.title, Collectors.toSet())));

        System.out.println(booksByGenre);
        // Output example:
        // {Fiction=[Book A, Book C], Science=[Book B, Book D]}
    }
}
```

### 15. **Summing and Averaging with Multiple Collectors in One Pass**

Calculate both the total and average salary of employees in a single pass.

```java
import java.util.*;
import java.util.stream.Collectors;

class Employee {
    String name;
    double salary;

    Employee(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }
}

public class SummingAndAveragingExample {
    public static void main(String[] args) {
        List<Employee> employees = Arrays.asList(
                new Employee("Alice", 70000),
                new Employee("Bob", 90000),
                new Employee("Charlie", 60000)
        );

        DoubleSummaryStatistics salaryStatistics = employees.stream()
                .collect(Collectors.summarizingDouble(emp -> emp.salary));

        System.out.println("Total Salary: " + salaryStatistics.getSum());
        System.out.println("Average Salary: " + salaryStatistics.getAverage());
        // Output:
        // Total Salary: 220000.0
        // Average Salary: 73333.33333333333
    }
}
```

These examples highlight different ways `Collectors` can be applied to aggregate, group, partition, and transform data in powerful ways, often simplifying otherwise complex operations.

