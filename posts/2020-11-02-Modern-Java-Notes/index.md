# Modern Java Notes


* [Method reference and lambdas](#method-reference-and-lambdas)
* [Streams](#streams)
* [Default methods](#default-methods)
* [Optional](#optional)
* [Miscellaneous](#miscellaneous)
* [References](#references)

Notes for the modern Java (Java 8+.)

## Method reference and lambdas

1. Java 8+ treats functions and lambdas as first-class citizens, which means we can pass functions around using method reference. Note that lambdas can only capture `final` variables in the same scope.

    ```java
    inventory.sort(comparing(Apple::getWeight));
    // Or
    inventory.sort((Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight()));
    // And the types can be inferred
    inventory.sort((a1, a2) -> a1.getWeight().compareTo(a2.getWeight()));
    // Instead of
    Collections.sort(inventory, new Comparator<Apple>() {
        public int compare(Apple a1, Apple a2) {
            return a1.getWeight().compareTo(a2.getWeight());
        }
    });

    File[] hiddenFiles = new File(".").listFiles(File::isHidden);
    // Instead of
    File[] hiddenFiles = new File(".").listFiles(new FileFilter() {
        public boolean accept(File file) {
            return file.isHidden();
        }
    });

    filterApples(inventory, (Apple a) -> a.getWeight() > 150 );

    // We can also use a Predicate to achieve behavior parameterization.

    public interface Predicate<T> {
        boolean test(T t);
    }
    public static <T> List<T> filter(List<T> list, Predicate<T> p) {
        List<T> result = new ArrayList<>();
        for(T e: list) {
            if(p.test(e)) {
                parameter T result.add(e);
            }
        }
        return result;
    }

    filter(numbers, (Integer i) -> i % 2 == 0);

    Thread t = new Thread(() -> System.out.println("Hello world"));
    // Instead of
    Thread t = new Thread(new Runnable() {
        public void run() {
            System.out.println("Hello world");
        }
    });

    // Callable is like the upgraded Runnable. It sends the task to a tread pool and the result
    // is stored in a Future.
    ExecutorService executorService = Executors.newCachedThreadPool();
    Future<String> threadName = executorService.submit( () -> Thread.currentThread().getName());
    // Instead of
    Future<String> threadName = executorService.submit(new Callable<String>() {
        @Override
        public String call() throws Exception {
            return Thread.currentThread().getName();
        }
    });

    // We can also use .and() .or() to create more complicated lambdas.
    Predicate<Apple> redAndHeavyAppleOrGreen =
        redApple.and(apple -> apple.getWeight() > 150).or(apple -> GREEN.equals(apple.getColor()));

    // Composing functions.
    f.andThen(g) // g(f(x))
    f.compose(g) // f(g(x))
    ```

## Streams

1. Streams let us manipulate collections in a declarative way. By using streams, we get parallel processing for free.

    ```java
    import static java.util.stream.Collectors.toList;
    // Sequential
    List<Apple> heavyApples = inventory.stream()
        .filter((Apple a) -> a.getWeight() > 150).collect(toList());
    // Parallel
    List<Apple> heavyApples = inventory.parallelStream()
        .filter((Apple a) -> a.getWeight() > 150).collect(toList());
    ```

2. Streams are like generators in Python. They are processed in-demand. Some stream functions:

    ```java
    List<String> lowCaloricDishesName =
        menu.parallelStream()
        .filter(d -> d.getCalories() < 400)
        .sorted(comparing(Dishes::getCalories))
        .map(Dish::getName)
        .distinct()
        .limit(3)
        .collect(toList()); // or .count() or .forEach()
    ```

3. Use `.flatMap()` to flatten each stream into a single stream.

    ```java
    List<String> uniqueCharacters =
        words.stream()
        .map(word -> word.split(""))
        .flatMap(Arrays::stream)
        .distinct()
        .collect(toList());
    ```

4. `.anyMatch()`, `.allMatch()`, and `.nonMatch()` return a bool.

## Default methods

1. Default methods for an interface allow concrete implementations not have to change.

    ```java
    // In List
    default void sort(Comparator<? super E> c) {
        Collections.sort(this, c);
    }
    // This made it possible to call apples.sort().
    ```

## Optional

1. `Optional<T>` is a better null type:

    ```java
    menu.stream()
        .filter(Dish::isVegetarian)
        .findAny() // or findFirst()
        .ifPresent(dish -> System.out.println(dish.getName()));

    isPresent() // returns a bool
    isPresent(Consumer<T> block) // executed only when the optional is not null
    get() // returns the value if present; otherwise it throws NoSuchElementException
    orElse(T other) // other is the default value if it's not present.
    ```

## Miscellaneous

1. Diamond operator `<>`:

    ```java
    List<String> listOfStrings = new ArrayList<>(); // The type here will be inferred.
    ```

## References

* [Modern Java in Action](https://www.goodreads.com/book/show/46213396-modern-java-in-action?from_search=true&from_srp=true&qid=Sqwlop5UTf&rank=1)

