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
  - [Refine the Plan](#refine-the-plan)
  - [Add Division and Run Tests](#add-division-and-run-tests)
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

These prompts match the latest recording script, in demonstration order. The three IntelliJ integration features are **IDE Diff Viewing**, **Selection Context**, and **Diagnostic Sharing**. Review Claude’s changes and requested permissions before accepting them.

### Inspect the Maven Project

Inspect the Maven project configuration and active JDK. No files should be changed.

```text
Inspect this Maven project: identify the configured
Java version, where it's set, and the installed and
active JDK. Do not modify files.
```

### Create the Calculator Class

Use Manual mode and review the new Java file in IntelliJ’s Diff Viewer.

```text
Create Calculator in package com.example.calculator
with public int add(int a, int b) that returns the sum.
```

### Add Multiplication and Compile

Use Accept Edits and inspect the compilation results.

```text
Add public int multiply(int a, int b) to Calculator
that returns the product. Run `mvn compile`.
```

### Review Selected Method — Selection Context

Select the `multiply()` method in IntelliJ before submitting. Claude receives the selection context automatically.

```text
Review the selected IntelliJ method for important
edge cases or limitations. Do not modify code.
```

### Plan Subtraction and JUnit Tests

Use Plan mode. Review the proposed implementation before approving any changes.

```text
Plan adding subtraction and JUnit Jupiter tests for
addition, multiplication, and subtraction.

Cover normal values, negatives, and zero where relevant.
Avoid redundant tests and unnecessary setup.
Inspect the project and propose a plan only; don't implement.
```

### Refine the Plan

When the proposed plan goes beyond the requested scope, choose **Tell Claude what to change**, submit this feedback, and review the revised plan before approval.

```text
Revise the plan: keep all existing methods unchanged.
Add subtraction and focused JUnit Jupiter tests for
all Calculator methods, including subtraction.

Cover normal values, negatives, and zero where relevant.
Exclude overflow handling, overflow tests/checks,
and unrelated changes. Include only necessary Maven
test configuration.

Update the plan only; do not implement.
```

### Add Division and Run Tests

Use Auto mode. Review the method, tests, and Maven output afterward.

```text
Add a divide(int a, int b) method to Calculator
using Java integer division.

Add focused JUnit tests for division,
including division by zero.

Run mvn test. Only modify Calculator.java
and CalculatorTest.java.
```

### Inspect IntelliJ Diagnostics

Temporarily add `missingMethod();` **in the class body**, after `divide()` and before the class’s final `}`. Open IntelliJ’s **Problems** tool window and inspect the errors before asking Claude to retrieve them. Use Manual mode.

```text
Retrieve IntelliJ's diagnostics for Calculator.java
and explain any errors or warnings. Do not edit files.
```

After Claude replies, press **Ctrl+O** to inspect the tool activity. Verify an **`ide - getDiagnostics (MCP)`** call and compare its messages with IntelliJ’s **Problems** window. Remove `missingMethod();` when finished.
