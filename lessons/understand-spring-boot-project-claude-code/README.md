# Understand an Unfamiliar Spring Boot Project with Claude Code in IntelliJ IDEA

Explore an unfamiliar codebase through three questions: **What does the application do and where does it start? How do the components interact? How does a real feature work end to end?**

This lesson uses [Spring PetClinic](https://github.com/spring-projects/spring-petclinic), but the prompts are reusable for other projects. Claude Code investigates and documents the code; **you** build, test, run, and validate it.

**Tools:** IntelliJ IDEA, Claude Code, Java, installed Apache Maven, and Docker Desktop when required by integration tests.

**Video:** Link will be added after publication.

## Contents

- [Getting started](#getting-started)
- [Prompt 1 — Discover the application](#prompt-1--discover-the-application)
- [Prompt 2 — Understand the architecture](#prompt-2--understand-the-architecture)
- [Prompt 3 — Trace a real feature](#prompt-3--trace-a-real-feature)
- [Run and verify it yourself](#run-and-verify-it-yourself)
- [Generated architecture document](#generated-architecture-document)

## Getting started

1. Clone [Spring PetClinic](https://github.com/spring-projects/spring-petclinic) and open its root directory in IntelliJ IDEA.
2. Open Claude Code from that project directory. For a fresh run, move aside any existing `docs/codebase-analysis.md`.
3. Submit the following three prompts **in order, in the same session**. Review the findings and updated Markdown after each prompt.

The prompts do not authorize Claude to build, test, or start the application or change its source code.

## Prompt 1 — Discover the application

**What does this application do, and where does the code start?**

```text
I'm new to this project.

What does the application do, what technologies does it use, and where does the code start?

Explain its main features, startup process, and entry points, including how incoming
requests or events are handled.

Save the findings in docs/codebase-analysis.md with a clickable table of contents and
clickable source links to each file and line. Start it with only these sections: Purpose
and main features, Technology stack, Startup process, Entry points and request handling,
Open questions. Don't cover the data model, architecture, data flow, or build and run
steps yet, and add no placeholders for them. Open questions lists only what the evidence
couldn't confirm.
Keep this document updated throughout our investigation.

Use verifiable project evidence; don't invent details.
Use installed Maven (mvn) in any commands, not the Maven Wrapper (mvnw).
Don't run the application or modify application code.
```

## Prompt 2 — Understand the architecture

**How do the pieces talk to each other?**

```text
How do the main components of this application interact?

Explain its architecture, component dependencies, and data flow, including how data is
processed, validated, and stored where applicable.

Highlight important design decisions and evidence-based concerns.
Include one clear, high-level Mermaid architecture diagram with valid syntax.
Keep it to about 12 nodes. Put every node label in double quotes, and avoid
semicolons in labels.

Update docs/codebase-analysis.md with clickable source references. Add only these
sections, placed before Open questions: Architecture, Data flow, Design decisions, Concerns.
Don't cover a feature trace or build and run steps yet. Leave existing sections unchanged
unless a finding corrects them, and update Open questions as needed.
Don't invent components, layers, dependencies, or behavior.
Don't run or modify the application.
```

## Prompt 3 — Trace a real feature

**How does Add New Pet work end to end?** This recording specifies the feature to make the demonstration consistent. For another repository, replace the feature name on the first line.

```text
Trace the "Add New Pet" feature end to end.

Follow its entry point through the relevant components, processing, validation, data
operations, and final outcome. Show the key classes, functions, and clickable source references,
and include one clear Mermaid sequence diagram with valid syntax.
Avoid semicolons in message text.

Provide a practical, step-by-step guide to build, test, run, and verify the application
myself.

Identify prerequisites, required tools and versions, configuration, and external
services. Include exact commands, expected outcomes, and troubleshooting guidance.

For Maven projects, use installed Maven (mvn) with standard commands, not the Maven
Wrapper (mvnw) or batch mode (-B). Adapt to other build tools when applicable.

Include the complete test suite, relevant quality checks, application startup, and
successful and invalid workflow scenarios. Clearly identify any tests needing
external services.

Update docs/codebase-analysis.md with the feature trace and a concise execution
and manual verification runbook. Add only these sections, placed before Open questions:
Feature trace, Runbook. Leave existing sections unchanged unless a finding corrects them,
and update Open questions as needed.
Mark all execution results as pending until verified.

Use verifiable evidence; never invent results.
Do not run commands that build, test, or start the application.
Do not modify application code.
```

## Run and verify it yourself

Follow the **project-specific runbook Claude generates** in `docs/codebase-analysis.md`. For the PetClinic version used in this lesson, a basic terminal sequence is:

```powershell
java -version
mvn -version
docker info
mvn clean verify
mvn spring-boot:run
```

Docker is needed for database-dependent integration tests, but the default in-memory H2 application does not require it. Make sure the Docker engine is running before expecting those tests to execute. Review the Maven output and test reports for failures and skipped tests; do not mistake a partial run for complete verification.

Once the application starts, open `http://localhost:8080/`, find an existing owner, and select **Add New Pet**. Check one valid submission and one invalid submission (for example, a blank name), then confirm the resulting page. Stop the server with **Ctrl+C**.

The prompts mark every execution result **pending** until you carry out these checks. Upstream PetClinic versions, seed data, prerequisites and test behavior may differ.

## Generated architecture document

Claude creates and incrementally updates `docs/codebase-analysis.md` as the **output** of the three prompts. It should contain the overview and entry points, source-backed architecture, a Mermaid architecture diagram, an end-to-end feature trace with a sequence or flow diagram, and the manual execution runbook.

Source-based explanations and inferred framework behavior are not the same as observed runtime results. Preview diagrams before relying on them, and record actual test and browser outcomes accurately.
