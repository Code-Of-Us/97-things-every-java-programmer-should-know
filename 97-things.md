# 97 things every Java developer should know

Summary of every item from book **97 things every Java developer should know** by Kevlin Henney and Trisha Gee.

## 1. All You Need Is Java (_Anders Nors_)

Three kinds of progammers:

- **Mort** - the opportunistic developer, doing quick fixes and making things up as he went along
- **Elvis** - the pragmatic programmer, building solutions for the ages while learning on the job
- **Einstein** - the paranoid programmer, obsessed with designing the most efficient solution and figuring everything
  out before writing his code.

_Libraries grew into frameworks with prescripted architectures. And as these frameworks became technology ecosystems,
many of us forgot about the little language that could—Java._

Need to work wit files? - _java.nio_\
Databases? - _java.sql_\
HTTP server? - _com.sun.net.httpserver_

TLDR; Use Java as much as you can, don't just throw in dependencies.

## 2. Approval Testing (_Emily Bache_)

Did you ever try something like this:  `assertEquals("",functionCall())`

Where `functionCall` is returning a string and you’re not sure exactly what that string should be, but you’ll know it’s
right when you see it?

That can be a problem when expected strings change -> you can use _approval testing tools_ that will store strings in
file.
[ApprovalTests](https://approvaltests.com/)
[TextTests](http://texttest.sourceforge.net/)
More examples: [Link](https://www.softwaretestingmagazine.com/knowledge/approval-testing/)

Good situations for approval testing:

- _Code without unit tests that you need to change_ - turns into a problem of finding seams and carving out pieces of
  logic that return something interesting you can approve.
- _REST APIs and functions that return JSON or XML_ - nice when response is a bigger string (because you store it
  outside source code)
- _Business logic that builds a complex return object_ - Think of a Receipt or a Prescription or an Order. Good for
  non-tech persons.

If you already have tests that make assertions about strings that are longer than one line, then it is recommended
finding out more about approval testing.

## 3. Augment Javadoc with AsciiDoc (_James Elliott_)

Sometimes Javadoc is not enough , because you need more than API documentation, though—much more than you can fit in the
package and project overview pages Javadoc offers.

[AsciiDoc](https://asciidoctor.org/docs/what-is-asciidoc) is an alternative to Markdown.\
[Quick refference](https://docs.asciidoctor.org/asciidoc/latest/syntax-quick-reference/)\
There is [plugin](https://plugins.jetbrains.com/plugin/7391-asciidoc) for IntelliJ (*_.adoc_)\
Chrome [extension](https://chromewebstore.google.com/detail/asciidoctorjs-live-previe/iaalpfgpbocpdfblpnhhgllgbdbchmia)

## 4. Be Aware of Your Container Surroundings (_David Delabassee_)

[JVM ergonomics](https://docs.oracle.com/en/java/javase/11/gctuning/ergonomics.html#GUID-DA88B6A6-AF89-4423-95A6-BBCBD9FAE781)
enables the JVM to tune itself by looking at two key environmental metrics:

- number of CPUs
- available memory.

With these metrics, the JVM determines important parameters such as:

- which garbage collector to use
- how to configure it
- the heap size
- the size of the ForkJoinPool, and so on

Linux Docker container support allows the JVM to rely on Linux cgroups to get the metrics of resources allocated to the
**container** it runs in.
Any JVM **older than JDK 8 update 191** is not aware that it is running within a container and **will access metrics
from the host OS** and not from the container itself.

The following command shows which JVM parameters are configured by the JVM
ergonomics:  `java -XX:+PrintFlagsFinal -version | grep ergonomic`

JVM container support is **enabled by default** but can be disabled by using the `-XX:-UseContainerSupport` - this way
you can observe and explore the impact of JVM ergonomics with and without container support.

## 5. Behavior Is “Easy”;State Is Hard (_Edson Yanaga_)

**inheritance** - You have class _Shape_ and other classes _Circle_, _Triangle_ that inherit properties of _Shape_.\
**encapsulation** - state cannot be changed directly, so properties are enclosed in methods  (_getters_ and _setters_)\
**polymorphism** - you can access objects of different types through the same interface. Two types:

- <ins>static (compile time)</ins> - method _overloading_, operator _overloading_ (C++, Kotlin)
- <ins>dynamic (runtime)</ins> - method _overriding_

Focus here is **encapsulation**. We have state and expose API for getting and changing that state (_getters_ and
_setters_).
If our code doesn't work, bugs are 'easy' to find, but if our code seems to work but we still have bugs -> problems!\
One of the biggest problem is with **inconsistent state**\
How to solve it? **Immutability** is the one of the possible answers
> If we can guarantee that our objects are immutable, we’ll never have an inconsistent state.

Therefore, don’t generate your setters automatically. Take time to think about them.  
Do you really need that setter in your code? And if you decide that you do, perhaps because of some framework
requirement, consider using an **anti-corruption layer** to protect and validate your internal state after those setter
interactions.

## 6. Benchmarking Is Hard— JMH Helps (_Michael Hunger_)

**benchmarks** = Java programs intended to measure the performance of one or more elements of a system where the Java
program is being executed
**Micro-benchmarking** = the process of measuring the performance of a small piece of code

Programs:

- **JMH** may be used to perform micro benchmarking on code units
- Jmeter can be used to perform performance testing on applications

**JMH (Java Microbenchmarking Harness)**

- Developed for OpenJDK
- Consists of:
    - A small library
    - A build system plug-in
- Library features:
    - Provides annotations and utilities for creating benchmarks as annotated Java classes and methods
    - Includes a BlackHole class to consume generated values, preventing code elimination
    - Ensures correct state handling in multithreaded environments
- Build system plug-in capabilities:
    - Generates a JAR with the necessary infrastructure for running and measuring tests accurately
    - Incorporates dedicated warm-up phases
    - Supports proper multithreading
    - Manages multiple forks and averages results across them

## 7.The Benefits of Codifying and Asserting Architectural Quality (_Daniel Bryant_)

- checking and publishing quality metrics within the build pipeline can prevent the gradual decay of architectural
  quality

> Once upon a time, an architect drew a series of nice architectural diagrams that illustrated the components of the
> system and how they should interact. Then the project got bigger and use cases more complex, new developers dropped in
> and old developers dropped out. This eventually led to new features being added in any way that fit. Before long,
> everything depended on everything, and any change could have an unforeseeable effect on any other component.

**ArchUnit - An Open Source Library**

- Extensible library for checking the architecture of Java code using Java unit-test frameworks like JUnit or TestNG
- Checks for cyclic dependencies
- Checks dependencies between packages, classes, layers, and slices
- Analyzes Java bytecode and imports all classes for analysis

## 8.Break Problems and Tasks into Small Chunks (_Jeanne Boyarsky_)

- **Break Problems into Small Chunks**
    - Simplifies managing large tasks
    - Makes each piece easier to handle and test
- **Write Automated Tests for Small Problems**
    - Ensures each chunk works correctly
    - Helps identify issues early
- **Commit Frequently**
    - Provides rollback points
    - Prevents loss of significant progress if issues arise
- Even "special" tasks could be divided into smaller tasks

## 9.Build Diverse Teams (_Ixchel Ruiz_)

- **Importance in Software Development**
    - Cooperation is essential in modern software teams.
    - Teams need to function like pit crews, with each member contributing.

- **Diversity's Role in Innovation**

    - Four types of diversity: industry background, country of origin, career path, and gender.
    - Diverse teams bring varied perspectives, leading to innovation.
    - High gender diversity in management teams can increase innovation revenue by 8%.
- **Benefits of Diverse Teams**

    - Increases pool of information, skills, and networks.
    - Constructive debate in a positive environment leads to creative solutions.
- **Challenges of Diversity**

    - Potential for conflict and factionalism.
    - Communication issues and digital mishaps can create "us versus them" mentality.
- **Building Effective Teams**

    - Develop psychological safety and trust.
    - Trust encourages risk-taking and cooperation.
    - Speaking up is easier, leading to participation and novel ideas.
- **Personality in Teams**

    - Recognize and value different personalities and approaches.
    - Encourage a balance between diverse traits for better team dynamics.

## 10. Builds Don’t Have To Be Slow and Unreliable (_Jenn Strater_)

- Codebase and development team growing daily
- Build times increased from 8 minutes to over 15 minutes
- Initially manageable, later became frustrating
- **Impact of Long Build Times**

    - Increased context switching
    - Reliance on CI server for test validation
    - Delayed debugging process
- **Problems with Flaky Tests**

    - Naive solution: Ignoring failing tests (@Ignore)
    - Pushed issues down the line to CI step
    - Blocked entire team when tests failed post-merge
- **Efforts to Fix Tests**

    - Long feedback cycles (15+ minutes)
    - Wasted days tracking down bugs due to lack of relevant data
- **Developer Productivity Engineering (DPE)**

    - Practice of improving developer experience through data
    - Focus on measuring build performance and tracking regressions
    - Analysis of results to find bottlenecks
- **Benefits of DPE**

    - Sharing detailed reports (e.g., Gradle build scans) with teammates
    - Comparing failing and passing builds to identify issues
    - Optimizing the build process to reduce frustration
- **Outcome**

    - Teams focusing on DPE are happier and more productive
    - Prevents build performance issues and improves overall developer experience

## 11. "But it works on my local machine"

- The process of setting up the infrastructure to build source code on a developer's machine can be challenging
- Questions may arise regarding the required JDK version and distribution, the operating system compatibility, the IDE
  and its version, and the version of the build tool needed
- Every project should have a clearly defined set of tools that are compatible with the technical requirements to
  compile, test, execute, and package the code
- A better solution is the use of a wrapper, which helps with provisioning a standardized version of the build tool
  runtime without manual intervention.
- In the Java space, examples of wrappers include the <b>Gradle Wrapper</b> and the <b>Maven Wrapper</b>
- The Maven Wrapper requires the Maven runtime to be installed on the machine to generate the Wrapper files.
- These Wrapper files represent the scripts, configuration, and instructions every developer of the project uses to
  build the project with a predefined version of the Maven runtime.
- The wrapper concept improves build reproducibility and maintainability, eliminating the "But it works on my machine!"
  issue.

## 12. The Case Against Fat JARS

- The use of fat JARs for deploying Java applications has become popular with the rise of microservice architecture
  style, DevOps, and cloud-native technologies.
- Fat JARs can have disadvantages such as large size, long build process, lack of shared dependencies, and challenges
  with integration across services.
- Some organizations are now creating "skinny JARs" to address these challenges
- The HubSpot engineering team created a new Maven plug-in called SlimFast to separate the application code from the
  associated dependencies, building and uploading two separate artifacts.
- The SlimFast plug-in adds a Class-Path manifest entry to the skinny JAR that points to the dependencies’ JAR file, and
  generates a JSON file with information about all the dependency artifacts.
- At deploy time, the build downloads all of the application’s dependencies, but then caches these artifacts on each of
  the application servers.
- The result is that at build time, only the application’s skinny JAR is uploaded to the remote storage, and at deploy
  time, only this same thin JAR needs to be downloaded to the target deployment environment.

## 13. The code restorer

- Current software practices often suffer from haste, leading to fragile systems that can easily collapse under small
  disturbances
- Many teams and companies focus only on short-term goals, often neglecting long-term stability
- This approach can lead to a cycle of developers, managers, and entrepreneurs exiting a project or company before its
  potential issues become apparent
- The software industry can be compared to furniture, where some pieces are built to last hundreds of years, while
  others are expected to crumble within a decade

- There is a middle ground in the form of the code restorer, a role focused on reshaping existing codebases to make them
  manageable again
- The code restorer's job is not to recreate the same thing but better, but to improve the existing codebase by adding
  tests, breaking down complex classes, removing unused functionality, ...
- <i>focusing on profit and building something that holds for a while</i> <b>VS</b> <i>focusing on durability and
  carefully reshaping the code</i>

## 14. Concurrency on the JVM

- Raw threads were the original concurrency model in Java, but they have limitations.
- Threads and locks are low-level and hard to use correctly, leading to issues with shared mutable state and reduced
  parallelism
- Java doesn't support distributed memory, making it hard to scale multithreaded programs across multiple machines
- Coordinating threads via distributed queues instead of locks can overcome shared memory limitations
- Akka brings the actor model to the JVM, implementing concurrency through message flow between actors

  https://doc.akka.io/docs/akka/current/typed/guide/actors-intro.html?language=scala
- Clojure offers software transactional memory, turning the JVM heap into a transactional data set
- Java 8 lambdas promote functional programming properties like immutability and referential transparency
- There's no one-size-fits-all solution for concurrency. Different options have different trade-offs

## 15. CountDownLatch - Friend or Foe?

```java
ExecutorService pool=Executors.newFixedThreadPool(8);
        Future<?> future=pool.submit(()->{
        // Your task here
        });
```

```java
int tasks=16;
        CountDownLatch latch=new CountDownLatch(tasks);
        for(int i=0;i<tasks; i++){
        Future<?> future=pool.submit(()->{
        try{
        // Your task here
        }
        finally{
        latch.countDown();
        }
        });
        }
        if(!latch.await(2,TimeUnit.SECONDS)){
        // Handle timeout
        }
```

- CountDownLatch is useful in many different situations. It becomes especially useful when you’re testing your
  concurrent code, since it allows you to make sure that all the tasks are complete before checking their results
- CountDownLatch is very useful, but there’s one important catch: you shouldn’t use it in production code that makes use
  of concurrent libraries or frame‐ works, such as Kotlin’s coroutines or Spring WebFlux

# 16. Declarative Expression Is the Path to Parallelism

Russel Winder

- Java evolved from an imperative, object-based language to one favoring declarative expression.
- Imperative code tells the computer what to do; declarative code expresses goals, abstracting implementation details.
- Java 8 introduced higher-order functions, central to declarative expression.
- Imperative example for squaring a list:
  ```java
  List<Integer> squareImperative(final List<Integer> datum) {
      var result = new ArrayList<Integer>();
      for (var i = 0; i < datum.size(); i++) {
          result.add(i, datum.get(i) * datum.get(i));
      }
      return result;
  }
  ```
- Declarative example using streams:
  ```java
  List<Integer> squareDeclarative(final List<Integer> datum) {
      return datum.stream()
                  .map(i -> i * i)
                  .collect(Collectors.toList());
  }
  ```
- Declarative code is easier to maintain and allows easy parallelism:
  ```java
  List<Integer> squareDeclarative(final List<Integer> datum) {
      return datum.parallelStream()
                  .map(i -> i * i)
                  .collect(Collectors.toList());
  }
  ```
- Streams provide the right abstraction for data parallelism, making parallel computations straightforward.

# 17 Deliver Better Software, Faster

Burk Hufnagel

- **Deliver Better Software, Faster** is a guiding principle for ensuring user satisfaction and career fulfillment.

- **Deliver** means:
    - Taking responsibility beyond writing and debugging code.
    - Understanding the process of getting code into production.
    - Avoiding actions that hinder the process (e.g., unclear requirements).
    - Doing actions that speed up the process (e.g., automated tests).

- **Better Software** involves:
    - Building the right thing: meeting requirements and acceptance criteria.
    - Building the thing right: writing easily understandable code.
    - Balancing between quick feature delivery and avoiding technical debt.

- **Faster** entails:
    - Using processes like TDD for automated tests.
    - Regularly running unit, integration, and user acceptance tests.
    - Automating test runs and deployment processes.
    - Deploying changes more frequently to reduce the risk of issues and deliver benefits sooner.

- Adopting this principle is challenging but rewarding, improving both user experience and professional satisfaction.

# 18 Do You Know What Time It Is?

Christin Gorman

- Time in programming involves two aspects: seconds passing and the interaction of astronomy and politics.
- Time representation issues arise from mixing these concepts.

- **Key Concepts:**
    - **LocalDateTime:** Represents a date and time (e.g., 1:15 p.m., October 13, 2019) without timezone information.
    - **Instant:** A specific point on the timeline, the same globally (e.g., in Boston and Beijing).
    - **ZonedDateTime:** A LocalDateTime combined with a TimeZone, including UTC offsets and daylight saving time (DST)
      rules.

- **Pitfalls and Solutions:**
    - **Future Events:** Use ZonedDateTime instead of Instant to account for potential changes in DST rules or UTC
      offsets.
    - **Recurring Events:** Use ZonedDateTime for departure and Duration for flight duration to handle DST changes.
    - **Ambiguous Times:** Handle times like 2:30 a.m. during DST transitions using methods
      like `ZonedDateTime.withEarlierOffsetAtOverlap()` and `ZonedDateTime.withLaterOffsetAtOverlap()` in Java, or
      explicit DST resolvers in Noda Time.

- Good tools like java.time or Noda Time can prevent many common errors related to date and time handling.

# 19 Don’t hIDE Your Tools

Gail Ollis

- **Essential Tool:** The most crucial tool for Java programmers is `javac`, not an IDE.
- **Historical Context:** Programming without IDEs was common in the past, but essential tools were still indispensable.

- **Case Study:**
    - C++ programmers quickly adapted to a version control change using command-line tools.
    - Java programmers struggled with IDE configuration, losing productivity.

- **Issues with Relying Solely on IDEs:**
    - IDEs can obscure understanding of essential tools and processes.
    - Programmers might lose touch with the details of what their tools do.

- **Advantages of Knowing Essential Tools:**
    - **Problem Resolution:** Easier to troubleshoot and resolve issues like "it works on my machine."
    - **Flexibility:** Simple to set different options using commands like `javac --help`.
    - **Cross-Environment Assistance:** Easier to help others and troubleshoot when IDE tools fail.
    - **Tool Integration:** Ability to use a wider range of tools, including scripts and Linux commands.
    - **Real-World Testing:** Ensures code runs as it will for end users, outside of an IDE environment.

- **Conclusion:**
    - Understanding and using essential tools directly enhances mastery and troubleshooting capabilities.
    - While IDEs offer significant benefits, balancing their use with direct tool interaction is crucial for true skill
      development.

# 20 Don’t Vary Your Variables

Steve Freeman

- **Immutability:** Using `final` for variables simplifies reasoning about code and reduces errors.

- **Assign Once:**
    - Common mutable pattern:
      ```java
      Thing thing;
      if (nextToken == MakeIt) {
          thing = makeTheThing();
      } else {
          thing = new SpecialThing(dependencies);
      }
      thing.doSomethingUseful();
      ```
    - Improved with conditional expression:
      ```java
      final var thing = nextToken == MakeIt
          ? makeTheThing()
          : new SpecialThing(dependencies);
      thing.doSomething();
      ```
    - Further improved by extracting to a function:
      ```java
      final var thing = aThingFor(nextToken);
      thing.doSomethingUseful();
  
      private Thing aThingFor(Token aToken) {
          return aToken == MakeIt
              ? makeTheThing()
              : new SpecialThing(dependencies);
      }
      ```
    - Simplifies condition handling with a `switch` statement:
      ```java
      private Thing aThingFor(Token aToken) {
          switch (aToken) {
              case MakeIt:
                  return makeTheThing();
              case Special:
                  return new SpecialThing(dependencies);
              case Green:
                  return mostRecentGreenThing();
              default:
                  return Thing.DEFAULT;
          }
      }
      ```

- **Localize Scope:**
    - Avoid spreading variable assignments:
      ```java
      var thing = Thing.DEFAULT;
      // lots of code to figure out nextToken
      if (nextToken == MakeIt) {
          thing = makeTheThing();
      }
      thing.doSomethingUseful();
      ```
    - Extract to a supporting method:
      ```java
      final var thing = theNextThingFrom(aStream);
  
      private Thing theNextThingFrom(Stream aStream) {
          // lots of code to figure out nextToken
          if (nextToken == MakeIt) {
              return makeTheThing();
          }
          return Thing.DEFAULT;
      }
      ```
    - Separate concerns further:
      ```java
      final var thing = aThingForToken(nextTokenFrom(aStream));
      ```

- **Streaming Approach:** Use streams for concise and clear code:
  ```java
  final var thing = nextTokenFrom(aStream)
      .filter(t -> t == MakeIt)
      .findFirst()
      .map(t -> makeTheThing())
      .orElse(Thing.DEFAULT);
  ```

- **Benefits of Immutability:** Encourages careful design, exposes potential bugs, and ensures clear change points in
  code.

## 21. Embrace SQL Thinking (_Dean Wampler_)

``` postgresql
SELECT c.id, c.name, c.address, o.items 
FROM customers c
JOIN orders o
ON o.customer_id = c.id
GROUP BY c.id
```

this is a few lines of code in SQL that would take a lot more code in Java to achieve the same result

- **We don’t need a new table for the join output, so we don’t create one.**\
  Unnecessary classes become a burden as the code
  evolves.


- **The query is declarative.**\
  Nowhere does it tell the database **_how to do_** the query\
  Java is an imperative language,
  so we tend to write code that says what to do


- **The domain-specific language (DSL) is well matched to the problem**\
  DSLs can be somewhat controversial. It’s very hard to design a good one,
  and the implementations can be messy. SQL is a data DSL. It’s quirky, but
  its longevity is proof of how well it expresses typical data-processing
  needs.

``` java
@Query(nativeQuery = true,
            countQuery = """
                    SELECT
                        COUNT(b.id)
                        FROM users u
                        JOIN booking b on u.id = b.student_id
                        WHERE u.id IN :childrenIds
                    """,
            value = """
                    SELECT
                        b.id as bookingId,
                        u.first_name as studentName,
                        s.abrv as subject,
                        l.abrv as level,
                        t.first_name as tutorFirstName,
                        t.last_name as tutorLastName,
                        t.email as tutorEmail,
                        t.phone_number as tutorPhone,
                        tut.slug as tutorSlug,
                        b.created_at as createdAt,
                        b.start_time as startTime,
                        b.accepted as accepted,
                        b.deleted as deleted,
                        b.in_reschedule as inReschedule,
                        b.cost as price
                        FROM users u
                        JOIN booking b on u.id = b.student_id
                        JOIN users t on t.id= b.tutor_id
                        JOIN subject s on b.subject_id = s.id
                        JOIN level l on b.level_id = l.id
                        JOIN tutor tut on t.id=tut.user_id
                        WHERE u.id IN :childrenIds
                        ORDER BY b.start_time DESC
                    """)
    Page<StudentBookingDetailsView> getUserBookingsDetails(List<UUID> childrenIds, Pageable pageable);
```

``` java
public interface StudentBookingDetailsView {
    UUID getBookingId();
    String getStudentName();
    String getSubject();
    String getLevel();
    String getTutorFirstName();
    String getTutorLastName();
    String getTutorSlug();
    String getTutorEmail();
    String getTutorPhone();
    LocalDateTime getStartTime();
    LocalDateTime getCreatedAt();
    float getPrice();
    boolean isAccepted();
    boolean isInReschedule();
    boolean isDeleted();
}
```

## 22. Events Between Java Components (_A.Mahdy AbdelAziz_)

One of the core concepts of object orientation in Java is that every class can
be considered to be a _component_.\
An **event** in Java is an action that **changes the state** of a component.

Assume we have an `Oven` component and a `Person` component. And we want oven to prepare a meal when person is hungry.\
There are two ways to implement this:

1. `Oven` checks `Person` in fixed, short intervals. This annoys `Person` and is
   also expensive for `Oven` if we want it to check on multiple instances of
   `Person`.
2. `Person` comes with a public event, `Hungry`, to which `Oven` is subscribed.
   Once `Hungry` is fired, `Oven` is notified and starts preparing food.

In short this is [Observer design pattern](https://refactoring.guru/design-patterns/observer)
> I changed code example and used 'Observer' nomenclature

```java

@FunctionalInterface
public interface HungerObserver {
    void hungry();
}
```

``` java
public class Person {
    private List<HungerObserver> hungerObservers = new ArrayList<>();
    // rest of Person class
    
    public void subscribe(HungerObserver observer) {
        hungerObservers.add(observer);
    }

    public void unsubscribe(HungerObserver observer){
        hungerObservers.remove(observer);
    }
    
    public void notifyAllHungerObservers(){   
        hungerObservers.stream().forEach(HungerObserver::hungry);
    }
}
```

``` java
public class Oven implements HungerObserver {
    // variables
    
    @Override    
    public void hungry(){
        // make some food   
    }
}
```

``` java
public class Main {
   Oven oven = new Oven();
   Person person = new Person();
   
   person.subscribe(oven);
   person.notifyAllHungerObservers();
}
```

## 23. Feedback Loops (_Liz Keogh_)

Feedback is important
Some of notes:

- Because I make mistakes while writing code, I work with an IDE. My
  IDE corrects me when I’m wrong.
- Because I make mistakes in understanding the existing code, I use a statically
  typed language. The compiler corrects me when I’m wrong.
- Because I make mistakes while thinking, I work with a pair. My pair corrects
  me when I’m wrong.
- Because my pair is human and also makes mistakes, we write unit tests.
  Our unit tests correct us when we’re wrong.
- Because we sometimes misunderstand our product manager and other
  stakeholders, we showcase the system. Our stakeholders will tell us if
  we’re wrong.
- ...

## 24. Firing on All Engines (_Michael Hunger_)

Traditional Java profilers use either byte code instrumentation or sampling -> hard to understand outputs

Brendan Gregg, a performance engineer at Netflix, came up
with **flame** graphs.

A flame graph sorts and aggregates the traces up to each stack level, so that
their count per level represents the percentage of the total time spent in that part of code.\
Note that the left-to-right order has no
significance; often, it’s just alphabetical sorting. The same is true for colors.\
Only the **relative widths and stack depths are relevant**.

Download here: [Project](https://github.com/async-profiler/async-profiler)\
`./profiler.sh -d <duration> -f flamegraph.svg -s -o svg <pid> && \
open flamegraph.svg -a "Google Chrome"`

## 25. Follow the Boring Standards (_Adam Bien_)

Frameworks evolve very quickly, and it has become hard to master multiple frameworks at the same time. \
Focusing on standards allows you to gain knowledge incrementally over time
—an efficient way to learn.  
Evaluating popular frameworks is exciting, but
the gained knowledge isn’t necessarily applicable to the next “hot thing.”

## 26. Frequent Releases Reduce Risk (_Chris O’Dell_)

**Infrequent Releases and Risk**
- Large releases increase failure likelihood due to many changes at once
- High-impact failures can cause outages or severe data loss
- Testing for all possible failures is impossible, only covers known scenarios.

**Testing Strategy**
- Automated tests should cover known failure scenarios
- Add new failures to test suite as they occur
- Keep regression tests light, fast, and repeatable

**Frequent Small Releases**
- Smaller releases reduce the likelihood of including failures
- Each release contains minimal changes, lowering the chance of causing issues.

**Balancing Testing and Risk**
- Thorough testing provides confidence but must be balanced with practicality
- Production is the only place where success counts
- Frequent, small releases lower the overall risk by minimizing the likelihood of failures.

## 27. From Puzzles to Products (_Jessica Kerr_)

**Early career focus** = centered on solving specific puzzles with clear requirements using limited tools \
- fun 

**Broadening Perspective** = Engaged with customers and participated in design negotiations, transitioning from solving problems to defining them \
- real-word contributions


## 28. “Full-Stack Developer” Is a Mindset (_Maciej Walkowiak_)

What developer needed to know in 2007?

- relational database
- FE developer was limited to HTML and CSS, spiced with a bit og JavaScript
- Java developer primarily working with Hibernate plus either Spring or Struts
- This set of technologies covered almost everything necessary for building applications at that time

Today

- we started building more and more complex user interfaces - advanced JavaScript frameworks
- NpSQL database
- stream data with Kafka, message with RabbitMQ, and a lot more
- In many cases, we also are responsible for setting up or maintaining the infrastructure with Terraform or
  CloudFormation
- use or even configure Kubernetes clusters
- complexity has grown - separate positions for frontend developer, backend developer, and DevOps engineer
- Is it still possible to be a full-stack developer?

> You can’t be an expert in everything. The good thing is that you don’t have to be one

The most beneficial team (especially in smaller companies) 
- each area of expertise is covered by at least one expert
- these experts don’t limit themselves to working only on that one area
- BE developers can write frontend code—even if the code isn’t perfect \
-> This helps move projects forward more quickly \
-> It also leads to greater engagement during refinement meetings (no tasks isolated only to a certain group of people) \
-> not being strictly limited to one area changes how you approach tasks
-> Having one person go on vacation is not an issue \

>Full-stack developer is therefore a mindset. It’s being senior and junior at the same time, with a can-do attitude.

## 29. Garbage Collection Is Your Friend (_Holly Cummins_)

Before Java GC - programmers needed to track all the memory they’d allocated manually, and deallocate it once nothing was using it anymore. \
->  manual deallocation is a frequent cause of memory leaks (if too late) and crashes (if too early)
**Benefits of Java GC:**
- Modern GC can be faster than manual allocation/deallocation (malloc/free).
- Handles both memory allocation and object arrangement.

**Memory Management:**
- Efficient memory management improves performance.
- Object locality: placing frequently used objects close in memory enhances cache performance.
- Fragmentation increases allocation time; compacting memory can improve overall performance.

- GC strategies vary by JVM implementation
- each JVM offers a range of configurable options
- JVM defaults are usually good start, but it is worth understanding some of the mechanics and variations possible.
  
**GC Strategies:**
- (_Stop-the-world collectors_): Pause all program activity for safe collection.
- (_Concurrent collectors_): Spread collection tasks across threads, avoiding global pauses but with slight inefficiency.
  - less efficient than first one
  - suitable for applications where pauses would be noticed (music playback or GUI)

**Collection Methods:**
- (_Mark-and-sweep_): Identifies and allocates free space in the heap.
- (_Copying collectors_): Divide heap into two areas, copying live objects to a reserve space when needed.
  - Good for short-lived objects (there is nothing to copy!)


## 30. Get Better at Naming Things (_Peter Hilton_)

Improves the maintainability of code 

- Avoid names that are:
  - meaningless (foo)
  - too abstract (data)
  - duplicated (data2) 
  - vague (DataManager)
  - abbreviated or short (dat)
  - single letters (d)

- Avoid overly long names (>4 words) 
- Replace plurals with collective nouns (appointment_list -> calendar) 
- Rename parts of entities with relationship names (company_person -> employee, owner, shareholder) 
- Don’t mix up class and object names (to avoid duplicate type noise): rename a date field called dateCreated to created, and a Boolean field called isValid to valid
- instead of a Customer called customer, use a more specific name, such as recipient when sending a notification or reviewer when posting a product review.

## 31. Hey Fred, Can You Pass Me the HashMap?

Here are the bullet points summarizing the content:

- The choice of vocabulary in code can greatly impact its readability, maintainability, and performance.
- Using domain-specific language and meaningful abstractions in your code can improve its quality.
- Technical classes rarely have a place in the vocabulary of the domains we're working in.
- The need for utility classes should be a red flag that you're missing an abstraction.
- Using a `CompositeKey` class instead of two separate `String` parameters can make the code more expressive and easier to understand, and also improve the performance of the code.
```java
listOfNames.get(firstName + lastName);
listOfNames.get(new CompositeKey(firstName, lastName))
```
- The author advocates for the use of domain-specific language and meaningful abstractions in your code.

## 32. How to Avoid Null
- Null is often referred to as the “billion-dollar mistake” due to the issues it can cause in code
- Avoid initializing variables to null. Instead, declare a variable when you know what value it should hold
- instead of doing this:
```java
  public String getEllipsifiedPageSummary(Path path) {
  String summary = null;
  Resource resource = this.resolver.resolve(path);
  if (resource.exists()) {
    ValueMap properties = resource.getProperties();
    summary = properties.get("summary");
  } else {
    summary = "";
  }
  return ellipsify(summary);
}
```
  Do the following:
```java
  public String getEllipsifiedPageSummary(Path path) {
    var summary = getPageSummary(path);
    return ellipsify(summary);
}

public String getPageSummary(Path path) {
    var resource = this.resolver.resolve(path);
    if (!resource.exists()) {
        return "";
    }
    var properties = resource.getProperties();
    return properties.get("summary");
}
```
- Avoid returning null. Use `Optional<T>` instead to make your code more explicit and easier to handle when no value is produced
- Avoid passing and receiving null parameters. If an operation can have an optional parameter, create two methods: one with the parameter and one without
```java
  public class ImageDrawer {
    public void drawImage(Image original, int xCoord, int yCoord, int imgWidth, int imgHeight, ImageObserver observer) {
        // Implementation with ImageObserver
    }

    public void drawImage(Image original, int xCoord, int yCoord, int imgWidth, int imgHeight) {
        drawImage(original, xCoord, yCoord, imgWidth, imgHeight, null);
    }
}
  
```
- Null is acceptable as an implementation detail of a class, i.e., the value of an attribute. The code that needs to be aware of the absence of value is contained to the same file, making it simpler to reason about and prevent null leakage
```java
public class UserProfile {
    private String firstName;
    private String lastName;
    private String middleName; // This can be null if the user has no middle name.

    public UserProfile(String firstName, String lastName, String middleName) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.middleName = middleName; // null is acceptable here.
    }

    public String getFullName() {
        if (middleName == null) {
            return firstName + " " + lastName;
        }
        return firstName + " " + middleName + " " + lastName;
    }
}
```
- By avoiding unnecessary use of null, you can prevent NullPointerExceptions and contribute to solving the "billion-dollar problem"

## 33. How to Crash Your JVM

As a Java developer, understanding the environment your software runs in is indeed crucial. It's not just about the libraries and APIs you use, but also about how your software interacts with the system it's running on. Here are some ways you could potentially crash your Java Virtual Machine (JVM):

1. **Exhaust Memory:** Allocate as much memory as you can. RAM is not endless—if no more RAM can be allocated, your allocation will fail, causing the JVM to crash.
```java
public class MemoryExhaustion {
    public static void main(String[] args) {
        try {
            long[] memoryFiller = new long[Integer.MAX_VALUE];
        } catch (OutOfMemoryError e) {
            System.out.println("Memory exhausted!");
        }
    }
}
 ```
2. **Exhaust Disk Space:** Try to write data to your hard disk until it is full. Disk space is not endless either, and if the JVM can't write necessary data to disk, it can crash.
```java
import java.io.FileOutputStream;
import java.io.IOException;

public class DiskSpaceExhaustion {
    public static void main(String[] args) {
        try (FileOutputStream fos = new FileOutputStream("largefile.txt")) {
            byte[] buffer = new byte[1024];
            while (true) {
                fos.write(buffer);
            }
        } catch (IOException e) {
            System.out.println("Disk space exhausted!");
        }
    }
}
```
3. **Exhaust File Descriptors:** Try to open as many files as you can. There's a limit to the number of file descriptors your system can handle. If you exceed this limit, the JVM can crash.

4. **Exhaust Thread Limit:** Try to create as many threads as you can. There's a limit to the number of threads your system can handle. If you exceed this limit, the JVM can crash.

5. **Modify Class Files:** Try to modify your own .class files in the filesystem while your application is running. This can cause the JVM to crash.

6. **Kill Process:** Try to find your own process ID, and then try to kill it by using `Runtime.exec()` (e.g., by calling `kill -9` on your process ID).

7. **System Exit:** Try to create a class at runtime that only calls `System.exit()`, load that class dynamically via the class loader, then call it.

8. **Exhaust Socket Connections:** Try to open as many socket connections as possible. On a Unix system, the maximum number of possible socket connections equals the maximum number of file descriptors.

9. **Unsafe Operations:** Try using `sun.misc.Unsafe`. This class provides low-level operations that can directly manipulate memory and can easily crash the JVM if used incorrectly.

10. **Native Code:** Write some native code using the Java Native Interface (JNI). Errors in native code can lead to JVM crashes.


## 34. Improving Repeatability and Auditability with Continuous Delivery
- <b>Continuous Delivery</b> (CD) is a method of automating the steps of delivering code to production
- CD aims to reduce the time and effort required to deploy code
- CD improves the repeatability of the deployment process by scripting each step for execution by a computer instead of a human
- This reduces the risk associated with deployments and encourages organizations to release more often and with smaller changesets
- CD enhances auditability by improving transparency in deployments
- The scripts used to execute steps and the values supplied to them can be stored in version control, allowing for easy review
- Automated deployments can generate reports that can help with auditing

## 35. In the Language Wars, Java Holds Its Own
- Every developer has a preferred programming language, often influenced by comfort level or professional requirements
- Java, created in 1995, was designed with a C-like syntax and the principle of <b>write once, run anywhere</b> (WORA)
- Java aimed to simplify complex programming required in C-family languages and achieve platform independence via the Java Virtual Machine (JVM)
- Common criticisms of Java include larger deployables and verbose syntax, both of which are attributed to its design goals
- Java's larger deployables are a result of the need to include all dependencies for deployment to ensure platform independence
- Java's verbose syntax is due to its aim to be more user-friendly by abstracting some low-level details
- Despite these criticisms, Java is appreciated for its explicitness, wide applicability, and versatility
- Java's explicitness provides clarity on what is being built and how it's being done
- Knowledge of Java is beneficial in both business and technical markets
- Java's versatility allows developers to explore technology in all stacks and areas
- The diverse market offers many programming language options to fit various business needs
- Each developer should choose the best language for their specific task
- Even if Java isn't a developer's primary language, it's considered a valuable skill due to its versatility and wide applicability

## 36.Inline Thinking (_Patricia Aas_)

Imagine that you need to iterate through a big array of relatively small
objects. In Java today, you don’t really have an
array of triangles; you have an array of pointers to triangle objects because
regular objects in Java are “reference types,” meaning you access them
through Java pointers/references \
So even though the array is probably a contiguous
section of memory, the triangle objects themselves can be anywhere
on the Java heap. Looping through this array will be **“cache-unfriendly”**

Imagine instead that the array contained the actual triangle objects, not
pointers to them. Now they are close in memory, and looping over them is
much more **“cache-friendly”**

Object types that can be stored directly into an array like
that are called **“value types”** or **“inline types.”**

A difference between inline types and reference types is
that you **don’t have to allocate inline types on the heap**.\
As a bonus, objects that are not allocated on the
Java heap **do not** need to be garbage collected.

These cache-friendly behaviors are already present in Java when using socalled
“primitive types,” like ints and chars\
We will soon have user-defined ones, probably called “inline classes.”

## 37. Interop with Kotlin (_Sebastiano Poggi_)

You can call Java code in Kotlin and it just works.\
If you don’t apply nullability annotations in Java, Kotlin
assumes all those types have unknown nullability—they’re so-called platform
types. If you’re certain they will never be null, you can coerce them into a
non-null type with the !! operator or by casting them to a non-null type.
In either case, you’ll get a crash if the value is null at runtime.  \
When invoking Kotlin code from Java, you should find that while the majority
of the code will work just fine, you may see quirks with some advanced
Kotlin language features that don’t have a direct equivalent in Java.

## 38. It’s Done, But… (_Jeanne Boyarsky_)

How many times have you been to a stand-up, daily Scrum or status meeting
and heard the phrase “It’s done, but...”?

### 1. Communication and Clarity

- Ideally your team has a definition of done. But even if they don’t, there is
probably some expectation of what done means.\
- Common things that aren’t done include writing tests, documentation, and
edge cases.\
- Don't use term **done done**
- Be a clear communicator. If something isn’t done, don’t
say it’s done.
- “I coded the happy path and next I will add validation”
- “I finished all the code—the only thing remaining is for me to update the user manual”
- “I thought I was done and then discovered the widget doesn’t work on Tuesdays.”

### 2. Perception

- As soon as managers hear done, that becomes the
perception. **The but either gets forgotten or becomes a small thing.**
- Now you are moving on to the next thing when you didn’t finish the first
  thing. --> **That’s where technical debt comes from!**

### 3. There’s No Partial Credit for Done

- Done is a binary state. It’s either done or it isn’t
- Remember: don’t say you are done until you are done!

## 39. Java Certifications: Touchstone in Technology (_Mala Gupta_)

An apt metaphor would be the touchstone—the wonderstone used in ancient
times to measure the purity of gold and other precious metals that were used
as currency. A metal coin was rubbed against a dark siliceous stone like jasper,
and a colorful residue would be indicative of the metal’s purity.

Organizations like Oracle have defined these benchmarks in the form of professional certifications, to play the role of touchstones.

Validated skills establish the credibility of an individual’s ability in programming
in a particular language or their understanding of a platform, methodology,
or practice to prospective employers.

## 40. Java is 90's kid (_Ben Evans_)

_There are only two kinds of languages: the ones people complain about and the
ones nobody uses._ — Bjarne Stroustrup

Many think that Java is old language, that you are writing instant legacy code.

Through the lens of 2020 and beyond, Java is sometimes seen as a mainstream,
middle-of-the-road language. What that narrative misses is that the
world of software has radically changed since Java’s debut. Big ideas such as
virtual machines, dynamic self-management, JIT compilation, and garbage
collection are now part of the general landscape of programming languages.

Though some may view Java as The Establishment, it’s really the mainstream
that has moved to encompass the space where Java has always been. Underneath
the veneer of enterprise respectability, Java is still a ’90s kid.

## 41. Java Programming from a JVM Performance Perspective (_Monica Beckwith_)

**Tip #1: Don’t Obsess Over Garbage**
- Avoid excessive focus on the garbage your application produces.
- The JVM’s garbage collector (GC) and JIT compiler (C1 and C2) handle memory management and optimizations.
- Trust the adaptive compiler and use tools for verification when needed.

**Tip #2: Characterize and Validate Your Benchmarks**
- Properly characterize and validate benchmarking tests.
- Differences in performance metrics can stem from changes in default GCs (e.g., Parallel GC to G1 GC).
- Isolate the “unit of test” to avoid interference from other test system components.

**Tip #3: Allocation Size and Rate Still Matter**
- Monitor GC logs to understand allocation behavior.
- Large objects may be categorized as “humongous” in G1 GC, affecting allocation efficiency.
- High allocation rates can overwhelm GC’s marking algorithms and cause issues.

**Tip #4: An Adaptive JVM Is Your Right and You Should Demand It**
- Utilize adaptive JIT and GC advancements for better performance.
- Provide feedback to the Java community to drive innovation and improvements in JVM features.
- Test new features to help enhance the JVM ecosystem.

## 42. Java Should Feel Fun (_Holly Cummins_)

- Fun in programming includes exploration, play, puzzles, games, and satisfying work, all of which Java supports through debugging, learning new features, and more.
- Tools like Lombok reduce verbosity by generating boilerplate code, and simplifying tasks like auto-instrumenting trace can make coding more enjoyable and error-free.
- The introduction of lambdas in Java 8 provided new, enjoyable ways to write functional code, combining exploration and the challenge of functional programming.
- Ensuring code is fun to write and maintainable is crucial; automating tedious tasks and fostering a positive coding environment enhances productivity and developer happiness.

## 43. Java’s Unspeakable Types (_Ben Evans_)

What is _null_?

```java
String s = null;
Integer i = null;
Object o = null;
```
The symbol null is a value that can be assigned to any reference type.

Java Language Specification (JLS), in Section 4.1:
> There is also a special null type, the type of the expression null, which has no name. Because the null type has no name, it is impossible to declare a variable of the null type or to cast to the null type.

Types we cannot declare as the types of variables = **unspeakable types** or _nondenotable types_
- Multicatch exception parameters in Java 7, which are essentially unions of multiple types.
- Anonymous classes, where the type cannot be explicitly declared but can still be used via type inference within the same method.

## 44. The JVM Is a Multiparadigm Platform: Use This to Improve Your Programming (_Russel Winder_)

- Java, originally an imperative and object-oriented language, has been used to build large systems using objects, methods, and explicit iteration, often leading to "hacks" to resolve problems.
-  Java 8 introduced significant changes, including method references, lambda expressions, and higher-order functions, shifting towards a more declarative style of programming.
- Java evolved into a multiparadigm language, blending object-oriented and functional programming concepts, similar to languages like Scala, which integrate both paradigms.
- The JVM, initially for web plugins, became a robust platform for various languages, supporting both dynamic (e.g., Groovy, JRuby) and static (e.g., Scala, Kotlin) languages, enhancing Java's ecosystem.
-  Leveraging the JVM, developers can use the most suitable language for each part of a project, combining static languages like Java and Kotlin with dynamic languages like Clojure and Groovy for optimal results.

## 45. Keep Your Finger on the Pulse (_Trisha Gee_)

= to be aware of the latest things that are happening in a certain industry

**Learning from Peers** \
Early job experiences emphasized the value of having knowledgeable colleagues to guide through technological changes, a crucial role for senior team members.

**Embracing Evolution** \
Accept that Java is constantly evolving with new versions, libraries, frameworks, and JVM languages. Staying informed about these changes is essential.

**Continuous Learning** \
Keep up with Java's frequent updates by staying aware of key trends and focusing on relevant new features and technologies to improve productivity and user experience.

## 46. Kinds of Comments (_Nicolai Parlog_)

- **Javadoc Comments for Contracts**
  - Use /** ... */ for classes, interfaces, fields, and methods.
  - Placed directly above the elements.
  - Act as a contract for API users and implementers.
  - Java 8 introduced @apiNote, @implSpec, and @implNote to specify user or implementer guidance.

```java
    /**
    * Returns the number of key-value mappings in this map. If the
    * map contains more than Integer.MAX_VALUE elements, returns
    * Integer.MAX_VALUE.
    *
    * @return the number of key-value mappings in this map
    */
    int size();
```

- **Block Comments for Context**
  - Enclosed in /* ... */ with no placement restrictions.
  - Commonly used at the beginning of classes or methods.
  - Provide implementation details or context (why the code was written a certain way).

```java
    /*
     * Implementation notes.
     *
     * This map usually acts as a binned (bucketed) hash table,
     * but when bins get too large, they are transformed into bins
     * of TreeNodes, each structured similarly to those in
     * java.util.TreeMap.
     * [...]
     */
```

- **Line Comments for Weird Things**
  - Start with // and must be repeated on every line.
  - Commonly placed above the line or block of code.
  - Useful for explaining code that uses unusual language features or is prone to subtle issues.
  
- **Last Words**
  - Make sure to pick the right kind of comment.
  - Don’t break expectations.
  - Comment your &#!*@$ code!
    https://nipafx.dev/talk-comment-your-code/

## 47. Know Thy flatMap (_Daniel Hinojosa_)

- Works with containers like Stream<T>, CompletableFuture<T>, Observable<T> (RXJava), and Flux<T> (Project Reactor).

### Usage and Limitations of map
- **Usage**: Applies a function to each element in a stream or collection.
  - Example: `Stream.of(1, 2, 3, 4).map(x -> x * 2).toList()` 
  - produces `[2, 4, 6, 8]`.
  
- **Limitations**: Using map with a plural creates a list of streams.
  - Example: `Stream.of(1, 2, 3, 4).map(x -> Stream.of(-x, x, x + 1)).toList()` 
  - produces streams of references.
```text
     [java.util.stream.ReferencePipeline$Head@3532ec19,
      java.util.stream.ReferencePipeline$Head@68c4039c,
      java.util.stream.ReferencePipeline$Head@ae45eb6,
      java.util.stream.ReferencePipeline$Head@59f99ea]
```

### Using flatMap
- Converts each element into multiple elements in a single stream.
  - Example: `Stream.of(1, 2, 3, 4).flatMap(x -> Stream.of(-x, x, x + 1)).toList()`
  - produces `[-1, 1, 2, -2, 2, 3, -3, 3, 4, -4, 4, 5]`.

### Advanced flatMap Example
- Given a Stream<Manager>, determine all employees' salaries including managers and their employees.
- Code:
  ```java
     List.of(manager1, manager2).stream()
       .flatMap(m -> Stream.concat(m.getEmployeeList().stream(), Stream.of(m)))
       .distinct()
       .mapToInt(Employee::getYearlySalary)
       .sum();
  ```

### General Advice
- Use map, filter, flatMap, or groupBy for data processing.
- Avoid premature use of terminal operations like forEach or collect to maintain stream laziness and optimization benefits.

## 48. Know Your Collections (_Nikhil Nanivadekar_)

| **Collection** | **Sortable** | **Orderable** | **Allows Duplicates** | **Details**                                               |
|----------------|--------------|---------------|-----------------------|-----------------------------------------------------------|
| **ArrayList**  | No           | Yes           | Yes                   | List implementation, predictable iteration order, O(n) contains operation |
| **LinkedList** | No           | Yes           | Yes                   | List implementation, predictable iteration order, O(n) contains operation |
| **HashMap**    | No           | No            | N/A                   | Map implementation, unique keys, O(1) key lookup, O(n) value lookup |
| **LinkedHashMap** | No           | Yes           | N/A                   | Map implementation, maintains insertion order, unique keys, O(1) key lookup |
| **TreeMap**    | Yes          | Yes           | N/A                   | Map implementation, keys sorted by comparator or natural order, O(log n) operations |
| **HashSet**    | No           | No            | No                    | Set implementation, backed by HashMap, unique elements, O(1) contains operation |
| **LinkedHashSet** | No           | Yes           | No                    | Set implementation, maintains insertion order, unique elements, O(1) contains operation |
| **TreeSet**    | Yes          | Yes           | No                    | Set implementation, backed by TreeMap, elements sorted by comparator or natural order, O(log n) contains operation |


## 49. Kotlin Is a Thing (_Mike Dunn_)
- **Kotlin Overview**
  - Kotlin is designed to be interoperable with Java and allows writing shorter, cleaner, and more modern code.


- **Java vs. Kotlin Examples**
  - **Java Model Example**:
    ```java
    public class Person {
      private String name;
      private Integer age;
      // getters and setters
    }
    public class Person {
      public Person(String name, Integer age) {
        this.name = name;
        this.age = age;
      }
      // getters and setters
    }
    ```
  - **Kotlin Model Example**:
    ```kotlin
    class Person(val name: String, var age: Int)
    ```

- **Delegation**
  - **Java Lazy Initialization**:
    ```java
    public class SomeClass {
      private SomeHeavyInstance someHeavyInstance = null;
      public SomeHeavyInstance getSomeHeavyInstance() {
        if (someHeavyInstance == null) {
          someHeavyInstance = new SomeHeavyInstance();
        }
        return someHeavyInstance;
      }
    }
    ```
  - **Kotlin Lazy Initialization**:
    ```kotlin
    val someHeavyInstance by lazy {
      SomeHeavyInstance()
    }
    ```

- **Null Safety**
  - **Java Null Checks**:
    ```java
    Object something = null;
    if (someObject != null) {
      if (someObject.someMember != null) {
        if (someObject.someMember.anotherMember != null) {
          something = someObject.someMember.anotherMember;
        }
      }
    }
    ```
  - **Kotlin Null Safety**:
    ```kotlin
    val something = someObject?.someMember?.anotherMember
    ```
 + many more :) 

## 50. Learn Java Idioms and Cache in Your Brain (_Jeanne Boyarsky_)

### Summary of "Learn Java Idioms and Cache in Your Brain" by Jeanne Boyarsky

- Common ways of expressing functionality that have general community agreement. Knowing idioms helps write code faster.

 - **Loop Implementation**:
    ```java
    public int loopImplementation(int[] nums) {
      int count = 0;
      for (int num : nums) {
        if (num > 0) {
          count++;
        }
      }
      return count;
    }
    ```
  - **Stream Implementation**:
    ```java
    public long streamImplementation(int[] nums) {
      return Arrays.stream(nums)
                   .filter(n -> n > 0)
                   .count();
    }
    ```

- **Importance of Learning Idioms**:
  - Familiarity with idioms like looping, conditions, and streams enhances coding speed.
  - Practicing these patterns helps internalize them.


  - Reads a file, removes blank lines, writes it back:
    ```java
    Path path = Paths.get("words.txt");
    List<String> lines = Files.readAllLines(path);
    lines.removeIf(t -> t.trim().isEmpty());
    Files.write(path, lines);
    ```
  - Easy to modify conditions (e.g., changing `removeIf` to remove lines longer than 60 characters).
  - Focus on what to accomplish, not how to read/write files.
  - Idioms often learned through frequent use rather than intentional study.
  - Repetition aids memory or at least helps in knowing where to find information.


- **Conclusion**:
  - Treat your brain as a cache for idioms and common library calls.
  - This efficiency allows focus on more complex tasks.