# 97 things every Java developer should know

Summary of every item from book **97 things every Java developer should know** by  Kevlin Henney and Trisha Gee.



## 1. All You Need Is Java (_Anders Nors_)

Three kinds of progammers:
- **Mort** - the opportunistic developer, doing quick fixes and making things up as he went along
- **Elvis** - the pragmatic programmer, building solutions for the ages while learning on the job
- **Einstein** - the paranoid programmer, obsessed with designing the most efficient solution and figuring everything out before writing his code.

_Libraries grew into frameworks with prescripted architectures. And as these frameworks became technology ecosystems, many of us forgot about the little language that could—Java._

Need to work wit files? - _java.nio_
Databases? - _java.sql_
HTTP server? - _com.sun.net.httpserver_

TLDR; Use Java as much as you can, don't just throw in dependencies.

## 2. Approval Testing (_Emily Bache_)

Did you ever try something like this:  `assertEquals("",functionCall())`

Where `functionCall` is returning a string and you’re not sure exactly what that string should be, but you’ll know it’s right when you see it?

That can be a problem when expected strings change -> you can use _approval testing tools_ that will store strings in file.
[ApprovalTests](https://approvaltests.com/)
[TextTests](http://texttest.sourceforge.net/)
More examples: [Link](https://www.softwaretestingmagazine.com/knowledge/approval-testing/)

Good situations for approval testing:
- _Code without unit tests that you need to change_ - turns into a problem of finding seams and carving out pieces of logic that return something interesting you can approve.
- _REST APIs and functions that return JSON or XML_ - nice when response is a bigger string (because you store it outside source code)
- _Business logic that builds a complex return object_ - Think of a Receipt or a Prescription or an Order. Good for non-tech persons.


If you already have tests that make assertions about strings that are longer than one line, then it is recommended finding out more about approval testing.

## 3. Augment Javadoc with AsciiDoc (_James Elliott_)

Sometimes Javadoc is not enough , because you need more than API documentation, though—much more than you can fit in the package and project overview pages Javadoc offers.

[AsciiDoc](https://asciidoctor.org/docs/what-is-asciidoc) is an alternative to Markdown.
[Quick refference](https://docs.asciidoctor.org/asciidoc/latest/syntax-quick-reference/)
There is [plugin](https://plugins.jetbrains.com/plugin/7391-asciidoc) for IntelliJ (*_.adoc_)
Chrome [extension](https://chromewebstore.google.com/detail/asciidoctorjs-live-previe/iaalpfgpbocpdfblpnhhgllgbdbchmia)

## 4. Be Aware of Your Container Surroundings (_David Delabassee_)

[JVM ergonomics](https://docs.oracle.com/en/java/javase/11/gctuning/ergonomics.html#GUID-DA88B6A6-AF89-4423-95A6-BBCBD9FAE781) enables the JVM to tune itself by looking at two key environmental metrics:
- number of CPUs
- available memory.

With these metrics, the JVM determines important parameters such as:
- which garbage collector to use
- how to configure it
- the heap size
- the size of the ForkJoinPool, and so on

Linux Docker container support allows the JVM to rely on Linux cgroups to get the metrics of resources allocated to the **container** it runs in.
Any JVM **older than JDK 8 update 191** is not aware that it is running within a container and **will access metrics from the host OS** and not from the container itself.

The following command shows which JVM parameters are configured by the JVM ergonomics:  `java -XX:+PrintFlagsFinal -version | grep ergonomic` TODO: probaj ovo mozda

JVM container support is **enabled by default** but can be disabled by using the `-XX:-UseContainerSupport` - this way you can observe and explore the impact of JVM ergonomics with and without container support.


## 5. Behavior Is “Easy”;State Is Hard (_Edson Yanaga_)

**inheritance** - You have class _Shape_ and other classes _Circle_, _Triangle_ that inherit properties of _Shape_.
**encapsulation** - state cannot be changed directly, so properties are enclosed in methods  (_getters_ and _setters_)
**polymorphism** - you can access objects of different types through the same interface. Two types:
- <ins>static (compile time)</ins> - method _overloading_, operator _overloading_ (C++, Kotlin)
- <ins>dynamic (runtime)</ins> - method _overriding_

Focus here is **encapsulation**. We have state and expose API for getting and changing that state (_getters_ and _setters_).
If our code doesn't work, bugs are 'easy' to find, but if our code seems to work but we still have bugs -> problems!
One of the biggest problem is with **inconsistent state**
How to solve it? **Immutability** is the one of the possible answers
>If we can guarantee that our objects are immutable,  we’ll never have an inconsistent state.

Therefore, don’t generate your setters automatically. Take time to think about them. Do you really need that setter in your code? And if you decide that you do, perhaps because of some framework requirement, consider using an **anticorruption layer** to protect and validate your internal state after those setter interactions.


## 6. Benchmarking Is Hard— JMH Helps (_Michael Hunger_)

**benchmarks** = Java programs intended to measure the performance of one or more elements of a system where the Java program is being executed
**Micro-benchmarking** = the process of measuring the performance of a small piece of code

Programs:
- **JMH** may be used to perform micro benchmarking on code units
-  Jmeter can be used to perform performance testing on applications

**JMH (Java Microbenchmarking Harness)**

-   Developed for OpenJDK
-   Consists of:
    -   A small library
    -   A build system plug-in
-   Library features:
    -   Provides annotations and utilities for creating benchmarks as annotated Java classes and methods
    -   Includes a BlackHole class to consume generated values, preventing code elimination
    -   Ensures correct state handling in multithreaded environments
-   Build system plug-in capabilities:
    -   Generates a JAR with the necessary infrastructure for running and measuring tests accurately
    -   Incorporates dedicated warm-up phases
    -   Supports proper multithreading
    -   Manages multiple forks and averages results across them

## 7.The Benefits of Codifying and Asserting Architectural Quality (_Daniel Bryant_)

- checking and publishing quality metrics within the build pipeline can prevent the gradual decay of architectural quality

>Once upon a time, an architect drew a series of nice architectural diagrams that illustrated the components of the system and how they should interact. Then the project got bigger and use cases more complex, new developers dropped in and old developers dropped out. This eventually led to new features being added in any way that fit. Before long, everything depended on everything, and any change could have an unforeseeable effect on any other component.

**ArchUnit - An Open Source Library**

-   Extensible library for checking the architecture of Java code using Java unit-test frameworks like JUnit or TestNG
-   Checks for cyclic dependencies
-   Checks dependencies between packages, classes, layers, and slices
-   Analyzes Java bytecode and imports all classes for analysis

## 8.Break Problems and Tasks into Small Chunks (_Jeanne Boyarsky_)


-   **Break Problems into Small Chunks**
    -   Simplifies managing large tasks
    -   Makes each piece easier to handle and test
-   **Write Automated Tests for Small Problems**
    -   Ensures each chunk works correctly
    -   Helps identify issues early
-   **Commit Frequently**
    -   Provides rollback points
    -   Prevents loss of significant progress if issues arise
- Even "special" tasks could be divided into smaller tasks


## 9.Build Diverse Teams (_Ixchel Ruiz_)
-   **Importance in Software Development**
    -   Cooperation is essential in modern software teams.
    -   Teams need to function like pit crews, with each member contributing.
   
-   **Diversity's Role in Innovation**
    
    -   Four types of diversity: industry background, country of origin, career path, and gender.
    -   Diverse teams bring varied perspectives, leading to innovation.
    -   High gender diversity in management teams can increase innovation revenue by 8%.
-   **Benefits of Diverse Teams**
    
    -   Increases pool of information, skills, and networks.
    -   Constructive debate in a positive environment leads to creative solutions.
-   **Challenges of Diversity**
    
    -   Potential for conflict and factionalism.
    -   Communication issues and digital mishaps can create "us versus them" mentality.
-   **Building Effective Teams**
    
    -   Develop psychological safety and trust.
    -   Trust encourages risk-taking and cooperation.
    -   Speaking up is easier, leading to participation and novel ideas.
-   **Personality in Teams**
    
    -   Recognize and value different personalities and approaches.
    -   Encourage a balance between diverse traits for better team dynamics.

## 10. Builds Don’t Have To Be Slow and Unreliable (_Jenn Strater_)

 -   Codebase and development team growing daily
 - Build times increased from 8 minutes to over 15 minutes
 - Initially manageable, later became frustrating
-   **Impact of Long Build Times**
    
    -   Increased context switching
    -   Reliance on CI server for test validation
    -   Delayed debugging process
-   **Problems with Flaky Tests**
    
    -   Naive solution: Ignoring failing tests (@Ignore)
    -   Pushed issues down the line to CI step
    -   Blocked entire team when tests failed post-merge
-   **Efforts to Fix Tests**
    
    -   Long feedback cycles (15+ minutes)
    -   Wasted days tracking down bugs due to lack of relevant data
-   **Developer Productivity Engineering (DPE)**
    
    -   Practice of improving developer experience through data
    -   Focus on measuring build performance and tracking regressions
    -   Analysis of results to find bottlenecks
-   **Benefits of DPE**
    
    -   Sharing detailed reports (e.g., Gradle build scans) with teammates
    -   Comparing failing and passing builds to identify issues
    -   Optimizing the build process to reduce frustration
-   **Outcome**
    
    -   Teams focusing on DPE are happier and more productive
    -   Prevents build performance issues and improves overall developer experience

## 11. "But it works on my local machine"

- The process of setting up the infrastructure to build source code on a developer's machine can be challenging
- Questions may arise regarding the required JDK version and distribution, the operating system compatibility, the IDE and its version, and the version of the build tool needed
- Every project should have a clearly defined set of tools that are compatible with the technical requirements to compile, test, execute, and package the code
- A better solution is the use of a wrapper, which helps with provisioning a standardized version of the build tool runtime without manual intervention.
- In the Java space, examples of wrappers include the <b>Gradle Wrapper</b> and the <b>Maven Wrapper</b>
- The Maven Wrapper requires the Maven runtime to be installed on the machine to generate the Wrapper files.
- These Wrapper files represent the scripts, configuration, and instructions every developer of the project uses to build the project with a predefined version of the Maven runtime.
- The wrapper concept improves build reproducibility and maintainability, eliminating the "But it works on my machine!" issue.

## 12. The Case Against Fat JARS
- The use of fat JARs for deploying Java applications has become popular with the rise of microservice architecture style, DevOps, and cloud-native technologies.
- Fat JARs can have disadvantages such as large size, long build process, lack of shared dependencies, and challenges with integration across services.
- Some organizations are now creating "skinny JARs" to address these challenges
- The HubSpot engineering team created a new Maven plug-in called SlimFast to separate the application code from the associated dependencies, building and uploading two separate artifacts.
- The SlimFast plug-in adds a Class-Path manifest entry to the skinny JAR that points to the dependencies’ JAR file, and generates a JSON file with information about all the dependency artifacts.
- At deploy time, the build downloads all of the application’s dependencies, but then caches these artifacts on each of the application servers.
- The result is that at build time, only the application’s skinny JAR is uploaded to the remote storage, and at deploy time, only this same thin JAR needs to be downloaded to the target deployment environment.

## 13. The code restorer

- Current software practices often suffer from haste, leading to fragile systems that can easily collapse under small disturbances
- Many teams and companies focus only on short-term goals, often neglecting long-term stability
- This approach can lead to a cycle of developers, managers, and entrepreneurs exiting a project or company before its potential issues become apparent
- The software industry can be compared to furniture, where some pieces are built to last hundreds of years, while others are expected to crumble within a decade

- There is a middle ground in the form of the code restorer, a role focused on reshaping existing codebases to make them manageable again
- The code restorer's job is not to recreate the same thing but better, but to improve the existing codebase by adding tests, breaking down complex classes, removing unused functionality, ...
- <i>focusing on profit and building something that holds for a while</i> <b>VS</b> <i>focusing on durability and carefully reshaping the code</i>

## 14. Concurrency on the JVM

- Raw threads were the original concurrency model in Java, but they have limitations.
- Threads and locks are low-level and hard to use correctly, leading to issues with shared mutable state and reduced parallelism
- Java doesn't support distributed memory, making it hard to scale multithreaded programs across multiple machines
- Coordinating threads via distributed queues instead of locks can overcome shared memory limitations
- Akka brings the actor model to the JVM, implementing concurrency through message flow between actors
  
    https://doc.akka.io/docs/akka/current/typed/guide/actors-intro.html?language=scala
- Clojure offers software transactional memory, turning the JVM heap into a transactional data set
- Java 8 lambdas promote functional programming properties like immutability and referential transparency
- There's no one-size-fits-all solution for concurrency. Different options have different trade-offs

## 15. CountDownLatch - Friend or Foe?

```java
ExecutorService pool = Executors.newFixedThreadPool(8);
    Future<?> future = pool.submit(() -> {
        // Your task here
    });
```
```java
int tasks = 16;
CountDownLatch latch = new CountDownLatch(tasks);
for (int i = 0; i < tasks; i++) {
    Future<?> future = pool.submit(() -> {
        try {
        // Your task here
        }
        finally {
            latch.countDown();
        } 
    });
}
if (!latch.await(2, TimeUnit.SECONDS)) {
    // Handle timeout
}
```
- CountDownLatch is useful in many different situations. It becomes especially useful when you’re testing your concurrent code, since it allows you to make sure that all the tasks are complete before checking their results
- CountDownLatch is very useful, but there’s one important catch: you shouldn’t use it in production code that makes use of concurrent libraries or frame‐ works, such as Kotlin’s coroutines or Spring WebFlux