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
filter() is an intermediate operation.
It allows you to select elements from a stream that match a given condition (predicate).
It returns a new stream containing only the elements that match the condition.
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
  ❌ This cannot be replaced with a method reference, since method references **cannot include comparisons** like `.equals("DEL")`.

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
map() produces a new stream after applying a function to each element of the original stream. The new stream could be of different type.
```java
        long femalesMoreThan24y= footballerList.stream()
                .filter(footballer -> footballer.getGender().equals(Gender.FEMALE))
                .map(Footballer::getAge)
                .filter(age -> age > 24)
                .count();

        System.out.println("femalesMoreThan24y = " + femalesMoreThan24y);
        //prints femalesMoreThan24y = 2
```
# FlatMap
A stream can hold complex data structures like Stream<List<String>>. In cases like this, flatMap() helps us to flatten the data structure to simplify further operations.
```java
        String allPositionsOfMaleLessThan30y = footballerList.stream()
                .filter(footballer -> footballer.getGender().equals(Gender.MALE))
                .filter(footballer -> footballer.getAge() < 30)
                .map(Footballer::getPositions)
                .flatMap(Collection::stream)
                .collect(Collectors.joining(","));

        System.out.println("allPositionsOfMaleLessThan30y = " + allPositionsOfMaleLessThan30y);
        //prints allPositionsOfMaleLessThan30y = CF,CAM,LF,CM,CAM,GK,CM,CDM
```
# Distinct
Returns distinct elements in the stream, eliminating duplicates. It uses the equals() method of the elements to decide whether two elements are equal or not.
```java
        String allUniquePositionsOfMaleLessThan30y = footballerList.stream()
                .filter(footballer -> footballer.getGender().equals(Gender.MALE))
                .filter(footballer -> footballer.getAge() < 30)
                .map(Footballer::getPositions)
                .flatMap(Collection::stream)
                .distinct()
                .collect(Collectors.joining(","));

        System.out.println("allUniquePositionsOfMaleLessThan30y = " + allUniquePositionsOfMaleLessThan30y);
        //prints allUniquePositionsOfMaleLessThan30y = CF,CAM,LF,CM,GK,CDM
```
# Sorted
The ‘sorted’ method is used to sort the stream.
```java
        List<Footballer>  sortByGenderAndName= footballerList.stream()
                .sorted(Comparator.comparing(Footballer::getGender).thenComparing(Footballer::getName))
                .collect(Collectors.toList());

        System.out.println("sortByGenderAndName = " + sortByGenderAndName);
        
        //prints (prettified) sortByGenderAndName = [
//Footballer{name='Arthur', age=23, gender=MALE, positions=[CM, CAM]}, 
//Footballer{name='Griezmann', age=28, gender=MALE, positions=[CF, CAM, LF]}, 
//Footballer{name='Messi', age=32, gender=MALE, positions=[CF, CAM, RF]}, 
//Footballer{name='Puig', age=20, gender=MALE, positions=[CM, CDM]}, 
//Footballer{name='Ter Stegen', age=27, gender=MALE, positions=[GK]},
//Footballer{name='Alexia', age=25, gender=FEMALE, positions=[CAM, RF, LF]}, 
//Footballer{name='Jana', age=17, gender=FEMALE, positions=[CB]}, 
//Footballer{name='Jennifer', age=29, gender=FEMALE, positions=[CF, CAM]}, 
//]
```
# Peek
It performs the specified operation on each element of the stream and returns a new stream which can be used further.
```java        
        long malePlayerCount = footballerList.stream()
                .filter(footballer -> footballer.getGender().equals(Gender.MALE))
                .sorted(Comparator.comparing(Footballer::getAge))
                .peek(footballer -> {
                    System.out.println("footballer = " + footballer);
                })
                .count();

        System.out.println("malePlayerCount = " + malePlayerCount);
//        prints
//        footballer = Footballer{name='Puig', age=20, gender=MALE, positions=[CM, CDM]}
//        footballer = Footballer{name='Arthur', age=23, gender=MALE, positions=[CM, CAM]}
//        footballer = Footballer{name='Ter Stegen', age=27, gender=MALE, positions=[GK]}
//        footballer = Footballer{name='Griezmann', age=28, gender=MALE, positions=[CF, CAM, LF]}
//        footballer = Footballer{name='Messi', age=32, gender=MALE, positions=[CF, CAM, RF]}
//        malePlayerCount = 5
```
# Limit
The ‘limit’ method is used to reduce the size of the stream.
```java
        List<Footballer> twoMaleFootballers = footballerList.stream()
                .filter(footballer -> footballer.getGender().equals(Gender.MALE))
                .limit(2)
                .collect(Collectors.toList());
                //prints 
                //twoMaleFootballers = [Footballer{name='Messi', age=32, gender=MALE, positions=[CF, CAM, RF]}, Footballer{name='Griezmann', age=28, gender=MALE, positions=[CF, CAM, LF]}]

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
```java
        long femalesMoreThan24y= footballerList.stream()
                .filter(footballer -> footballer.getGender().equals(Gender.FEMALE))
                .map(Footballer::getAge)
                .filter(age -> age > 24)
                .count();

        System.out.println("femalesMoreThan24y = " + femalesMoreThan24y);
        //prints femalesMoreThan24y = 2
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
        Footballer[] femaleFootballers = footballerList.stream()
                .filter(footballer -> footballer.getGender().equals(Gender.FEMALE))
                .toArray(Footballer[]::new);

        System.out.println("femaleFootballers = " + Arrays.asList(femaleFootballers));
        //prints To Array femaleFootballers = [Footballer{name='Jennifer', age=29, gender=FEMALE, positions=[CF, CAM]}, Footballer{name='Jana', age=17, gender=FEMALE, positions=[CB]}, Footballer{name='Alexia', age=25, gender=FEMALE, positions=[CAM, RF, LF]}]

```
# Min
Returns the minimum element in the stream.
```java
        Integer minAge = footballerList.stream()
                .min(Comparator.comparing(Footballer::getAge))
                .map(Footballer::getAge)
                .get();

        System.out.println("min age = " + minAge);
        //prints min age = 17
```
# Max
Returns the maximum element in the stream.
```java
        Integer maxAge = footballerList.stream()
                .max(Comparator.comparing(Footballer::getAge))
                .map(Footballer::getAge)
                .get();

        System.out.println("max age = " + maxAge);
        //prints max age = 32
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
## Reference
1. [Stackify](https://stackify.com/streams-guide-java-8/)
2. [Advanced Java Programming](https://www.rokomari.com/book/179965/advanced-java-programing)
