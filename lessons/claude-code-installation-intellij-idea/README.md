# Claude Code with IntelliJ IDEA: Setup & Your First Java Project

## Useful links

- [Claude Code overview](https://code.claude.com/docs/en/overview) — What Claude Code is and how it works.
- [Claude plans and pricing](https://claude.com/pricing) — Individual subscription options and separate API pricing.
- [Claude Code installation](https://code.claude.com/docs/en/setup) — Official installation commands for supported operating systems.
- [Windows installation troubleshooting](https://code.claude.com/docs/en/troubleshoot-install) — Help with installation or PATH problems.
- [Claude Code with JetBrains IDEs](https://code.claude.com/docs/en/jetbrains) — IntelliJ IDEA integration information.

## Claude Code installation (Windows PowerShell)

```powershell
irm https://claude.ai/install.ps1 | iex
```

If the installer says the Claude Code directory is missing from your Windows user `Path`, add this entry to **Environment Variables → User variables → Path** (only if absent), then open a new PowerShell window:

```text
%USERPROFILE%\.local\bin
```

## Java development prompts

### Inspect the Maven Project

```text
Inspect this Maven project.

Which Java version is it configured to compile with?
Where is that version configured?

Also determine which JDK is installed and active.

Do not modify any files.
```

### Create the Calculator Class

```text
Create a Calculator class in the
com.example.calculator package.

Add a public int add(int a, int b)
method that returns the sum.
```

### Add Multiplication and Compile

```text
Add a public int multiply(int a, int b)
method to the existing Calculator class.

The method should return the product
of the arguments.

Then compile the project using `mvn compile`.
```

### Plan Subtraction and JUnit Tests

```text
Inspect the existing Calculator project.

Create an implementation plan to:

1. Add a public int subtract(int a, int b)
   method to Calculator.

2. Add JUnit Jupiter tests for add,
   multiply, and subtract.

3. Review pom.xml and include only
   necessary Maven testing configuration.

4. Verify the implementation using `mvn test`.

Keep the solution simple.
Do not implement anything yet.
```

### Add Division and Run Tests

```text
Add a public int divide(int a, int b)
method to the existing Calculator class.

Use Java integer division.

Add JUnit Jupiter tests for the method
and run all Maven tests using `mvn test`.

Limit code changes to Calculator.java and
CalculatorTest.java. Do not change pom.xml
or add new dependencies.
```

### Review Division Tests (Optional)

```text
Review divide(int a, int b) and its tests.

Ensure tests verify that 7 / 2 returns 3
and division by zero throws
ArithmeticException.

Keep the implementation simple.
Rerun the tests using `mvn test`.
```

### Inspect IntelliJ Diagnostics (Optional)

```text
Check IntelliJ IDEA's inspection diagnostics
for Calculator.java.

Explain any errors or warnings you find.
Do not modify the code.
```

