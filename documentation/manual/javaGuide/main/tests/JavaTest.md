# Testing your application

Test source files must be placed in your application’s `test` folder. You can run tests from the Play console using the `test` (run all tests) and `test-only` (run one test class: `test-only my.namespace.MySpec`) tasks.

## Using JUnit

The default way to test a Play 2 application is with [[JUnit| http://www.junit.org/]].

```
package test;

import org.junit.*;

import play.mvc.*;
import play.test.*;
import play.libs.F.*;

import static play.test.Helpers.*;
import static org.fest.assertions.Assertions.*;

public class SimpleTest {

    @Test 
    public void simpleCheck() {
        int a = 1 + 1;
        assertThat(a).isEqualTo(2);
    }

}
```

> **Note:** A new process is forked each time `test` or `test-only` is run.  The new process uses default JVM settings.  Custom settings can be added to `play.Project.settings` in `Build.scala`.  For example:  
> ```
> javaOptions in (Test) += "-Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=9998",
> javaOptions in (Test) += "-Xms512M",
> javaOptions in (Test) += "-Xmx1536M",
> javaOptions in (Test) += "-Xss1M",
> javaOptions in (Test) += "-XX:MaxPermSize=384M"
> ```

## Running in a fake application

If the code you want to test depends on a running application, you can easily create a `FakeApplication` on the fly:

```
@Test
public void findById() {
  running(fakeApplication(), new Runnable() {
    public void run() {
      Computer macintosh = Computer.find.byId(21l);
      assertThat(macintosh.name).isEqualTo("Macintosh");
      assertThat(formatted(macintosh.introduced)).isEqualTo("1984-01-24");
    }
  });
}
```

You can also pass (or override) additional application configuration, or mock any plugin. For example to create a `FakeApplication` using a `default` in-memory database:

```
fakeApplication(inMemoryDatabase())
```

> **Next:** [[Writing functional tests | JavaFunctionalTest]]