# Build a Full-Stack Spring Boot + Angular App with Claude Code in IntelliJ IDEA

Follow the **eShop** lesson using three sequential Claude Code prompts: **requirements**, **Spring Boot backend**, and **Angular frontend**. The application supports a product catalog, shopping cart and checkout with order confirmation, using six sample products across three categories. Accounts, payments and order tracking are outside the lesson scope.

**Technology:** Java 25, Spring Boot 4 (retain the version selected when creating the starter), system Maven, MySQL 8.4 with Docker Compose, Flyway, Swagger/OpenAPI, Angular, TypeScript and component-scoped SCSS.

**Three complete, copy-ready prompts are included below**, following the same single-README format as the EcomStore React lesson. Submit each prompt in order, and review the generated files before moving to the next.

## Contents

- [Prerequisites](#prerequisites)
- [Prepare the project in IntelliJ IDEA](#prepare-the-project-in-intellij-idea)
- [Three development prompts](#three-development-prompts)
  - [Prompt 1 — Functional requirements](#prompt-1--functional-requirements)
  - [Prompt 2 — Spring Boot backend](#prompt-2--spring-boot-backend)
  - [Prompt 3 — Angular frontend](#prompt-3--angular-frontend)
- [Review the generated outputs](#review-the-generated-outputs)
- [Run and verify eShop](#run-and-verify-eshop)
- [Recording and data notes](#recording-and-data-notes)

## Prerequisites

Install IntelliJ IDEA, **JDK 25**, **system Maven**, **Node.js and npm**, Docker Desktop with Compose, and Claude Code CLI. Check your installations in the project terminal:

```text
java -version
mvn -version
node -v
npm -v
docker compose version
claude --version
```

Use JDK 25 for both IntelliJ and Maven. If your terminal defaults to another JDK, correct `JAVA_HOME` before building. The local application uses ports **3306** (MySQL), **8080** (Spring Boot) and **4200** (Angular).

## Prepare the project in IntelliJ IDEA

1. Select **File → New → Project → Empty Project** and name it `eshop`.
2. Set **Project SDK** to JDK 25.
3. Within that project, select **File → New → Module → Spring Boot**. Create `backend/` with Java 25, Maven, JAR packaging, group `org.learningfromexperience`, artifact `eshop` and base package `org.learningfromexperience.eshop`. Select **Spring Web** as the initial dependency, retaining the Spring Boot version chosen in the wizard.
4. Confirm the project contains `backend/pom.xml`. Claude will **extend this existing module**, not create a replacement Spring Boot project.
5. Start Claude Code from the **`eshop/` project root**. Before Prompt 1, review the actual **Sonnet model and effort setting** shown in your installed Claude Code version, and select the intended setting for the demonstration.
6. Submit the three prompts **one at a time**, reviewing the results and generated files between prompts.

## Three development prompts

The three complete prompt blocks below are taken from the finalized **eShop (Spring Boot + Angular) — Three Claude Code Prompts** document. Copy the **entire fenced prompt** and submit it to Claude Code from the `eshop/` project root. Each stage depends on the documents and code created in the preceding stage.

### Prompt 1 — Functional requirements

Create `docs/requirements.md` and the initial application `README.md`; do not generate application code.

```text
Create docs/requirements.md as a concise, implementation-independent FUNCTIONAL specification for eShop. Group separately numbered, stable (never reused) CAT-001..., CART-001..., CHK-001... requirements under Catalog, Cart and Checkout. Each requirement must use "The system SHALL..." and have one or more "WHEN...THEN..." acceptance scenarios. Then include Sample data, Visual expectations and Out of scope. Do not add functionality.

CATALOG
- One catalog: product name, description, category, price, stock; no product-detail page.
- Case-insensitive, trimmed substring search by name; optional category filtering; both filters combine.
- Add products directly to cart; cannot newly add products currently showing zero stock.

CART
- Add increments quantity by 1; change quantities to positive integers; remove lines; calculate line subtotals and overall total; header shows total units.
- Reject zero/negative/fractional/non-numeric quantities without changing the previous valid value; removal is a distinct action.
- Persist cart across refresh; no user account.
- Cart quantities may exceed displayed stock; check latest stock only at checkout. Keep cart lines if an item subsequently becomes unavailable; never silently adjust/remove quantities.

CHECKOUT / CONFIRMATION
- Require full name (nonblank), valid email, nonblank shipping address and >=1 item; reject invalid quantities and show field-specific errors.
- Validate latest stock when submitting; report all shortages with item, requested and available quantities. Failure must cause NO partial order or stock change, and preserve cart/customer entry.
- Use authoritative server prices, not browser-submitted prices; calculate totals accurately.
- On success persist order/items, reduce stock, show unique order number, items/quantities/subtotals/total and date/time; clear cart only on confirmed success.

SAMPLE DATA — EXACT THREE CATEGORIES AND SIX PRODUCTS (category | name | USD price | stock | exact description)
Electronics | Wireless Headphones | 129.99 | 10 | Comfortable over-ear headphones for everyday listening.
Electronics | Mechanical Keyboard | 89.50 | 15 | Compact mechanical keyboard with tactile switches.
Books | Clean Code | 34.95 | 20 | A practical book about writing maintainable software.
Books | Spring Boot in Practice | 44.00 | 10 | Hands-on guidance for building Spring Boot applications.
Home & Kitchen | Espresso Machine | 249.99 | 3 | Countertop espresso maker for home coffee brewing.
Home & Kitchen | Water Bottle | 24.99 | 25 | Reusable insulated bottle for daily use.

VISUAL EXPECTATIONS
Responsive layout; navy header, prominent search, category pills below header, amber Add to Cart buttons/cart count, light-gray background, white rounded cards, dark footer; clear loading/empty/error states. Stock badge: green "In stock" when >5, amber "Only N left" when 1–5, red "Out of stock" when 0 (disable Add to Cart). Format USD to two decimals.

OUT OF SCOPE
Payments, accounts, product details/images, pagination/sorting, order tracking/lookup, cancellation/refunds, admin, coupons and emails.

GUARDRAIL: docs/requirements.md describes WHAT, not HOW. No framework, architecture, API routes, HTTP codes, DTOs/entities, database tables or test filenames.

Also create a short root README.md with eShop's goal, a link to requirements.md and Backend/Frontend statuses both "Not implemented". Write documentation only—no application or database changes. Report created files and any assumptions/interpretations needing review.
```

**After Prompt 1:** Inspect requirement IDs, `SHALL`/`WHEN…THEN` scenarios, the six sample products, assumptions, and the initial README status. This stage should produce documentation only.

### Prompt 2 — Spring Boot backend

Read the requirements and current README, document the backend design, implement the existing Spring Boot module and verify the API.

```text
OBJECTIVE
Read docs/requirements.md and README.md. Design, implement and verify ONLY the eShop backend for Catalog, Cart and Checkout, preserving approved scope. No frontend.

STACK AND NON-NEGOTIABLE CONSTRAINTS
- Java 25; use existing backend/pom.xml unchanged in Spring Boot version, Maven coordinates (`org.learningfromexperience:eshop`), base package (`org.learningfromexperience.eshop`) and main class. Stop if missing. System `mvn`, no Maven Wrapper. Keep backend code/tests in backend/.
- Spring MVC, Spring Data JPA, Bean Validation, Java records for API DTOs, BigDecimal for money; no Lombok/MapStruct.
- JUnit 5; if using *IT integration-test filenames, configure Failsafe so `mvn verify` executes them.
- Root compose.yaml: MySQL 8.4 service `mysql`, host port 3306, named volume (e.g., `eshop_mysql_data`) and health check; DB `eshop_db`, existing Docker-created user/password `eshop`/`eshop`. One-time init SQL creates only `eshop_test` and grants access to that existing user (never CREATE USER again). Tests use isolated test DB.
- Spring Boot Flyway starter plus flyway-mysql; versioned migrations create schema and seed EXACT three categories/six products from requirements.
- Compatible springdoc OpenAPI; CORS on /api/** from http://localhost:4200, including preflight (Angular CLI development server).

FIXED PUBLIC REST CONTRACT — /api/v1, exactly three operations; no entities exposed
GET /categories -> [{id,name}]
GET /products?search=&categoryId= -> [{id,name,description,price,stock,categoryId,categoryName}]
POST /orders request -> {customer:{fullName,email},shippingAddress,items:[{productId,quantity}]}
201 response -> {orderNumber,customer,shippingAddress,items:[{productId,productName,unitPrice,quantity,subtotal}],totalAmount,createdAt}
Use Spring ProblemDetail: HTTP 400 identifies invalid fields; HTTP 409 lists ALL shortage items with product name, requested and available quantities. Define and document a consistent machine-readable error extension structure so the Angular UI can display all errors.

BACKEND SAFETY
Calculate prices on the server. Save each order in one transaction: on any failure, save nothing and change no stock. Reduce stock with one UPDATE that succeeds only if enough stock remains. Order number: "ORD-" plus part of a random UUID, no retry logic. Minimal design; no unrelated functionality or altered API.

IMPLEMENT IN THIS ORDER
1. Create docs/backend-design.md: Constraints (versions, database, Maven, CORS, API); real domain model, schema, service design, transactions/stock strategy, request/response examples, error structure.
2. Implement controllers, services, DTOs, persistence, Docker/init SQL and Flyway migrations.
3. Implement tests for catalog/search/filter, valid order and stock reduction, invalid fields, two or more simultaneous product shortages in one order, and full rollback. Every test uses plain-English @DisplayName starting with its requirements ID.
4. Start MySQL; run `mvn verify` in backend/; start Spring Boot; make real HTTP requests to all three endpoints and live /v3/api-docs, including a successful checkout and stock-shortage failure. Capture actual statuses and stock differences. Update backend-design.md for implementation deviations, not public API changes. Document checks not run. Do not silently delete orders or reset stock after live requests.

README RUNBOOK
Update root README.md, retaining its unnumbered project goal and status. Commands must work in PowerShell/CMD, with working directories, expected URLs and expected-versus-actually-observed results clearly separated. Populate ALL sections below with REAL files, identifiers, data and results (never invent evidence):
1. Prerequisites: JDK 25, system Maven, Docker, version checks.
2. Database setup: exact Compose startup/health checks, app/test databases, credentials, Docker fresh-volume init versus Flyway.
3. Backend project structure: actual annotated fenced `text` tree from project root showing key backend source, tests, configs, migrations and Docker files; only existing files, no target/build artifacts. Prompt 3 will expand this ONE tree to include frontend.
4. Database schema & sample data: table of EVERY actual relevant table/column, SQL type, key/constraint and plain-English purpose; relationships/constraints; six seeded products in a second table with actual ID, product, category, price, stock and test/demo purpose; working SQL inspection command.
5. Flyway: every migration filename/version, purpose/order, what seeds data, when migrations apply and flyway_schema_history query; do not edit an applied migration.
6. Backend tests: `mvn verify` command, EVERY executed test class/file and @DisplayName, linked requirement ID, individual PASS/FAIL/SKIPPED plus totals. No synthetic results.
7. Swagger/UI API runbook, SHORT numbered steps: (a) check/start MySQL/health in project root; (b) start backend in backend/ using `mvn spring-boot:run` and a dedicated terminal; (c) Swagger UI and raw /v3/api-docs URLs; (d) how to Try it out/Execute GET categories (3) and products (6), using live product IDs; (e) copyable, correctly shaped POST JSON inline for 2 Wireless Headphones (201; $259.98; fresh 10->8), 4 Espresso Machines (409; requested 4/available 3; unchanged stock), and invalid email (400; customer.email), with expected status and response fields; (f) re-query products, optional working SQL checks of orders/stock, explain changed demo data. Note actual IDs/order numbers/timestamps vary; do not say Swagger was operated if checks used only direct HTTP.
8. Frontend project structure and run — RESERVED for Prompt 3.
9. Frontend UI walkthrough and verification — RESERVED for Prompt 3.
10. Demo reset: stop backend; `docker compose down -v` (WARN: deletes local volume/orders), `docker compose up -d --wait`; restart backend to reapply migrations. Never make destructive reset the routine startup step.

README evidence rule: Report facts only from actual created files, executed tests, DB queries and HTTP responses. Mark PASS/FAIL/NOT RUN accurately; differentiate verification already executed from instructions for viewers. Do not create separate test-report files.

FINAL REPORT
Brief summary of files/design, actual verification results, blockers and backend start commands.
```

**After Prompt 2:** Review `docs/backend-design.md`, then `backend/pom.xml` to see the dependencies added to the original Spring Web starter. Expand the backend packages, migrations and tests in IntelliJ. Verify actual Maven and API results.

### Prompt 3 — Angular frontend

Validate the live backend API contract, document the Angular design, build and test the frontend, and complete the application README.

```text
OBJECTIVE
Read docs/requirements.md, docs/backend-design.md and README.md. Build ONLY the Angular frontend for eShop's three approved features in frontend/. Do not modify backend API, schema or approved functionality.

ANGULAR CONSTRAINTS
- Current stable Angular/Angular CLI compatible with installed Node; TypeScript, standalone components, Angular Router, Angular HttpClient, Angular Reactive Forms and npm; strict checking. Component-scoped SCSS with the responsive navy/amber styling from requirements; use shared SCSS variables/tokens for consistent colors and spacing. No additional UI, styling or state libraries. Keep cart state in one injectable service using signals, persisted to localStorage. Keep components few and small.
- Create the project non-interactively: pass every Angular CLI choice as a flag (routing, SCSS, no SSR, no git), accept defaults for the rest and disable CLI analytics. Prevent interactive prompts. Run build/test/lint commands without watch mode; backend and Angular dev servers may run during integration checks, then stop only processes started for verification.
- Angular dev server at http://localhost:4200; API at http://localhost:8080. Put the API base URL in Angular environment files, not in components.
- Confirm backend CORS accepts http://localhost:4200, including preflight; if not, STOP and report mismatch (do not silently change backend).

CONTRACT-FIRST INTEGRATION
1. From project root check/start MySQL if needed; start backend from backend/ if needed.
2. Fetch live http://localhost:8080/v3/api-docs; compare with docs/backend-design.md and documented request/response/error fields. Stop/report if unreachable or inconsistent. Use actual IDs/shapes, never guess.
3. Create docs/frontend-design.md FIRST: Constraints (actual Angular/CLI/Node versions, ports, API configuration), routes/pages/components, typed API service, reactive forms/validators, cart-state persistence, handling of 400/409, responsive layout. Keep architecture minimal.

BEHAVIOR
4. Catalog: display all six seeded products; name search and category filters (independent/combinable), loading/empty/API-error states, exact stock badges; disable adding when stock=0.
5. Cart: add/remove, positive-integer quantity editing and validation, line subtotals/cart total/header total units; localStorage survives refresh. Do not cap cart quantity by available stock or silently remove lines.
6. Checkout: reactive form for required name, valid email, required address; display field-specific validation messages, including HTTP 400 field errors; POST typed request to existing backend. On HTTP 409 display EVERY shortage from the API (item, requested, available); retain cart and entered details after failure. On success display confirmation from POST response and only then clear cart. Confirmation remains visible until navigation/refresh; it is transient, not a history/lookup page, and is lost on refresh.

VERIFY, THEN DOCUMENT
7. Add focused automated tests for cart quantity validation, totals/persistence and checkout-form validation; run them once in non-watch mode. Install packages; run Angular's configured strict type/template checks and production build; run lint if configured, reporting each check accurately. Start Angular dev server, check its HTTP response, test API requests/CORS from http://localhost:4200 (including preflight), then stop server. Do NOT claim user clicks or complete browser journey unless actually performed.
8. Update docs/frontend-design.md to reflect the actual implementation and any deviations.

README
Keep README goal/status and sections 1, 2, 4–7 and 10 unchanged; mark frontend status truthfully. Update section 3 to ONE accurate annotated fenced `text` project tree showing existing backend AND Angular frontend, docs, Docker and README, including real Angular bootstrap/routes, components/pages, API service, cart state, styling, environment configuration, package scripts, key backend files/tests/migrations; omit node_modules, dist, target. Do NOT duplicate the tree elsewhere.
Section 8. Frontend project structure and run: refer to section 3's combined tree. Give working directory, compatible Node/Angular CLI versions, `npm install`, actual configured check/test/build commands, `npm start`/`ng serve` as appropriate, stopping with Ctrl+C, browser URL http://localhost:4200, environment API URL mechanism, backend/CORS dependencies and PowerShell/CMD-friendly commands.
Section 9. Frontend UI walkthrough and verification — label MANUAL UI TEST (unless real browser interactions actually ran); numbered exact steps, expected results and actual-versus-expected distinction:
   (a) Load UI and six products/stock badges;
   (b) search Espresso + Home & Kitchen filter; clear filters/empty state;
   (c) add Wireless Headphones, cart quantity 2, total $259.98;
   (d) refresh and verify saved cart;
   (e) submit invalid/empty form then valid order; inspect order number/items/date/total; on a fresh DB verify stock 10->8 and cart cleared;
   (f) cart 4 Espresso Machines when stock=3, submit valid details, show exact requested/available 409 and retained cart/customer inputs.
Describe confirmation as derived only from POST response (not persistent order history); include how to start needed services and avoid implying UI checks were automated.

EVIDENCE AND SCOPE
Document actual files, package scripts, versions, endpoints and outcomes. Explicit PASS/FAIL/NOT RUN; never fabricate browser actions. Follow every behavior/out-of-scope constraint in docs/requirements.md. No accounts, payments, images, order lookup, cancellation, product detail or extra features.

FINAL REPORT
Briefly list created files, Angular implementation/checks actually run, blockers and startup instructions.
```

**After Prompt 3:** Review `docs/frontend-design.md`; expand `frontend/` to inspect components, services, routing, templates, SCSS and tests. Check the actual typecheck, test and build results; browser interaction remains a separate manual verification.

## Review the generated outputs

The **`README.md` at the root of the generated `eshop/` application** is a separate document from this lesson README. Claude updates that application README as development progresses; it records the actual project tree, database schema, commands, test evidence, Swagger/API instructions and manual browser walkthrough.

The generated application's documentation should include:

- `docs/requirements.md` — functional specification.
- `docs/backend-design.md` — backend decisions and transaction/stock strategy.
- `docs/frontend-design.md` — Angular architecture, state management and API integration.
- `README.md` — the generated application's reproducible runbook and observed verification results.

## Run and verify eShop

Follow the **generated application's README** for the authoritative, complete, environment-specific instructions. A typical order of operations is:

1. **Database:** From the `eshop/` root, check or start MySQL with Docker Compose; verify that the service is healthy.
2. **Backend tests:** From `eshop/backend/`, with JDK 25 active, run `mvn verify` and inspect the results.
3. **Backend:** From `eshop/backend/`, run `mvn spring-boot:run` and inspect the API in [Swagger UI](http://localhost:8080/swagger-ui/index.html).
4. **Angular:** From `eshop/frontend/`, run `npm install` (when needed) and `npm start`. Open [http://localhost:4200](http://localhost:4200).
5. **Browser:** Verify catalog search/filter, adding products, adjusting and retaining cart quantities, checkout validation, successful order confirmation and the insufficient-stock error. Confirm that failed checkout retains the cart, and that the cart clears only after successful checkout.

These are **instructions for the reader**, not claims that this README ran those commands. The demo database may already contain orders or changed stock after backend HTTP verification, so use actual stock quantities when recording.

## Recording and data notes

- The original seed catalog contains six products. A successful order changes database stock; don't assume seed quantities are still current.
- A clean demo reset may delete locally saved orders and volumes. Only use the reset procedure described in the generated application README when that data can safely be discarded.
- Keep the actual Claude model/effort setting, Java/Maven version and test output visible where they matter; do not substitute hypothetical results.
- The Angular checkout confirmation is shown in the checkout flow, not necessarily a separate confirmation route.

---

**Repository placement:** Save this single file as `lessons/full-stack-spring-boot-angular-app/README.md`. No additional prompt files are required.
