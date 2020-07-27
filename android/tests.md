# Tests in Android Development

App has 2 test types:

1. **Local unit tests** - tests which can run on the JVM.
2. **Instrumented unit tests** - tests which require the Android system.

---

Tests can be seperated to three main branches.

1. Unit Tests
2. Integrations Tests
3. End to End Tests

Our testing strategy should include each of these types.

## Unit Tests

- Unit tests scope of single method or class.
- When test fails we know exactly where the error is.
- Unit tests meant to run fast. Because they also meant to be run often.

![Unit Test Scope](/-images/unit_test_scope.png){:height="50%" width="50%"}

## Integration Tests

- Several classes or a single feature
- Ensure classes work together successfully

![Integration Test Scope](/-images/integration_test_scope.png){:height="50%" width="50%"}

## End-to-End Tests

- Combination of features working together
- Test the app work as a whole
- Simulate real usage, almost always instrumented

![End-to-End Test Scope](/-images/end_to_end_test_scope.png){:height="50%" width="50%"}

---

General Test Code Percentage

![General Test Percentage](/-images/test_percentage.png){:height="50%" width="50%"}

**REFERENCES**
: <https://www.udacity.com/course/advanced-android-with-kotlin--ud940#>

**< [Go back to Android section](../android)**
