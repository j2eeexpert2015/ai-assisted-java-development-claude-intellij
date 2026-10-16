# Claude Code with IntelliJ IDEA: Setup & Your First Java Project

Follow along with the Java Maven Calculator demonstration. The prompts below follow the recording order so you can jump to each task and copy its instructions into Claude Code.

## Contents

- [Useful Resources](#useful-resources)
  - [Claude Code Overview](#claude-code-overview)
  - [Plans and Pricing](#plans-and-pricing)
  - [Claude Code Installation](#claude-code-installation)
  - [Windows Installation Troubleshooting](#windows-installation-troubleshooting)
  - [Claude Code with IntelliJ IDEA](#claude-code-with-intellij-idea)
- [Windows PowerShell Installation](#windows-powershell-installation)
- [Java Development Prompts](#java-development-prompts)
  - [Inspect the Maven Project](#inspect-the-maven-project)
  - [Create the Calculator Class](#create-the-calculator-class)
  - [Add Multiplication and Compile](#add-multiplication-and-compile)
  - [Review Selected Method — Selection Context](#review-selected-method--selection-context)
  - [Plan Subtraction and JUnit Tests](#plan-subtraction-and-junit-tests)
  - [Refine the Plan (Optional)](#refine-the-plan-optional)
  - [Add Division and Run Tests](#add-division-and-run-tests)
  - [Review Division Tests (Optional)](#review-division-tests-optional)
  - [Inspect IntelliJ Diagnostics](#inspect-intellij-diagnostics)

## Useful Resources

### Claude Code Overview

Official introduction to Claude Code.

https://code.claude.com/docs/en/overview

### Plans and Pricing

Claude subscriptions and pricing information.

https://claude.com/pricing

### Claude Code Installation

Official installation instructions for supported operating systems.

https://code.claude.com/docs/en/setup

### Windows Installation Troubleshooting

Help with Windows installation, PATH, and command-discovery problems.

https://code.claude.com/docs/en/troubleshoot-install

### Claude Code with IntelliJ IDEA

Official JetBrains plugin and IntelliJ integration documentation.

https://code.claude.com/docs/en/jetbrains

## Windows PowerShell Installation

If Claude Code is not installed yet, open **PowerShell** and run:

```powershell
irm https://claude.ai/install.ps1 | iex
```

If the installer reports that the Claude Code directory is missing from your Windows user `Path`, add the following entry under **Environment Variables → User variables → Path**, but only if it is not already present:

```text
%USERPROFILE%\.local\bin
```

Open a new PowerShell window after updating `Path`. See [Claude Code Installation](#claude-code-installation) and [Windows Installation Troubleshooting](#windows-installation-troubleshooting) for details.

## Java Development Prompts

The prompts follow the order of the lesson. This lesson demonstrates **IDE Diff Viewing, Selection Context, and Diagnostic Sharing**. Use optional prompts only if a result requires refinement; review proposed changes and command permissions before approval.

### Inspect the Maven Project

Identify the Java version in Maven and the active JDK, without changing project files.

```text
Inspect this Maven project.

Which Java version is it configured to compile with?
Where is that version configured?

Also determine which JDK is installed and active.

Do not modify any files.
```

### Create the Calculator Class

Use Manual mode and review the proposed new Java file in IntelliJ's Diff Viewer.

```text
Create a Calculator class in the
com.example.calculator package.

Add a public int add(int a, int b)
method that returns the sum.
```

### Add Multiplication and Compile

Use Accept Edits for a small, predictable change, and inspect the Maven compilation output.

```text
Add a public int multiply(int a, int b)
method to the existing Calculator class.

The method should return the product
of the arguments.

Then compile the project using `mvn compile`.
```

### Review Selected Method — Selection Context

**Before submitting:** Select the entire `multiply()` method in the IntelliJ editor. Claude Code picks up the selected lines automatically. Do not add an explicit `@` file reference for this exercise.

```text
Review the method selected in IntelliJ.

Are there any important edge cases
or limitations?

Do not modify the code.
```

### Plan Subtraction and JUnit Tests

Use Plan mode to review the multi-file implementation approach and keep the tests focused.

```text
Add subtraction to our Calculator and set up
JUnit Jupiter tests for addition, multiplication,
and subtraction.

Keep the tests focused and easy to understand.
Cover normal behavior, negative values, and
zero where relevant.

Avoid redundant test cases and unnecessary
testing infrastructure.

First, inspect the project and propose a plan.
Do not implement anything yet.
```

### Refine the Plan (Optional)

**Use when the proposed plan includes work outside the requirement**, such as changes to existing methods, special overflow handling, or overflow-specific tests. In the plan approval menu, choose **Tell Claude what to change**, enter this feedback, and review the updated plan before approving implementation.

```text
Refine the plan with these requirements:

1. Keep all existing methods unchanged.

2. Add a subtraction method and create focused
JUnit Jupiter tests for all Calculator methods,
including the new subtraction method.

3. Cover normal values, negative values, and
zero where relevant. Avoid unnecessary overflow
handling, overflow-specific tests, and optional
overflow checks.

4. Include only the Maven testing configuration
needed to run the tests.

Keep the solution simple and avoid unrelated
changes.

Update the plan only. Do not implement anything yet.
```

### Add Division and Run Tests

Use Auto mode on a bounded task, then review the implementation, tests, Maven output, and changed files. File boundaries specified in a prompt are instructions, not enforced permission restrictions.

```text
Add a public int divide(int a, int b)
method to the existing Calculator class.

Use standard Java integer division.

Add focused JUnit Jupiter tests that verify:
- 7 / 2 returns 3
- Division by zero throws ArithmeticException
- Division with a negative number

Keep the tests concise. Avoid redundant
boundary tests and unrelated refactoring.

Run all Maven tests using `mvn test`.

Only modify Calculator.java and
CalculatorTest.java. Do not modify pom.xml
or add new dependencies.
```

### Review Division Tests (Optional)

Use only if the first division implementation or its tests are incomplete.

```text
Review divide(int a, int b) and its tests.

Ensure tests verify that 7 / 2 returns 3
and division by zero throws
ArithmeticException.

Keep the implementation simple.
Rerun the tests using `mvn test`.
```

### Inspect IntelliJ Diagnostics

**Before submitting:** Temporarily insert `missingMethod();` **inside** the `add()` method in `Calculator.java` and allow IntelliJ to report the error. Use Manual mode for this read-only diagnostic exercise.

```text
Check IntelliJ IDEA's inspection diagnostics
for Calculator.java.

Explain any errors or warnings you find.
Do not modify the code.
```

Check that Claude actually retrieves IntelliJ diagnostic information rather than only reading the source file. After the demonstration, remove the temporary invalid statement.
