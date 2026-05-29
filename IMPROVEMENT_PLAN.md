# Solutionary — Improvement Plan

A prioritized roadmap of improvements grouped by impact area.

---

## 1. Complete Unfinished Features

These are methods already listed in the UI dropdown but not yet implemented:

- **Fixed Point Iteration** — the ATES combo box lists it, but there is no corresponding class
- **Secant Method** — same situation; add `Secant.java` and wire it into `ATESPanel`
- **Newton's Forward/Backward Difference Interpolation** — `LegacyInterpolationPanel` exists but is currently parked in `ExtraTools`; promote and complete it

---

## 2. Architecture & Code Quality

### Extract a Common `NumericalMethod` Interface
All solver classes (`Bisection`, `FalsePosition`, `NewtonRaphson`, etc.) share the same signature pattern but have no shared contract. A simple interface would make `ATESPanel` generic and eliminate the long `if/else` dispatch on the combo box selection:

```java
public interface RootFinder {
    double execute(Expression expr, double a, double b, double tolerance, DefaultTableModel model);
}
```

### Separate Business Logic from Swing
`RungeKutta.java` builds `JLabel` components directly inside the algorithm class. Solver classes should return plain data (arrays, lists, records) and let the panel classes handle display. This makes the math testable in isolation.

### Package Naming
Package names currently use `PascalCase` (`ATES`, `CurveFitting`, `ExtraTools`). Java convention is `lowercase` (`ates`, `curvefitting`, `extratools`). Renaming avoids IDE warnings and confusion with class names.

### Replace Magic Numbers with Named Constants
Values like `101` (max iterations), `1e-10` (singularity threshold), and UI sizes (`FIELD_HEIGHT = 64`) are scattered across multiple files. Consolidate them into a single `Constants.java` or per-class `static final` fields with explanatory names.

---

## 3. Testing

There are currently no tests. Since the algorithm classes are pure functions (inputs in, result out), they are easy to unit-test:

- Add JUnit 5 to `pom.xml`
- Write tests for each solver against known analytical results (e.g., `f(x) = x² - 2`, expected root ≈ `1.41421`)
- Test edge cases: singular matrices in Gauss Elimination, non-bracketing intervals in Bisection, zero-derivative in Newton-Raphson

---

## 4. User Experience

### Input Validation & Error Messages
Currently errors surface through `ErrorUtil` dialog boxes. Improve by:
- Highlighting the offending field in red before the user clicks Calculate
- Showing inline messages rather than modal dialogs for non-critical issues

### Result Export
Add a **Copy to Clipboard** or **Export CSV** button on iteration tables so users can paste results into lab reports or Excel.

### Graphing
Integrate a lightweight chart library (e.g., [JFreeChart](https://www.jfree.org/jfreechart/)) to:
- Plot `f(x)` and visually mark the found root in the ATES module
- Show the regression curve overlaid on scatter data in Curve Fitting

### Recent Activity
`DashBoardPanel` has a "Recent Activity" label with no backing data. Persist a simple history (method used, inputs, result) in a local file or `Preferences` and display the last 5 runs on the dashboard.

---

## 5. Build & Distribution

### Add a Proper Maven Build Configuration
The `pom.xml` has no `<build>` section, so `mvn package` produces a jar without bundled dependencies. Add the `maven-assembly-plugin` or `maven-shade-plugin` to produce a self-contained fat jar:

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-shade-plugin</artifactId>
    <version>3.5.0</version>
    <configuration>
        <transformers>
            <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                <mainClass>Main</mainClass>
            </transformer>
        </transformers>
    </configuration>
    <executions>
        <execution><phase>package</phase><goals><goal>shade</goal></goals></execution>
    </executions>
</plugin>
```

### Cross-Platform Packaging
Use [jpackage](https://docs.oracle.com/en/java/docs/jdk/jpackage/) (bundled with JDK 14+) to produce native installers (`.exe`, `.dmg`, `.deb`) so users don't need a JDK pre-installed.

### CI Pipeline
Add a GitHub Actions workflow that builds and runs tests on every push:

```yaml
# .github/workflows/build.yml
on: [push, pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-java@v4
        with: { java-version: '23', distribution: 'temurin' }
      - run: mvn verify
```

---

## Priority Summary

| Priority | Item |
|---|---|
| High | Complete Fixed Point Iteration & Secant Method |
| High | Fat-jar Maven build (app currently can't be distributed) |
| High | Unit tests for all solver classes |
| Medium | Extract `RootFinder` interface; decouple Swing from algorithm logic |
| Medium | Input validation with inline field highlighting |
| Medium | Result export (CSV / clipboard) |
| Low | Graphing / plotting |
| Low | Recent activity history on dashboard |
| Low | Native installer via jpackage |
| Low | Package rename to lowercase |
