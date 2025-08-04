## 📘 What is Stream API?

The **Java Stream API** (introduced in Java 8) allows you to process collections of data in a **functional and declarative** way. It works like a pipeline where data flows through multiple operations such as `filter`, `map`, and `collect`.

## ✅ Why Use Stream API?

- 🚀 **Simplifies complex logic** using clean, readable code.
- 🔄 **Supports method chaining** for streamlined data operations.
- ⚡ **Enables parallel processing** for better performance.
- 🔒 Encourages **immutable and side-effect-free** operations.
- 🔍 Reduces boilerplate when working with collections (List, Set, etc.).

---

# Data Set
```java
import java.time.LocalDate;
import java.util.List;

public class CrewMember {
    private String name;
    private String ecn;
    private String emailId;
    private String base;
    private int age;
    private LocalDate dob;
    private double flyingHours;
    private String rank;
    private boolean isOnDuty;
    private List<String> certifications;
    private List<String> languages;
    private LocalDate lastFlightDate;

    public CrewMember(String name, String ecn, String emailId, String base, int age, LocalDate dob,
                      double flyingHours, String rank, boolean isOnDuty,
                      List<String> certifications, List<String> languages, LocalDate lastFlightDate) {
        this.name = name;
        this.ecn = ecn;
        this.emailId = emailId;
        this.base = base;
        this.age = age;
        this.dob = dob;
        this.flyingHours = flyingHours;
        this.rank = rank;
        this.isOnDuty = isOnDuty;
        this.certifications = certifications;
        this.languages = languages;
        this.lastFlightDate = lastFlightDate;
    }

    // Add Getters only (recommended for stream operations)
    public String getName() { return name; }
    public String getEcn() { return ecn; }
    public String getEmailId() { return emailId; }
    public String getBase() { return base; }
    public int getAge() { return age; }
    public LocalDate getDob() { return dob; }
    public double getFlyingHours() { return flyingHours; }
    public String getRank() { return rank; }
    public boolean isOnDuty() { return isOnDuty; }
    public List<String> getCertifications() { return certifications; }
    public List<String> getLanguages() { return languages; }
    public LocalDate getLastFlightDate() { return lastFlightDate; }

}

private List<CrewMember> getCrewList() {
    return List.of(
        new CrewMember("Arjun Verma", "ECN001", "arjun.verma@airline.com", "DEL", 34,
            LocalDate.of(1991, 5, 23), 1200.5, "Captain", true,
            List.of("Boeing 737", "Night Operations"), List.of("English", "Hindi"),
            LocalDate.of(2025, 7, 31)),

        new CrewMember("Sneha Roy", "ECN002", "sneha.roy@airline.com", "BLR", 28,
            LocalDate.of(1997, 2, 14), 800.0, "First Officer", false,
            List.of("Airbus A320", "Emergency Handling"), List.of("English", "Hindi", "Kannada"),
            LocalDate.of(2025, 7, 20)),

        new CrewMember("Ravi Sharma", "ECN003", "ravi.sharma@airline.com", "DEL", 45,
            LocalDate.of(1979, 9, 1), 2200.3, "Captain", true,
            List.of("Boeing 777", "Safety Protocols"), List.of("English", "Hindi"),
            LocalDate.of(2025, 8, 1)),

        new CrewMember("Meera Nair", "ECN004", "meera.nair@airline.com", "BOM", 30,
            LocalDate.of(1994, 8, 19), 1050.2, "Flight Attendant", true,
            List.of("Customer Service", "First Aid"), List.of("English", "Malayalam"),
            LocalDate.of(2025, 7, 28)),

        new CrewMember("Zoya Khan", "ECN005", "zoya.khan@airline.com", "HYD", 25,
            LocalDate.of(1999, 12, 3), 400.0, "Flight Attendant", false,
            List.of("Emergency Evacuation"), List.of("English", "Telugu", "Hindi"),
            LocalDate.of(2025, 6, 15)),

        new CrewMember("Akash Gupta", "ECN006", "akash.gupta@airline.com", "BLR", 38,
            LocalDate.of(1986, 3, 27), 1800.0, "First Officer", true,
            List.of("Airbus A320", "Night Operations"), List.of("English", "Hindi"),
            LocalDate.of(2025, 7, 30)),

        new CrewMember("Fatima Sheikh", "ECN007", "fatima.sheikh@airline.com", "DEL", 41,
            LocalDate.of(1983, 7, 7), 1950.6, "Captain", false,
            List.of("Boeing 737", "Emergency Response"), List.of("English", "Hindi", "Urdu"),
            LocalDate.of(2025, 7, 15))
    );
}
```

# Filter

1. filter() is an intermediate operation.
2. It allows you to select elements from a stream that match a given condition (predicate).
3. It returns a new stream containing only the elements that match the condition.

```java

// Get all Captains from the crew list
List<CrewMember> captains = crewList.stream()
    .filter(crew -> crew.getRole().equals("Captain"))
    .collect(Collectors.toList());

// Crew with more than 1500 flying hours
List<CrewMember> experiencedCrew = crewList.stream()
    .filter(crew -> crew.getFlyingHours() > 1500)
    .collect(Collectors.toList());

// Filter crew from Delhi (DEL) who are currently active
List<CrewMember> activeDelhiCrew = crewList.stream()
    .filter(crew -> crew.getBase().equals("DEL"))
    .filter(CrewMember::isOnDuty)   // this is double operator means method referencing short way of writing predecate we can also write this like crew-> crew.isOnDuty 
    .collect(Collectors.toList());


 🔍 Explanation:

  .filter(crew -> crew.getBase().equals("DEL")) 
  ✅ Lambda expression** is used here because we are **comparing** the result of `getBase()` with `"DEL"`.  
  ❌ This cannot be replaced with a method reference, since method references cannot include comparisons like .equals("DEL").

  .filter(CrewMember::isOnDuty) 
  ✅ This is a method reference, a cleaner version of:  
  `.filter(crew -> crew.isOnDuty())`  
  Use method reference when you're just calling a method with no arguments or additional logic.

💡 Rule of Thumb:
> Use method reference when you're calling a method directly.  
> Use lambda expressions when you need to evaluate or compare the method’s result.
✅ Rule:
The condition inside filter() must always evaluate to a true or false (i.e., a boolean), either via lambda or method reference.
```
# Map

1. The word **map** means “to transform” or “to convert.”  
2. `map()` produces a new stream after applying a function to each element of the original stream.
3. The new stream could be of a different type.
4. It is commonly used to extract, transform, or modify data in a stream pipeline.
5. filter() is used to select elements based on a condition, while map() is used to transform each element into a new form.

```java
    //  Get All Crew Names
List<String> crewNames = crewList.stream()
    .map(CrewMember::getName)  // converts CrewMember to String (name)
    .collect(Collectors.toList());

// Extract Ages
List<Integer> ages = crewList.stream()
    .map(CrewMember::getAge)
    .collect(Collectors.toList());

// Extract emails domains
List<String> domains = crewList.stream()
    .map(CrewMember::getEmailId)
    .map(email -> email.substring(email.indexOf('@') + 1))
    .collect(Collectors.toList());


Here's a more advanced and real-world-style map() example using your CrewMember dataset.  
We'll convert each crew member into a custom DTO (Data Transfer Object) that contains only selected and transformed fields, such as:  
- name in uppercase  
- experience level (based on flying hours)  
- languages as a comma-separated string  
- and days since last duty

public class CrewSummaryDTO {
    private String name;
    private String experienceLevel;
    private String languagesSpoken;
    private long daysSinceLastDuty;

    public CrewSummaryDTO(String name, String experienceLevel, String languagesSpoken, long daysSinceLastDuty) {
        this.name = name;
        this.experienceLevel = experienceLevel;
        this.languagesSpoken = languagesSpoken;
        this.daysSinceLastDuty = daysSinceLastDuty;
    }

    // Getters, toString(), etc. (optional)
}
// Map CrewMember to DTO
List<CrewSummaryDTO> summaries = crewList.stream()
    .map(crew -> {
        String name = crew.getName().toUpperCase();
        String experienceLevel = crew.getFlyingHours() >= 2000 ? "Veteran"
                                : crew.getFlyingHours() >= 1000 ? "Experienced"
                                : "Junior";
        String languages = String.join(", ", crew.getLanguages());
        long daysSinceLastDuty = ChronoUnit.DAYS.between(crew.getLastDutyDate(), LocalDate.now());
        return new CrewSummaryDTO(name, experienceLevel, languages, daysSinceLastDuty);
    })
    .collect(Collectors.toList());


```
# FlatMap
A stream can hold complex data structures like Stream<List<String>>. In cases like this, flatMap() helps us to flatten the data structure to simplify further operations.
```java
// using Map
     crewList.stream()
    .map(CrewMember::getLanguages)
    // Returns Stream<List<String>>

// using flatMap
crewList.stream()
    .flatMap(crew -> crew.getLanguages().stream())
    // Returns Stream<String> → all languages flattened

List<String> allLanguages = crewList.stream()
    .flatMap(crew -> crew.getLanguages().stream())
    .distinct()
    .collect(Collectors.toList());

```
# Distinct
Returns distinct elements in the stream, eliminating duplicates. It uses the equals() method of the elements to decide whether two elements are equal or not.
```java
     String uniqueLanguagesOfOnDutyCrew = crewList.stream()
    .filter(CrewMember::isOnDuty)                       // Only crew members currently on duty
    .map(CrewMember::getLanguages)                      // Get list of languages each crew member knows
    .flatMap(List::stream)                              // Flatten into a single Stream<String>
    .distinct()                                         // Remove duplicates
    .sorted()                                           // Optional: Sort alphabetically
    .collect(Collectors.joining(", "));                 // Join all languages with comma

System.out.println("uniqueLanguagesOfOnDutyCrew = " + uniqueLanguagesOfOnDutyCrew);
```
# Sorted
1. The ‘sorted’ method is used to sort the stream.
2. Syntax (Basic Form) -------- stream().sorted()
3. Syntax (With Comparator) ----------- .stream().sorted(Comparator.comparing(...))
```java

// Sort CrewMember by Base
List<CrewMember> sortedByBaseThenName = crewList.stream()
    .sorted(Comparator.comparing(CrewMember::getBase)
                      .thenComparing(CrewMember::getName))
    .collect(Collectors.toList());

// Sort by descending flyingHours
List<CrewMember> sortByFlyingHoursDesc = crewList.stream()
    .sorted(Comparator.comparingDouble(CrewMember::getFlyingHours).reversed())
    .collect(Collectors.toList());


```



# Peek
1. It performs the specified operation on each element of the stream and returns a new stream which can be used further.
2. The peek() method is an intermediate operation that allows you to inspect or perform an action on each element of the stream without modifying it.

📌 Useful for debugging, logging, or temporarily monitoring the stream pipeline.
```java

List<CrewMember> delhiCrew = crewList.stream()
    .filter(crew -> crew.getBase().equals("DEL"))
    .peek(crew -> System.out.println("Passing: " + crew.getName() + " from " + crew.getBase()))
    .collect(Collectors.toList());


long onDutyCount = crewList.stream()
    .peek(crew -> System.out.println("Checking duty: " + crew.getName()))
    .filter(CrewMember::isOnDuty)
    .peek(crew -> System.out.println("On duty: " + crew.getName()))
    .count();


List<String> emails = crewList.stream()
    .peek(crew -> System.out.println("Original: " + crew.getName()))
    .filter(CrewMember::isOnDuty)
    .peek(crew -> System.out.println("Active: " + crew.getName()))
    .map(CrewMember::getEmailId)
    .peek(email -> System.out.println("Email Collected: " + email))
    .collect(Collectors.toList());       

```
# Limit
The ‘limit’ method is used to reduce the size of the stream.   limit(n)
```java

//Get first three onDuty crew member
       List<CrewMember> firstThreeOnDuty = crewList.stream()
    .filter(CrewMember::isOnDuty)
    .limit(3)
    .collect(Collectors.toList());

```
# Skip
```java
        List<Footballer>  sortByGenderAndNameSkipping5= footballerList.stream()
                .sorted(Comparator.comparing(Footballer::getGender).thenComparing(Footballer::getName))
                .skip(5)
                .collect(Collectors.toList());
                //prints (prettified)
                //sortByGenderAndNameSkipping5 = [
                //Footballer{name='Alexia', age=25, gender=FEMALE, positions=[CAM, RF, LF]}, 
                //Footballer{name='Jana', age=17, gender=FEMALE, positions=[CB]}, 
                //Footballer{name='Jennifer', age=29, gender=FEMALE, positions=[CF, CAM]}]
                //Note : See the above mentioned Sorted example for comparison


```
# Take While
```
        //Normal Filter
        List<Integer> filteredList = Stream.of(2, 4, 6, 8, 9, 10, 11, 12)
                .filter(n -> n % 2 == 0)
                .collect(Collectors.toList());
        System.out.println("filteredList = " + filteredList);
        //prints filteredList = [2, 4, 6, 8, 10, 12]

        //Take, While ...
        List<Integer> takeAWhile = Stream.of(2, 4, 6, 8, 9, 10, 11, 12)
                .takeWhile(n -> n % 2 == 0)
                .collect(Collectors.toList());

        System.out.println("takeAWhile = " + takeAWhile);
        //prints takeAWhile = [2, 4, 6, 8]

```
# Drop While
```
        List<Integer> dropWhile = Stream.of(2, 4, 6, 8, 9, 10, 11, 12)
                .dropWhile(n -> n % 2 == 0)
                .collect(Collectors.toList());

        System.out.println("dropWhile = " + dropWhile);
        //prints dropWhile = [9, 10, 11, 12]
```
# Count
The count() method is a terminal operation that returns the number of elements in the stream.
```java

//  Count total on-duty crew based in DEL
    long delhiOnDutyCount = crewList.stream()
    .filter(crew -> crew.getBase().equals("DEL"))
    .filter(CrewMember::isOnDuty)
    .count();
```
# For Each
It loops over the stream elements, calling the supplied function on each element.
```java
        List.of(4,1,6,7,19,2,3,81,64).stream()
                .parallel()
                .filter(number -> number<65)
                .forEach(number -> System.out.println("number = " + number));
                //prints
                //forEach
                //number = 2
                //number = 19
                //number = 3
                //number = 4
                //number = 6
                //number = 7
                //number = 1
                //number = 64
```
# For Each Ordered
```java
List.of(4,1,6,7,19,2,3,81,64).stream()
                .parallel()
                .filter(number -> number<65)
                .forEachOrdered(number -> System.out.println("number = " + number));
                //prints
                //forEach
                //number = 4
                //number = 1
                //number = 6
                //number = 7
                //number = 19
                //number = 2
                //number = 3
                //number = 64
```
# To Array
If we need to get an array out of the stream, we can simply use toArray().
```java
    // Get an array of crew members who are on-duty
CrewMember[] onDutyCrewArray = crewList.stream()
        .filter(CrewMember::isOnDuty)
        .toArray(CrewMember[]::new);

System.out.println("onDutyCrewArray = " + Arrays.asList(onDutyCrewArray));
// Example output: [CrewMember{name='Ayesha'...}, CrewMember{name='Rahul'...}]

```
# Min
Returns the minimum element in the stream.
```java
   // Finding the minimum age among all crew members
Integer minAge = crewList.stream()
        .min(Comparator.comparing(CrewMember::getAge))
        .map(CrewMember::getAge)
        .get();
```
# Max
Returns the maximum element in the stream.
```java
  Integer maxAge = crewList.stream()
        .max(Comparator.comparing(CrewMember::getAge))
        .map(CrewMember::getAge)
        .get();
```
# Any Match
anyMatch() checks if the predicate is true for any one element in the stream.
```java
        boolean anyMatch = footballerList
                .stream()
                .anyMatch(footballer -> footballer.getAge() > 25);
        System.out.println("anyMatch = " + anyMatch);
        
        //prints anyMatch = true
```
# All Match
allMatch() checks if the predicate is true for all the elements in the stream. 
```java
        boolean allMatch = footballerList.stream()
                .allMatch(footballer -> footballer.getAge() > 25);
        System.out.println("allMatch = " + allMatch);
        
        //prints allMatch = false
```
# None Match
noneMatch() checks if there are no elements matching the predicate. 
```java
        boolean noneMatch = footballerList.stream()
                .noneMatch(footballer -> footballer.getAge() > 100);
        System.out.println("noneMatch = " + noneMatch);
        //prints noneMatch = true
```
# Find First
findFirst() returns an Optional for the first entry in the stream; the Optional can, of course, be empty.
```java
        Integer findFirst = List.of(4, 1, 3, 7, 5, 6, 2, 28, 15, 29)
                .parallelStream()
                .filter(number -> number > 5)
                .findFirst()
                .get();

        System.out.println("findFirst = " + findFirst);
        //prints findFirst = 7
```
# Find Any
```java
        Integer findAny = List.of(4, 1, 3, 7, 5, 6, 2, 28, 15, 29)
                .parallelStream()
                .filter(number -> number > 5)
                .findAny()
                .get();

        System.out.println("findAny = " + findAny);
        //prints findAny = 28
```
# Reduce
```java
        Optional<String> longestName = footballerList.stream()
                .map(Footballer::getName)
                .reduce((name1, name2)
                        -> name1.length() > name2.length()
                        ? name1 : name2);

        longestName.ifPresent(System.out::println);
        //prints Ter Stegen
```
# Collect
```java
        List<Footballer> collect = footballerList.stream()
                .filter(footballer -> footballer.getGender().equals(Gender.FEMALE))
                .filter(footballer -> footballer.getAge() > 23)
                .collect(Collectors.toList());
                
                //List collect contains -
                //{name='Jennifer', age=29, gender=FEMALE, positions=[CF, CAM]} &
                //{name='Alexia', age=25, gender=FEMALE, positions=[CAM, RF, LF]}
```

# Collectors.groupingBy()
1. Collectors.groupingBy() is a collector used with .collect() to group stream elements based on a classifier function.
2. The result is a Map<K, List<T>>, where:
K is the key returned by the classifier function.
List<T> contains the elements that share that key.
```java

//  Group CrewMembers by Base
Map<String, List<CrewMember>> crewByBase = crewList.stream()
    .collect(Collectors.groupingBy(CrewMember::getBase));

// Group by On-Duty Status
Map<Boolean, List<CrewMember>> crewByDutyStatus = crewList.stream()
    .collect(Collectors.groupingBy(CrewMember::isOnDuty));

//  Group by Experience Level (Custom Classifier)
private static String getExperienceLevel(int flyingHours) {
    if (flyingHours > 5000) return "Veteran";
    else if (flyingHours > 2000) return "Experienced";
    else return "Novice";
}

Map<String, List<CrewMember>> crewByExperience = crewList.stream()
    .collect(Collectors.groupingBy(crew -> getExperienceLevel(crew.getFlyingHours())));

In Collectors.groupingBy(), you can define both the key and the value transformation, giving you full control over how to group and what to collect.

// Group and map values (transform collected values)
Map<String, List<String>> baseToNames = crewList.stream()
    .collect(Collectors.groupingBy(
        CrewMember::getBase, // Key: base
        Collectors.mapping(CrewMember::getName, Collectors.toList()) // Value: List of names
    ));

//  Group and count
Map<String, Long> baseToCount = crewList.stream()
    .collect(Collectors.groupingBy(
        CrewMember::getBase,
        Collectors.counting()
    ));

//  What is Multi-Level Grouping?
It means grouping by more than one level — like grouping crew by base, and then within each base, grouping by whether they are on duty.

Map<String, Map<Boolean, List<CrewMember>>> crewGroupedByBaseAndDuty = crewList.stream()
    .collect(Collectors.groupingBy(
        CrewMember::getBase,                              // 1st level grouping (Base)
        Collectors.groupingBy(CrewMember::isOnDuty)       // 2nd level grouping (OnDuty status)
    ));


// More Advanced Example (Grouping + Mapping)
// Group by base → then get a list of crew names who are on duty:

Map<String, List<String>> dutyCrewNamesByBase = crewList.stream()
    .filter(CrewMember::isOnDuty)
    .collect(Collectors.groupingBy(
        CrewMember::getBase,
        Collectors.mapping(CrewMember::getName, Collectors.toList())
    ));

//  Even Deeper: Group by base, then age category

Map<String, Map<String, List<CrewMember>>> crewByBaseAndAgeGroup = crewList.stream()
    .collect(Collectors.groupingBy(
        CrewMember::getBase,
        Collectors.groupingBy(crew -> {
            int age = crew.getAge();
            if (age < 25) return "Young";
            else if (age <= 35) return "Mid-age";
            else return "Senior";
        })
    ));


```

## Reference
1. [Stackify](https://stackify.com/streams-guide-java-8/)
2. [Advanced Java Programming](https://www.rokomari.com/book/179965/advanced-java-programing)
