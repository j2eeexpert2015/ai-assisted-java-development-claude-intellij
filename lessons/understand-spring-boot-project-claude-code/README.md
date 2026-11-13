# Understand an Unfamiliar Spring Boot Project with Claude Code in IntelliJ IDEA

Explore an unfamiliar codebase through three questions: **What does the application do and where does it start? How do the components interact? How does a real feature work end to end?**

This lesson uses [a fork of Spring PetClinic](https://github.com/j2eeexpert2015/spring-petclinic), so your code matches the recording, but the prompts are reusable for other projects. Claude Code investigates and documents the code; **you** build, test, run, and validate it.

**Tools:** IntelliJ IDEA, Claude Code, Java, installed Apache Maven, and Docker Desktop when required by integration tests.

**Video:** Link will be added after publication.

## Contents

- [Getting Started](#getting-started)
- [Prompt 1 — Discover the Application](#prompt-1--discover-the-application)
- [Prompt 2 — Understand the Architecture](#prompt-2--understand-the-architecture)
- [Prompt 3 — Trace and Verify a Real Feature](#prompt-3--trace-and-verify-a-real-feature)

## Getting Started

1. Clone the [Spring PetClinic fork](https://github.com/j2eeexpert2015/spring-petclinic) (`https://github.com/j2eeexpert2015/spring-petclinic.git`) with Git rather than downloading a ZIP, and open its root directory in IntelliJ IDEA.
2. Open Claude Code from that project directory. For a fresh run, move aside any existing `docs/codebase-analysis.md`.
3. Submit the following three prompts **in order, in the same session**. Review the findings and updated Markdown after each prompt.

The three prompts progressively create one document, `docs/codebase-analysis.md`. Claude Code does **not** build, test, start, or modify the application. You execute and verify the Runbook yourself.

## Prompt 1 — Discover the Application

**What does this application do, and where does the code start?**

```text
I'm new to this project. Investigate it using source code and configuration.

Purpose and Main Features — Briefly summarize what the application does.
List only its main features with clickable source links; leave implementation
details to the relevant technical sections.

Technology Stack — Group the main technologies by role, with versions and
source evidence. List supporting tools separately.

Startup Process — Explain how the application starts, including its entry
point, configuration, and important initialization steps.

Entry Points and Request Handling — Identify how incoming requests or events
enter the application and how they are handled.

Create docs/codebase-analysis.md with a clickable table of contents,
clickable source links to relevant files and lines, and only these four sections
in the order listed. Use Title Case for all section and subsection headings.
Do not add placeholders for later sections.

Base findings on source and configuration. Treat the README as claims to
verify, not as evidence. Do not invent details; state uncertainty beside
the relevant finding. Do not build, test, start, or modify the application.
```

## Prompt 2 — Understand the Architecture

**How do the main components interact?**

```text
Continue the investigation and update docs/codebase-analysis.md.

Architecture — Explain the main components, their responsibilities, and
dependencies.

Include one readable Mermaid architecture diagram. Show each main entry point
separately, connected to the components it interacts with; omit framework
internals. Label arrows briefly and distinguish direct dependencies from
framework-managed interactions. Include only evidence-backed relationships.

Under the diagram, trace one representative request in a few numbered steps.
Put node and arrow labels in double quotes, and avoid semicolons in labels.

Add only the Architecture section after the existing sections. Update the
clickable table of contents and include clickable source links to relevant
files and lines. Use Title Case for headings. Leave earlier sections unchanged
unless evidence requires a correction. Do not add later sections or placeholders.

Treat the README as claims to verify, not as evidence. Do not invent
components or behavior. Do not build, test, start, or modify the application.
```

## Prompt 3 — Trace and Verify a Real Feature

**How does Add New Pet work end to end?** This recording specifies the feature to keep the demonstration consistent. For another repository, replace the feature name with one that exists in that codebase.

```text
Continue the investigation and update docs/codebase-analysis.md.

Feature Trace — Trace the "Add New Pet" feature from initiation to final
outcome. Explain the actual execution order, participating components,
important decisions, and alternative outcomes supported by the code.

Present the trace as one numbered table showing each step, what happens,
the relevant classes or functions, and clickable source links. Keep the
main execution path clear and avoid unnecessary details.

Include one simple Mermaid flowchart after the table showing the feature's
main steps, decisions, and outcomes. Do not include a sequence diagram.

Runbook — Provide one dependency-ordered guide covering environment checks,
required services, complete build and tests, result verification, application
startup, and manual feature verification.

Inspect build and test configuration to identify prerequisites, tools and
versions, profiles, quality checks, test categories, and external dependencies.
Provide exact commands, expected outcomes, troubleshooting, and corrective
rerun steps. Distinguish required steps from optional alternatives and avoid
redundant commands.

Determine whether services such as Docker must be started manually or are
managed by tests. Include readiness checks before dependent steps.

Explain how to verify that all applicable tests executed, including integration
tests. Identify failures, errors, and unexpected skips. Treat predicted test
counts as estimates until confirmed by actual execution.

Include a browser-based walkthrough covering successful and invalid scenarios.
Keep relevant test coverage in the Runbook rather than the Feature Trace.

For Maven projects, use installed Maven (mvn), not the Maven Wrapper (mvnw)
or batch mode (-B). Adapt to other build tools where applicable.

Add only Feature Trace and Runbook, in that order, after Architecture. Update
the clickable table of contents and include clickable source links to relevant
files and lines. Use Title Case for all headings. Leave previous sections
unchanged unless evidence requires a correction.

Do not add separate Data Flow, Design Decisions, Concerns, or Open Questions
sections. Keep the document concise and avoid repetition. Treat the README
as claims to verify, not as evidence. Do not invent behavior or results;
state uncertainty beside relevant findings. Mark all execution results PENDING.

Do not build, test, start, or modify the application. I will perform all
execution and manual verification myself.
```

The final document contains seven sections, in order: **Purpose and Main Features**, **Technology Stack**, **Startup Process**, **Entry Points and Request Handling**, **Architecture**, **Feature Trace**, and **Runbook**.

The architecture diagram shows component relationships; the feature flowchart summarizes the selected feature's main steps and outcomes. The Runbook explains how to verify the findings manually. Preview both diagrams, check that source links resolve, and record real test and browser results accurately. **Source-based expectations are not observed runtime results.**
