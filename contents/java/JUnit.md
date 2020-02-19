<frontmatter>
  title: "JUnit"
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

{{ booktitle | safe }}

# JUnit

<b>Author: [Lin Si Jie](https://github.com/sijie123)</b>

Reviewers: [Aadyaa Maddi](https://github.com/amad-person), [Marvin Chin](https://github.com/marvinchin)

<box id="article-toc">

* [JUnit‎](#junit)
  * [What is JUnit?‎](#what-is-junit)
  * [Why JUnit?‎](#why-junit)
  * [JUnit Features‎](#junit-features)
    * [Assertions API‎](#assertions-api)
    * [Before/After hooks‎](#before-after-hooks)
    * [Extension Model‎](#extension-model)
  * [Getting Started‎](#getting-started)
</box>

## What is JUnit?

**JUnit is an _automated testing framework_ for Java** i.e., it can be used to programmatically verify the actual behavior of Java code against the expected behavior.

Let's use a simple example to see how JUnit is used. Suppose we want to test the method `calculateArraySum(int[] values, int n)` which returns the sum of the first `n` elements of `values`.

```java
public class Utils {
  // Sum up first n elements in values, and return the result
  public static int calculateArraySum(int[] values, int n) {
    int sum = 0;
    for (int i = 1; i < n; i++) {
      sum += values[i];
    }
    return sum;
  }
}
```

Let's write a test to check the correctness of our code. This test will use a sample array of size 5 that sums to 15, and check that our `calculateArraySum` returns the correct result.

```java
public class UtilsTest {
  @Test
  public void calculateArraySum_fiveNumbers_correctAnswer() {
    int[] testArray = {1, 2, 3, 4, 5};
    assertEquals(15, calculateArraySum(testArray, 5));
  }
}
```
<tip-box>:bulb: By convention, we append `Test` to the name of the tested class. It is also good practice to write a descriptive name for the test, so other developers will know the intention and scope of each test.</tip-box>

That's it! We are ready to run the tests through our IDE. The `@Test` tag tells JUnit that the `calculateArraySum_fiveNumbers_correctAnswer` method is meant to be a test. JUnit will then automatically execute our test(s).

Let's have a look at the test results. 

<img src="{{baseUrl}}/contents/java/assertFailed.PNG" />

Hmm, the test seems to fail when we run it.
Did you notice the subtle bug in `calculateArraySum`?


We "forgot" to include the 0-th element of inputArray, a common off-by-one bug.
With JUnit, we can use automatic tests to ensure that our functions are correct. This is easier and less error-prone than repeatedly testing and calculating manually.

Now, let's fix the code.

```java
public class Utils {
  // Sum up first n elements in values, and return the result
  public static int calculateArraySum(int[] values, int n) {
    int sum = 0;
    for (int i = 0; i < n; i++) {
      sum += values[i];
    }
    return sum;
  }
}
```

<img src="{{baseUrl}}/contents/java/assertPass.PNG" />

Running the same unit test again, we now have a passing test. Our `calculateArraySum` function works as intended. 

To assure ourselves that `calculateArraySum` works for more scenarios (what if `n` = 0?), we can write more tests covering these cases.

```
public class UtilsTest {
  @Test
  public void calculateArraySum_fiveNumbers_correctAnswer() {
    int[] testArray = {1, 2, 3, 4, 5};
    assertEquals(15, calculateArraySum(testArray, 5));
  }
  @Test
  public void calculateArraySum_noNumbers_zero() {
    int[] testArray = {};
    assertEquals(0, calculateArraySum(testArray, 0));
  }
}
```

Unit tests like these help to prevent against regression. If `calculateArraySum` is ever changed, these tests can quickly verify that its functionality remains correct.

<p></p> 

## Why JUnit?

Here are some key reasons why developers should consider using JUnit, as compared to other Java testing tools like TestNG:

1. <b>JUnit is well-integrated with development tools.</b> It is supported by many popular IDEs such as IntelliJ IDEA, Eclipse, NetBeans, Visual Studio Code and many more. <b>This makes it convenient for developers to write and run tests.</b>

2. <b>JUnit is well-established.</b> JUnit has a longer history and a larger community. It is also more popular among developers (according to [Synk's 2018 report](https://snyk.io/blog/jvm-ecosystem-report-2018-tools/)). <b>This means that it is easier to find answers or get help with JUnit.</b>

<p></p> 

## JUnit Features

In addition to the basic example in the introduction, JUnit also has more powerful features that further simplifies the process of writing complex tests.

### Assertions API
In addition to the `assertEquals` method illustrated in the introduction, JUnit allows us to check for many other things. These include:
* `assertTrue`/`assertFalse`: checks whether statements return true/false as expected.
* `assertNull`/`assertNotNull`: checks whether something is null or not, without causing a NullPointerException.
* `assertArrayEquals`: yes, it loops through an entire array for us!
* `assertThrows`/`assertDoesNotThrow`: verify the actual error handling behavior against the expected error handling behavior.
* `assertTimeout`: ensures that a piece of code runs within time constraints - we don't want to keep the user waiting!

These powerful expressions means that we can write more expressive tests.
Without JUnit, developers can test by writing:
```java
try {
  int result = divide(100, 0); // Divides 100 by 0
} catch (ArithmeticException ae) {
  // This is the expected behaviour.
  return;
}
// If code is here, then the ArithmeticException wasn't thrown.
throw new AssertionError("Dividing 100 by 0 should throw an exception!");
```

With JUnit, we can replace the above code with just this line:
```java
assertThrows(ArithmeticException.class, () -> divide(100, 0));
```

This expression is a lot easier to understand, and as an added bonus, the code is also much shorter to write!

### Before/After hooks

Not every system can be tested so easily. Sometimes, testing is not as straightforward as running a function.
For example, we might want to store information in a database. We would have to first connect to the database before we can run any test.

JUnit exposes an API that allows developers to do this process easily and clearly. The `@BeforeAll` tag allows us to run code once before all tests. Using `@BeforeAll`, we can easily initialize our testing environment:

```java
class TestInvolvingDatabases {
  Database db;
  @BeforeAll
  public static void initializeDatabaseConnection() {
    // Initialize database connections
    db = connectToDatabase();
  }
  @Test
  public static void addUserTest() {
    db.addUser('NewUser');
    ...
  }
}
```

JUnit also offers a `@BeforeEach` tag to run code before every test. One use case is to ensure that we standardise the testing environment before each test.
For example, we can clear our database before each test to ensure that tests do not <trigger for="modal:affectEachOther" trigger="click">affect each other</trigger>.

```java
class TestInvolvingDatabase {
  Database db;
  @BeforeEach
  public static void resetDatabaseTable() {
    db.truncate("testTable");
  }
}
```

Similarly, we can use the `@AfterAll` and `@AfterEach` tags to run code after all tests, and after each test respectively:

```java
class TestInvolvingDatabases {
  Database db;
  DatabaseSnapshot snapshot;
  @AfterEach
  public static void restoreDatabaseTable() {
    db.restore(snapshot); // Restore previous state of database
  }
  @AfterAll
  public static void closeDatabaseConnection() {
    db.close(); // Explicitly close DB connection instead of timing out.
  }
  
}
```

### Extension Model

Consider a project where tests need to:
* connect to the database before all tests
* clear a database before each test to ensure consistency

One way would be to write helper methods `initializeDatabaseConnection()` and `resetDatabaseTable()` and invoke them as required, in the `@BeforeAll`/`@BeforeEach` methods.
However, this is prone to errors - if we do not explicitly call these methods, the database is not initialised or reset.

In JUnit, the extension model allows us to augment the test class. Instead of needing to invoke helper methods when we need them, we can introduce a `ManagedDatabase` extension that can automatically connect to a remote database and reset itself before each test.
This abstracts away the need to manage the database, allowing us to focus on testing our logic.

```java
class ManagedDatabase implements BeforeAllCallback, BeforeEachCallback {
  Database db;
  String url;

  public ManagedDatabase(String url) {
      this.url = url;
  }

  @Override
  public void beforeAll(ExtensionContext context) throws Exception {
      db = connectToDB(url);
  }
  
  @Override
  public void beforeEach(ExtensionContext context) throws Exception {
      db.truncate("testTable");
  }
}
```

Now, any code requiring a database can simply do the following to initialize a database connection that resets itself before every test:

```java
public class LogicTest {
  @RegisterExtension
  public ManagedDatabase db = new ManagedDatabase("localhost");
}
```

<p></p> 

## Getting Started

If this introduction has got you interested in using JUnit, do check out the following resources for an introduction to using JUnit:

* Introductory JUnit guide: [JUnit 5 Tutorial](https://howtodoinjava.com/junit-5-tutorial/)
* Comprehensive JUnit guide: [A Guide to JUnit 5](https://www.baeldung.com/junit-5)
* Extensions and Lifecycle of Tests: [Deep Dive into JUnit 5 Extension Model](https://www.infoq.com/articles/deep-dive-junit5-extensions)
* Official User Guide: [JUnit 5 User Guide](https://junit.org/junit5/docs/current/user-guide/)

</div>

<modal id="modal:affectEachOther">
  <div slot="modal-header" class="modal-title text-center">
    <h4>How will tests affect each other?</h4>
  </div>
  <p>If we do not reset a shared resource (e.g. databse / file), a test that uses the resource permanently changes the resource. As a result, other tests cannot assume anything about the initial state of this resource.</p>
  <p>For example, test A checks for a correctly set-up, empty database. Test B writes to an initially empty database. Without resetting the database, test A can fail if test B manages to runs first.</p>
</modal>  

</div>
