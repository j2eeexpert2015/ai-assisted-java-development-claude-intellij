# Claude Code with IntelliJ IDEA: Setup & Your First Java Project

This page contains the **commands and prompts used in the lesson**, in demonstration order. It is intended for viewers to copy and follow—not as a narration script.

> **Environment used in the video:** Windows PowerShell, IntelliJ IDEA, Java 25, pre-installed Maven, and Git. A Claude Code-enabled account is needed. Commands for other operating systems are available in the [official setup documentation](https://code.claude.com/docs/en/setup).

## Claude Code installation

Use the **official** [Claude Code setup instructions](https://code.claude.com/docs/en/setup) and, in **Windows PowerShell**, run:

```powershell
irm https://claude.ai/install.ps1 | iex
```

If the installer reports that Claude Code's installation directory is missing from `Path`, open Windows **Edit environment variables for your account** → **User variables** → **Path** → **Edit** → **New** and add the following *only if not already present*:

```text
%USERPROFILE%\.local\bin
```

Close and reopen PowerShell before verifying. The directory above is for the `claude` executable; it is **not** the working project directory.

```powershell
claude --version
```

If the command isn't found after configuring `Path`, follow [Anthropic's installation troubleshooting](https://code.claude.com/docs/en/troubleshoot-install).

## Start Claude Code in a fundamentals folder

```powershell
New-Item -ItemType Directory -Force "C:\ClaudeProjects\claude-code-basics"
Set-Location "C:\ClaudeProjects\claude-code-basics"
claude
```

On first launch, follow the on-screen sign-in and folder-trust prompts. Choose the authentication option appropriate for your plan. The folder above is for the introductory CLI demonstration; it is separate from the Java project created below.

### Claude Code commands introduced

Enter these inside the **Claude Code prompt**, not the PowerShell command line:

```text
/model
/usage
/effort
```

You may also encounter `/login` and `/logout` for account sign-in management. Model and effort choices and available usage depend on your account and installed version. The video uses Sonnet for the Java demonstration; if Opus initially appears, you can select Sonnet with `/model`.

## IntelliJ IDEA and Java project setup

Install the Claude Code integration from **IntelliJ IDEA → Settings → Plugins → Marketplace**. Use the official listing and restart IntelliJ if prompted. Claude Code CLI installation should already be complete.

Create a **Java/Maven** project in IntelliJ using:

| Setting | Value |
| --- | --- |
| Project name | `claude-java-starter` |
| Location | `C:\ClaudeProjects\claude-java-starter` |
| JDK | 25 |
| Build system | Maven |
| Group ID | `com.example` (check Advanced Settings if hidden) |
| Git repository | Enable **Create Git repository** |

The video assumes Java 25, Maven, and Git are **already installed**. After creating/opening the project, run the following in **IntelliJ's integrated PowerShell terminal**:

```powershell
java -version
mvn -version
git --version
claude --version
```

Verify that `mvn -version` reports the intended Java runtime (JDK 25). If the IDE was open while Windows `Path` changed, fully restart it. The demonstration uses standard `mvn` commands, **not Maven Wrapper**.

Commit the untouched project as **`Initial Maven project`** before generating any Calculator code. The initial project contains no `Calculator.java`.

## Working in Claude Code inside IntelliJ IDEA

Launch Claude Code from the IntelliJ integration and confirm that the working directory is:

```text
C:\ClaudeProjects\claude-java-starter
```

The demo explores **Manual**, **Accept Edits**, **Plan**, and **Auto** modes. Use **Shift+Tab** to cycle through the modes available in your version, and confirm the active mode before executing a prompt. Automatic approval modes still require careful review of changes.

### Prompt sequence — Calculator example

Follow these prompts in order; later prompts depend on files and configuration created by earlier prompts.

**Mode:** Manual. Before submitting, open `pom.xml` and select the Java-version configuration lines if you want to demonstrate editor selection context.

### Prompt 1 — Inspect the Maven project

Paste into **Claude Code**:

```text
Inspect this Maven project.

Which Java version is it configured to compile with?
Where is that version configured?

Also determine which JDK is installed and active.

Do not modify any files.
```

**Mode:** Manual. Review the new file and IDE diff before approving changes.

### Prompt 2 — Create Calculator and add()

Paste into **Claude Code**:

```text
Create a Calculator class in the
com.example.calculator package.

Add a public int add(int a, int b)
method that returns the sum.
```

**Mode:** Accept Edits. Inspect the source change and any Maven command approval.

### Prompt 3 — Add multiply() and compile

Paste into **Claude Code**:

```text
Add a public int multiply(int a, int b)
method to the existing Calculator class.

The method should return the product
of the arguments.

Then compile the project using `mvn compile`.
```

**Mode:** Plan. Review the proposed plan *before* approving implementation; choose manual approval if offered.

### Prompt 4 — Plan subtraction and JUnit tests

Paste into **Claude Code**:

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

**Mode:** Auto (if available). Inspect the full diff and test results; automatic execution is not proof of correctness.

### Prompt 5 — Add divide() and run tests

Paste into **Claude Code**:

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

### Optional correction prompt — only if division tests need improvement

```text
Review divide(int a, int b) and its tests.

Ensure tests verify that 7 / 2 returns 3
and division by zero throws
ArithmeticException.

Keep the implementation simple.
Rerun the tests using `mvn test`.
```

## Independent verification

After the changes, run your JUnit tests in IntelliJ, and run a clean Maven test build in the integrated terminal:

```powershell
mvn clean test
```

Review the Git diff against the initial commit. Confirm the methods behave correctly, including Java integer division (`7 / 2` evaluates to `3`) and division by zero (`ArithmeticException`). Check that no unrelated files were modified.

### Optional IDE diagnostics prompt — only if useful warnings or errors exist

```text
Check IntelliJ IDEA's inspection diagnostics
for Calculator.java.

Explain any errors or warnings you find.
Do not modify the code.
```

## Official references

- [Claude Code installation](https://code.claude.com/docs/en/setup)
- [Claude Code in JetBrains IDEs](https://code.claude.com/docs/en/jetbrains)
- [Claude Code permission modes](https://code.claude.com/docs/en/permission-modes)
- [Git for Windows](https://git-scm.com/install/windows)

The video demonstrates a starting workflow, not a guarantee that generated code will be identical across models and versions. Always inspect AI changes and test the result.
