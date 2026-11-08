# Understand an Unfamiliar Spring Boot Project with Claude Code in IntelliJ IDEA

Explore a real Spring Boot application with Claude Code in IntelliJ IDEA. Rather than immediately
changing code, we investigate the application, document its architecture, verify initial findings,
run its tests and trace an existing feature from the browser to persistence.

**Demo application:** [Spring PetClinic](https://github.com/spring-projects/spring-petclinic) (the
lesson uses the project version visible in the recording; upstream may change).
**Tools demonstrated:** IntelliJ IDEA, Claude Code, Java, Spring Boot, installed Apache Maven, H2;
Docker Desktop for MySQL/PostgreSQL integration tests.

**Video:** Link will be added after publication.

## Contents

- [Prerequisites](#prerequisites)
- [Open Spring PetClinic in IntelliJ IDEA](#open-spring-petclinic-in-intellij-idea)
- [Five investigation prompts](#five-investigation-prompts)
  - [Prompt 1 — Understand the application](#prompt-1--understand-the-application)
  - [Prompt 2 — Map the architecture](#prompt-2--map-the-architecture)
  - [Prompt 3 — Development and testing guide](#prompt-3--development-and-testing-guide)
  - [Prompt 4 — Verify and consolidate](#prompt-4--verify-and-consolidate)
  - [Prompt 5A — Environment preflight](#prompt-5a--environment-preflight)
  - [Prompt 5B — Build, run and trace a feature](#prompt-5b--build-run-and-trace-a-feature)
- [Generated architecture analysis](#generated-architecture-analysis)
- [What the recorded execution showed](#what-the-recorded-execution-showed)
- [Run and explore the application](#run-and-explore-the-application)
- [Key lessons and limits](#key-lessons-and-limits)

## Prerequisites

- IntelliJ IDEA with Claude Code configured and accessible from the project directory.
- JDK 17 or newer (the recorded workflow used JDK 25.0.3).
- Installed Apache Maven on PATH (`mvn -version`); the workflow does **not** use Maven Wrapper or
  `java -jar`.
- Docker Desktop **only if** running the MySQL/PostgreSQL integration tests or external database
  profiles. The normal H2 application run does not need Docker.
- Network access may be required to download Maven dependencies and Docker images.

## Open Spring PetClinic in IntelliJ IDEA

1. Clone [spring-projects/spring-petclinic](https://github.com/spring-projects/spring-petclinic) and
   open the project root (containing `pom.xml`) in IntelliJ IDEA. The repository may evolve: the
   exact build/runtime findings below correspond to the lesson recording.
2. Check the project SDK and verify Maven from the project terminal with `mvn -version`.
3. Open Claude Code from the same project root, using your installed launcher (for example,
   `claude`).
4. For a fresh reproduction, keep any previous `docs/application-architecture-analysis.md` outside
   the project before beginning.
5. Submit the prompts below **in order**. Review the generated Markdown after each prompt. Prompts
   1–4 deliberately do not build or run the application.

## Five investigation prompts

The blocks below reproduce the finalized recording prompts **verbatim**. Prompt 5 is separated into
**5A (preflight)** and **5B (execution after human approval)**. Follow Claude’s permission prompts
and review operations before approving them.

### Prompt 1 — Understand the application

Creates the initial six-section architecture document and populates only its overview.

```text
I'm exploring this application for the first time.

NO SPECULATION: Use verifiable evidence; distinguish confirmed source facts,
framework-dependent inferences and unknowns. Never invent functionality or results.

Investigate the application's purpose and likely users, main business capabilities,
primary technologies/versions and entry points.

Create docs/application-architecture-analysis.md as a concise developer onboarding
reference. Start with a clickable Contents index linking to the numbered top-level
sections using standard Markdown heading anchors. Create these sections:

1. Application Overview
2. Architecture
3. Development and Testing Guide
4. Architecture Verification
5. Feature Trace and Practical Validation
6. Open Questions

Populate ONLY Section 1; keep the remaining sections as short placeholders. Include
source-file references for important findings. Record unresolved questions briefly
under Section 6, without duplicating them elsewhere. Only claim to have opened,
inspected or verified a file if you actually did so.

Use source code, build files, configuration and tests only—not README files, existing
architecture documentation or external websites. Do not use Git, run builds/tests or
modify application files. Only create/update the analysis Markdown.

Keep the output concise, focused on information a new developer needs, and avoid
exhaustive inventories. Provide a short completion summary.
```

### Prompt 2 — Map the architecture

Explores components, repository calls, domain relationships, design risks, and one Mermaid diagram.

```text
Continue investigating the codebase and update
docs/application-architecture-analysis.md.

NO SPECULATION: Validate relationships against source. Distinguish direct code
references from framework-managed behavior and unresolved questions. Do not invent
layers, components or calls.

Populate Section 2 — Architecture. Explain the overall style, principal
packages/components and their responsibilities, request/data flow, domain
relationships and persistence, startup/wiring, and 3–5 significant design
characteristics or risks. Use one compact component table with source paths. Focus on
developer understanding, not exhaustive class-by-class coverage.

Produce ONE readable Mermaid architecture diagram (roughly 8–12 meaningful nodes if
clarity allows; do not force a node count). Use simple node IDs and quoted text
labels; avoid HTML tags, unquoted special characters, complicated shape syntax and
chained multi-node arrows. Distinguish direct source-code dependencies from
framework-managed interactions using a clear legend. Check the syntax carefully. Do
not claim the diagram was rendered unless you actually previewed or parsed it
successfully, and record that status clearly.

Update Section 2 IN PLACE. Keep one authoritative architecture explanation and
diagram; do not create a second deep-dive subsection or duplicate earlier summaries.
Add unresolved items only to Section 6. Keep the Contents links synchronized with the
numbered top-level headings.

Use source code, build files, configuration and tests only; exclude README files,
architecture docs and websites. Do not use Git, execute builds/tests or modify
application files. Provide a short completion summary.
```

### Prompt 3 — Development and testing guide

Documents Maven commands, environment prerequisites and test categories without running them.

```text
Continue investigating the codebase and update
docs/application-architecture-analysis.md.

NO SPECULATION: Ground setup, dependency and test conclusions in files. Distinguish
source evidence, framework defaults and unknown execution outcomes.

Populate Section 3 — Development and Testing Guide, optimized for someone onboarding
to this project:
- Minimum prerequisites and the simplest local development configuration.
- Relevant environments/profiles and their database or external-service needs.
- The primary build, run and test commands using the installed Maven CLI only (`mvn`),
  including `mvn -B verify`, `mvn test`, and `mvn spring-boot:run`; cite build-file or
  plugin evidence. If the repository also supports Gradle, describe it briefly but do
  not propose Gradle commands for this video. Do not recommend Maven Wrapper scripts
  (`mvnw`, `mvnw.cmd`) or `java -jar`.
- Major runtime dependencies by role, not an exhaustive inventory.
- Main test categories, representative classes, infrastructure and skip conditions.
- Only notable build/configuration differences or risks that materially affect the
  developer workflow.

Use concise tables only when they improve scanning. Do not duplicate the architecture
section, or list every plugin and test. Do not execute any builds, tests or
application processes; mark commands UNEXECUTED. Include `mvn -version` as a
prerequisite check for the later preflight, not as a command already executed. Record
unknowns once in Section 6. Keep the Contents links synchronized.

Use source code, build files, configuration and tests only; exclude README files,
architecture documentation and websites. Do not use Git or modify application files.
Update Section 3 IN PLACE, not as an appended 'deep dive'. Briefly summarize what you
added.
```

### Prompt 4 — Verify and consolidate

Independently rechecks the findings and corrects the existing document in place.

```text
Independently verify and consolidate docs/application-architecture-analysis.md against
the actual application source and configuration.

NO SPECULATION: Do not assume previous findings are correct. Re-check important
claims: purpose, capabilities, entry points, package/component relationships,
domain/persistence, configuration, build/test structure, and the Mermaid diagram.
Specifically compare database-specific schema constraints and missing-entity paths if
the source makes these relevant; do not assume a discrepancy exists. Distinguish code
evidence, framework-dependent behavior and unknowns.

Correct, merge, replace or REMOVE inaccurate, duplicated, superseded and unsupported
statements IN PLACE across Sections 1–3. Do not preserve erroneous earlier statements
or create chronological 'pass 2 / pass 3' appendices. Keep useful source-file
references.

Populate Section 4 — Architecture Verification with a SHORT summary of what was
checked, up to 3–5 consequential corrections/risks if present, and evidence. Do not
repeat the full architecture or test tables. Maintain one prioritized list of
unresolved questions in Section 6.

Refine the existing SINGLE Mermaid diagram in Section 2; do not append another. Use
simple quoted labels, no HTML, parser-safe syntax, and a clear distinction between
direct dependencies and framework behavior. Check syntax conservatively; do not claim
a preview/render check unless performed.

Keep numbered top-level headings and their clickable Contents links in sync. Keep
Section 5 as a placeholder until hands-on validation.

Use source code, build files, configuration and tests only, not README files,
architecture docs or websites. Do not use Git, run builds/tests or change application
files. Provide a brief verification summary and name any corrected claims.
```

### Prompt 5A — Environment preflight

Checks Maven, Java, Docker readiness, dependencies and test scope; **must stop** before execution.

```text
Before running any builds, tests, servers or HTTP requests, perform a READ-ONLY
ENVIRONMENT PREFLIGHT for docs/application-architecture-analysis.md and the Spring
PetClinic repository.

Use the installed Apache Maven CLI (`mvn`) exclusively for this workflow. Never invoke
mvnw, mvnw.cmd, java -jar, or Gradle for build/run tasks. For the application, the
required startup command is `mvn spring-boot:run`.

Check, without launching the application or any services:
- Current working directory and whether it contains pom.xml.
- Java availability/version and `mvn -version`, including Maven's effective Java
  runtime.
- Whether port 8080 is free and whether an application process is already running.
- The project configuration and test source to identify which test groups need Docker
  or other external dependencies, and whether these tests may be skipped or not
  discovered when dependencies are missing.
- Docker CLI and daemon availability (`docker info` or equivalent non-destructive
  check), not merely whether Docker is installed; identify Docker Compose availability
  if applicable.
- The simplest default H2 run versus MySQL/PostgreSQL integration-test prerequisites.
- Any permissions, network, process-control limitations or human-operated setup that
  could block the intended validation. Check readiness only; do not install software
  or start services.

Return a short PRE-FLIGHT REPORT with columns: Prerequisite | Observed status |
Required action | Needed for. Explicitly distinguish mandatory from optional
requirements and label anything unverified. State whether the planned scope includes
Docker-dependent MySQL/PostgreSQL tests or only default H2 validation; if it has not
been decided, ASK ME to choose before the build. Do not silently accept skipped
database tests as a complete run.

If an action only I can perform is required (for example starting Docker Desktop,
logging in, choosing a database profile, freeing a port or installing/configuring
Maven), explain the exact action and how to confirm readiness. A running Docker daemon
does not by itself guarantee database tests will pass. Do not claim you performed it.
Do not silently downgrade the test scope or skip infrastructure-dependent tests.

IMPORTANT: STOP after the preflight report. Do not build, run tests, start Spring
Boot, start Docker, make HTTP requests, modify source files or edit the Markdown
document. Wait for my explicit instruction before proceeding.
```

### Prompt 5B — Build, run and trace a feature

Runs verification after explicit approval; records test evidence and traces the Add Pet feature.

```text
Continue the same Prompt 5 task. I have reviewed the preflight report and confirmed
the agreed test scope and required human actions. Recheck any changed prerequisite
before running tests. If the test scope is still ambiguous, STOP and ask me to choose.

Use ONLY installed Maven CLI commands. Run `mvn -B verify` and record actual build
status, the executed/passed/failed/skipped test counts, and database-specific test
outcomes separately. Do not use Maven Wrapper scripts, Gradle or `java -jar`. If a
prerequisite for the AGREED scope is still unavailable, STOP and ask me rather than
proceeding with predictable skips or silently reducing scope. If I explicitly approved
a narrower H2-only scope, report Docker-based checks as NOT RUN, not PASSED.

If the build is ready and succeeds, start the local default H2 application using `mvn
spring-boot:run` in a controlled terminal/process. Wait for a confirmed startup and
port. Verify a few representative endpoints with local HTTP requests, and stop only
the process you started when appropriate. If a background application process cannot
be managed reliably, STOP and request a manual terminal action; do not claim the
server stayed running. Never kill unrelated processes.

Choose ONE representative create/update business operation (prefer Add New Pet if the
source/sample data support it). Trace URL/UI -> controller/model binding -> validation
-> repository/persistence -> response with exact project source references. Where
safe, confirm success and one invalid input with HTTP requests. Note that successful
writes change the running H2 state; restart with a fresh in-memory database before
demonstrating the same sample record in the browser. Distinguish mocked tests,
real-server HTTP checks and manual browser observations.

Update `docs/application-architecture-analysis.md` IN PLACE, maintaining one concise
authoritative reference:
- Section 3: make installed Maven commands (`mvn -B verify`, `mvn spring-boot:run`,
  etc.) the primary commands. Keep observed versus unexecuted statuses accurate.
- Section 5: actual execution results, a roughly five-stage feature trace, and one
  practical Manual Browser Validation Runbook. Use `mvn spring-boot:run` as the
  browser demo startup command; state whether Docker is needed for the chosen runtime
  profile. Give URL, navigation, seeded example, valid/invalid inputs, expectations,
  cleanup and a short Action | Expected | Actual | Status checklist. All browser steps
  remain PENDING until manually tested.
- Section 6: retain only genuinely unresolved questions. Update prior claims in place
  when results establish the answer.

Do not fabricate passing tests or browser observations. If a test is skipped, aborted
or not discovered—even with Docker available—record exactly that and inspect the
available test reports/logs before assigning a cause. Do not conflate tests run,
passed, skipped, aborted, or zero discovered. Keep a single Mermaid diagram and
working Contents anchors. No source/test/config edits, no direct Git commands, no
duplicate results tables and no lengthy logs in the Markdown. Return a concise summary
of actual outcomes and pending manual checks.
```

## Generated architecture analysis

Claude builds and updates `docs/application-architecture-analysis.md` in the Spring PetClinic
project. The six main sections are:

1. Application Overview
2. Architecture
3. Development and Testing Guide
4. Architecture Verification
5. Feature Trace and Practical Validation
6. Open Questions

This file is a **generated output**, not a file to paste into Claude as input. Keep source-backed
observations, framework inferences and unresolved questions distinct. The architecture diagram in
the recorded run was checked by eye, **not rendered/parsed**, and should not be presented as
validated Mermaid output.

## What the recorded execution showed

| Check | Recorded result |
|---|---|
| Maven `mvn -B verify`, first run | **Failed:** 81 tests run, 2 PostgreSQL integration-test errors, 0 skipped |
| PostgreSQL blocker | PostgreSQL rejected the JVM time-zone alias `Asia/Calcutta` |
| Maven `mvn -B verify`, second run | **Passed with a process-only workaround:** 81 run, 0 failures, 0 errors, 0 skipped |
| Workaround | For that Maven process, `JAVA_TOOL_OPTIONS=-Duser.timezone=Asia/Kolkata` |
| Docker-backed integration tests | MySQL: 2 passed; PostgreSQL: 2 passed on the second run |
| Default H2 application | Started with `mvn spring-boot:run` on port 8080 |
| Real-server Add Pet HTTP checks | Valid Rex and Bella creates observed; invalid inputs produced field errors without saving |
| Missing-record endpoints | The tested unknown owner/pet paths returned HTTP 500, not 404 |
| Browser walkthrough | Welcome page, owner pagination/search, Add New Pet form, blank-name validation were visually observed |

**Important:** The passing build required an environment workaround; the first failure should not be
concealed. The workflow also called for stopping on unexpected failures before unapproved retries;
in the recorded run Claude performed the retry itself. Do not present that as the preferred approval
practice. These are recorded results, not guarantees for a fresh clone.

## Run and explore the application

From the Spring PetClinic root, run:

```powershell
mvn spring-boot:run
```

Open `http://localhost:8080` and use **Find owners**. In the recorded seed data, searching for
`Franklin` opened George Franklin with his pet Leo. Select **Add New Pet** to inspect the Name,
Birth Date and Type fields. The blank-name case showed `is required` in the browser. The other
invalid-input scenarios were checked over HTTP, not all repeated in the browser.

**Five-stage Add Pet trace:** form and route → `PetController` and model binding → `PetValidator`
and business rules → `OwnerRepository.saveAndFlush(owner)` and JPA cascade → redirect to owner
details.

Stop Maven using **Ctrl+C**. The default in-memory H2 database is reinitialized on restart, so
example pets added during a previous run are not preserved.

## Key lessons and limits

- PetClinic is a feature-organized Spring MVC application where controllers call repositories
  directly; it does not introduce a distinct service layer. Repository operations may still have
  their own transactions.
- The initial architecture explanation was corrected after checking the source, especially
  missing-pet flows and differing database constraint definitions.
- The MySQL duplicate-name error handling under concurrency remains a **potential** issue, not a
  demonstrated failure.
- Not every database-specific behavior, cache manager detail or diagram-rendering question was
  verified. Refer to the generated analysis document for the exact evidence and unresolved items.
