# Build a Full-Stack Spring Boot + React App with Claude Code in IntelliJ IDEA

Follow along with the **EcomStore** demonstration using three copy-ready Claude Code prompts: requirements, backend, and frontend. The application includes **product catalog, shopping cart, and checkout/order confirmation**, with six sample products across three categories. Payments, user accounts, order tracking and cancellation are out of scope.

**Stack:** Java 25, Spring Boot 4 (retain the version in `backend/pom.xml`), Maven, MySQL 8.4 in Docker, Flyway, Swagger/OpenAPI, React, TypeScript, Vite and Tailwind CSS.

## Contents

- [Useful resources](#useful-resources)
- [Prerequisites](#prerequisites)
- [Create the project in IntelliJ IDEA](#create-the-project-in-intellij-idea)
- [Three development prompts](#three-development-prompts)
  - [Prompt 1 — Functional requirements](#prompt-1--functional-requirements)
  - [Prompt 2 — Backend](#prompt-2--backend)
  - [Prompt 3 — Frontend](#prompt-3--frontend)
- [Run and verify EcomStore](#run-and-verify-ecomstore)
- [Demo preparation notes](#demo-preparation-notes)

## Prerequisites

- JDK 25 configured in IntelliJ and for command-line Maven (`java -version`, `mvn -v`).
- IntelliJ IDEA, Claude Code CLI and JetBrains integration (`claude --version`).
- **System Maven**; this lesson explicitly uses `mvn`, **not Maven Wrapper**.
- Docker Desktop running, Docker Compose, Node.js with npm.
- Ports **3306**, **8080** and **5173** available.

## Create the project in IntelliJ IDEA

1. File → New → Project → **Empty Project** named `ecomstore`. Its directory is the project root.
2. Set Project SDK to **JDK 25**.
3. File → New → Module → **Spring Boot**. Name the module `backend` under `ecomstore/backend/` and use **Java 25**, **Maven**, **Jar**, group `org.learningfromexperience`, artifact `ecomstore`, package `org.learningfromexperience.ecomstore`, a stable Spring Boot 4 version, and **Spring Web** as the initial dependency.
4. Confirm that `backend/pom.xml` exists. Open the terminal at the **project root**, launch `claude` in a **fresh session**, and use **Auto mode** for the three prompts.
5. Review the generated files and results after each prompt; do not assume reported tests or browser interactions took place unless shown in Claude's output.

## Three development prompts

The following prompt blocks are reproduced **verbatim** from the finalized EcomStore recording script. Submit them in this order.

### Prompt 1 — Functional requirements

Creates `docs/requirements.md` and the initial project `README.md`. No application code yet.

```text
Create docs/requirements.md as a short FUNCTIONAL specification of EcomStore
based on the requirements below.

Group requirements under Catalog, Cart, and Checkout. Give each distinct,
testable behavior a stable ID — CAT-001..., CART-001..., CHK-001... —
numbered within its group and never reused.

State each requirement as "The system SHALL ..." and place one or more
"WHEN ... THEN ..." acceptance scenarios directly below it.

After these groups, add Sample data, Visual expectations, and Out of scope.
Do not invent more features.

Also create a short README.md describing the goal and marking
backend/frontend as not implemented.

FEATURE 1 — PRODUCT CATALOG
- Show a single catalog of products with name, description, category, price, and stock
  quantity.
- Search by product name using case-insensitive substring matching after trimming spaces,
  and filter by category. Either filter is optional; they can work together.
- Customers can add a product directly to the shopping cart. No separate product-detail
  page.

FEATURE 2 — SHOPPING CART
- Add products (adding the same product again increases its quantity by one), change
  quantities (positive integers), remove products, and show line subtotals and cart total.
- Preserve the cart when the page is refreshed; show the total number of units in the
  header cart count.
- Customers may increase quantities in the cart even beyond the stock currently
  available. The server checks stock only when an order is submitted at checkout.
  Products showing zero stock cannot be added from the catalog. If an item already in
  the cart later becomes unavailable, keep it in the cart until checkout reports the
  shortage; do not silently remove it or change its quantity.
- No login or account is required.

FEATURE 3 — CHECKOUT AND ORDER CONFIRMATION
- Collect customer full name, email address, and shipping address; require at least one
  order item.
- Reject missing/invalid required fields and nonpositive quantities with a clear message
  identifying the problem.
- Validate available stock at checkout. If any requested quantity is unavailable, identify
  the affected item and requested/available quantities. Do not create a partial order or
  change stock on failure.
- Use current authoritative product prices, not cart-submitted prices. Calculate the
  correct order total.
- On success save the order and its items, reduce stock, and show a unique order number,
  ordered items, quantities, item subtotals, total and order date/time. Clear the cart only
  after confirmed success.

SAMPLE PRODUCTS (use these exact six records in three categories):
- Electronics: Wireless Headphones, $129.99, stock 10 — Comfortable over-ear headphones for
  everyday listening.
- Electronics: Mechanical Keyboard, $89.50, stock 15 — Compact mechanical keyboard with
  tactile switches.
- Books: Clean Code, $34.95, stock 20 — A practical book about writing maintainable
  software.
- Books: Spring Boot in Practice, $44.00, stock 10 — Hands-on guidance for building Spring
  Boot applications.
- Home & Kitchen: Espresso Machine, $249.99, stock 3 — Countertop espresso maker for home
  coffee brewing.
- Home & Kitchen: Water Bottle, $24.99, stock 25 — Reusable insulated bottle for daily use.
- Use these exact descriptions.

VISUAL EXPECTATIONS
A simple, responsive EcomStore interface: navy header, prominent search, amber Add to Cart
buttons and cart count, light-gray background, white rounded product cards, category-filter
pills under the header, a green/amber/red availability badge on each product card, and a
dark footer.

Clear empty/loading/error states. Badges: green “In stock” for stock above 5, amber “Only N
left” for stock 1–5, and red “Out of stock” for stock 0; disable Add to Cart at stock 0.
Prices display as USD with two decimal places.


OUT OF SCOPE
Payments, accounts, product detail, images, pagination, sorting, order tracking/lookup,
cancellation, refunds, status workflow, admin, coupons, email, deployment, and
production-scale concurrency/load testing.

GUARDRAILS — KEEP REQUIREMENTS IMPLEMENTATION-INDEPENDENT: This requirements document must
describe WHAT the app does, not HOW to implement it. Do not choose Java classes, entities,
database tables, API routes, HTTP status codes, JSON schemas, frameworks, architecture, or
test file names. Those decisions belong to subsequent prompts.

```

After Prompt 1, review the requirement IDs and acceptance scenarios, six sample products, visual expectations, out-of-scope list and initial README implementation status.

### Prompt 2 — Backend

Reads `docs/requirements.md` and `README.md`; creates `docs/backend-design.md`, implements and tests Spring Boot/MySQL APIs, and adds a reproducible README runbook.

```text
OBJECTIVE
Read docs/requirements.md and README.md. Design and implement the backend for the
three approved EcomStore features. Do not implement the frontend or add features.

TECHNICAL CONSTRAINTS

Java and Spring Boot
- Use Java 25 and the existing backend/pom.xml. Keep its Spring Boot version,
  coordinates, base package and main class. If the project is missing, stop and report it.
- Use Spring MVC, Spring Data JPA and Bean Validation. Use Java records for API DTOs,
  BigDecimal for money, and no Lombok or MapStruct.

Maven and testing
- Use the system-installed `mvn`, not Maven Wrapper. Keep code and tests in backend/.
- Use JUnit 5. If tests use *IT filenames, configure Failsafe so `mvn verify` runs them.

MySQL and Docker
- Use MySQL 8.4 in a root compose.yaml with one service named `mysql`, port 3306,
  a named volume and a health check.
- Configure database `ecomstore_db`, user `ecomstore`, password `ecomstore`.
  The Docker MYSQL_USER/MYSQL_PASSWORD settings create the user. A one-time initialization
  SQL file creates `ecomstore_test` and grants that existing user access to it;
  do not CREATE USER again. Tests must use the isolated test database.

Flyway
- Use `spring-boot-starter-flyway` and `flyway-mysql`.
- Use versioned SQL migrations to create the schema and seed the exact three
  categories and six products from docs/requirements.md.

OpenAPI and CORS
- Use springdoc OpenAPI compatible with the installed Spring Boot version.
- Allow CORS for `/api/**` only from `http://localhost:5173`, including preflight.

REST API CONTRACT
Use exactly these three public operations under `/api/v1`. Do not expose JPA entities.

- GET /categories
  Response: [{id, name}]
- GET /products?search=&categoryId=
  Response: [{id, name, description, price, stock, categoryId, categoryName}]
- POST /orders
  Request: {customer:{fullName,email}, shippingAddress,
            items:[{productId,quantity}]}
  Success (201): {orderNumber, customer, shippingAddress,
                  items:[{productId,productName,unitPrice,quantity,subtotal}],
                  totalAmount, createdAt}

API ERROR HANDLING
- Use Spring ProblemDetail: HTTP 400 identifies invalid fields; HTTP 409 identifies
  each short-stock product with requested and available quantities.

GUARDRAILS
Preserve this public API contract. Calculate prices on the server and create orders
transactionally. Decrement stock with a conditional atomic update that succeeds only
when sufficient stock remains; roll back the entire order on any failure. Use a unique
order number without elaborate collision-retry logic. Keep the design minimal.

IMPLEMENTATION

1. Backend design
   IMPORTANT — DOCUMENT DECISIONS: Create docs/backend-design.md, beginning with a
   Constraints section recording the stack, version, databases, Maven rules, CORS
   and API contract above. Then document the actual domain model, schema, services,
   transactions, stock strategy, request/response examples and error format.

2. Backend implementation
   Implement the design using controllers, services, DTOs, persistence, Docker Compose
   and Flyway migrations. Do not add features outside docs/requirements.md.

3. Automated tests
   Test catalog/search/filter, valid order and stock reduction, invalid requests,
   and insufficient-stock rollback. Each test must have a plain-English @DisplayName
   beginning with the relevant requirement ID from docs/requirements.md.

4. Backend verification
   Start MySQL; run `mvn verify` from backend/; start Spring Boot; check the three
   endpoints and live `/v3/api-docs` with real HTTP requests. Include a successful
   order and an insufficient-stock rejection. Update docs/backend-design.md if the
   implementation differs, without changing the agreed public API. Report actual
   results and any checks not run.

   Live order requests modify demo stock. Do not perform manual SQL deletion or stock
   restoration to hide those changes; document the clean database reset in README.md.

5. README runbook
   IMPORTANT — REPRODUCIBLE RUNBOOK: Update README.md so someone can run and
   understand the project without seeing these prompts. Preserve the unnumbered
   project goal and status at the top, updating the backend status accurately.
   Provide commands compatible with PowerShell and CMD, including the working
   directory, expected URLs and results.

   Use these numbered sections, leaving sections 8 and 9 for Prompt 3:
   1. Prerequisites — JDK 25, system Maven and Docker commands.
   2. Database setup — Docker startup, databases and credentials.
   3. Backend project structure — show a readable, actual file-and-folder tree in
      a fenced `text` block, starting at the project root and showing the important
      backend source, tests, resources, configuration, migrations and Docker files.
      Annotate significant files or folders inline with short `#` comments explaining
      their roles. Show real files only; do not invent or dump generated build files.
      Prompt 3 will expand this into ONE combined backend-and-frontend project tree.
   4. Database schema and sample data — show the actual database tables in a
      Markdown table with columns such as Table, Column, SQL type, Key/constraint,
      and Purpose (plain-English meaning of the column). Explain relationships
      and important constraints. Separately list the six actual seeded products
      in a table with ID, Product, Category, Price, Stock and Purpose (for example,
      which demo/test scenario the seed supports). Use actual IDs from the database,
      not assumed positions. Give a short SQL inspection command after the tables.
   5. Flyway migrations — each actual SQL file, its version, purpose and execution
      order; which script seeds data; when migrations run at test/app startup;
      `flyway_schema_history` inspection command. Distinguish Docker initialization
      from Flyway migrations and ordinary order data changes. Applied versioned
      migrations are not edited.
   6. Backend tests — `mvn verify`, actual test class/file and @DisplayName for
      EVERY executed test, linked requirement ID, PASS/FAIL/SKIPPED and totals.
   7. Run backend and test APIs with Swagger UI — use short, numbered instructions:
      (a) In the project root, check/start MySQL using Docker Compose and verify
          service health. Explain whether this step is already done.
      (b) In a terminal opened in `backend/`, run `mvn spring-boot:run` and wait
          for successful startup. Do not imply that this terminal can be used for
          other commands while the app is running; use another terminal if needed.
      (c) Open `http://localhost:8080/swagger-ui/index.html` and locate the
          Categories, Products and Orders endpoints. Include the raw API docs URL
          `http://localhost:8080/v3/api-docs` as an optional check.
      (d) Explain the Swagger UI interaction: expand endpoint -> Try it out ->
          enter parameters or edit JSON -> Execute -> inspect HTTP status and
          Response body. Start with GET /api/v1/categories (3 categories) and
          GET /api/v1/products (6 products). Obtain product IDs from the live
          product response and use those IDs in subsequent order requests.
      (e) For POST /api/v1/orders, show clearly labeled, copyable JSON request
          bodies: successful checkout (2 Wireless Headphones), insufficient stock
          (4 Espresso Machines) and invalid email. Use the same customer/address
          request shape as the implemented API. Give expected status and the key
          response fields for each: 201 and total 259.98 with stock 10 -> 8;
          409 with requested 4/available 3 and no order/stock change; 400 with
          a customer.email validation error. State that IDs, order numbers and
          timestamps can differ. Put request examples directly in the relevant
          Swagger steps, not in a separate unexplained list of JSON files/curl.
      (f) Re-run GET /api/v1/products to confirm stock after the success case.
          Provide OPTIONAL SQL commands to inspect stock and saved orders, with
          clear working directory/context and actual table names. Explain that
          live orders change demo data; do not silently reset it.
      Distinguish expected outputs from checks actually performed. Do not claim
      Swagger was used interactively unless it really was; if Claude only verified
      via HTTP, mark the Swagger instructions as manual steps for the reader.
   8. Frontend project structure and run — reserved for Prompt 3.
   9. Frontend UI walkthrough and verification — reserved for Prompt 3.
   10. Demo reset — stop backend; `docker compose down -v` and
       `docker compose up -d --wait`; restart backend to reapply migrations.
       Warn that this destroys all local orders and volume data.

   Explain that MySQL's fresh-volume init script creates the test database,
   while Flyway applies application migrations and seed data on startup.
   Do not invent filenames, database columns or test results. Do not create a
   separate test-report document or change docs/changes/.

FINAL REPORT
Briefly report design files, implementation, checks actually run
(PASS/FAIL/NOT RUN), blockers and how to start the backend.
```

After Prompt 2, review the real `mvn verify` result, `docs/backend-design.md`, README project hierarchy and API tests. For manual checks, use the generated README's Swagger UI guide.

### Prompt 3 — Frontend

Reads all existing documents, checks the live `/v3/api-docs` API contract, implements React and updates the README's frontend runbook and manual UI test.

```text
OBJECTIVE
Read docs/requirements.md, docs/backend-design.md and README.md. Build the frontend
for the three approved EcomStore features. Do not alter the backend API or add features.

TECHNICAL CONSTRAINTS
- React, TypeScript, Vite, Tailwind CSS, React Router and npm; place files in frontend/.
- Backend: `http://localhost:8080`; frontend: `http://localhost:5173`.
- Configure `VITE_API_BASE_URL` and provide `frontend/.env.example`.
- Match the navy-and-amber visual requirements in docs/requirements.md.

API INTEGRATION
1. Start MySQL with `docker compose up -d --wait` from project root and start
   the backend with `mvn spring-boot:run` from backend/ if needed.
2. Fetch `http://localhost:8080/v3/api-docs`; check it against
   docs/backend-design.md. If unavailable or inconsistent, report and stop.
   Use the actual verified request and response fields.

FRONTEND DESIGN
3. IMPORTANT — DOCUMENT DECISIONS: Create docs/frontend-design.md starting with
   Constraints (frameworks, ports, environment variable, visual style). Describe
   actual routes, pages/components, cart state, API client, validation, error
   handling and responsive design. Keep the component structure small.

FEATURE IMPLEMENTATION
4. Catalog — product cards, search, category pills, loading/empty/error states.
   Follow exact stock badge labels and disable Add to Cart when stock is zero.
5. Cart — add/remove items, edit positive integer quantities, show subtotals
   and totals, persist in localStorage. Do not cap cart quantities at stock.
6. Checkout — customer form, POST to the verified API, inline validation and
   clear stock-shortage details. Show confirmation from the successful response
   and clear the cart only after success; no order lookup is required.

FRONTEND VERIFICATION
7. Install packages, run the project's type-check command if defined, and run
   `npm run build` (lint if configured). Start Vite and verify its server
   responds and backend CORS accepts `http://localhost:5173`. Stop Vite afterward.
   Do not claim browser interactions were tested unless actually performed.
8. Update docs/frontend-design.md to match what was built; report deviations.

README DOCUMENTATION
9. IMPORTANT — REPRODUCIBLE FRONTEND DEMO: Preserve README sections 1–7 and 10,
   and the unnumbered goal/status. Update the frontend status, then fill sections:
   8. Frontend project structure and run — replace README section 3's partial
      backend tree with ONE complete file-and-folder hierarchy for the project,
      covering backend AND frontend, along with docs, Docker Compose and README.
      Use a fenced `text` tree, with concise inline `#` comments for the important
      folders and actual files: frontend entry point, pages/components, API client,
      cart persistence/state, styling, environment template, package configuration,
      plus key backend files, tests and migrations. Show only files that exist,
      and avoid generated directories (node_modules, dist, target). Do not repeat
      a large path table. In this section, explain how to configure and launch
      the frontend: working directory `frontend/`, `npm install`, available
      type-check command, `npm run build`, `npm run dev`, Ctrl+C, browser URL,
      API base URL/environment configuration, and backend/CORS prerequisites.
      Refer to the combined tree in section 3 instead of duplicating it.
   9. Frontend UI walkthrough and verification — provide a simple numbered,
      browser-based user journey rather than calling it a generic walkthrough.
      Explain what to click, what should appear and how to check each result:
      (a) open the UI and check that all six products load;
      (b) search Espresso and choose Home & Kitchen, then clear filters;
      (c) add Wireless Headphones, open cart, set quantity to 2, confirm 259.98;
      (d) refresh and confirm cart persists;
      (e) complete checkout with valid customer details, inspect confirmation,
          verify stock 10 -> 8 on a fresh seeded database and cleared cart;
      (f) attempt checkout with 4 Espresso Machines (3 in stock), inspect the
          requested/available error and confirm the cart remains unchanged.
      Label this section a MANUAL UI TEST; do not claim Claude performed browser
      clicks unless it actually did. State the confirmation comes from the POST
      response and is not an order-history or refresh-persistent page.

   State that the confirmation relies on the POST response and does not survive
   a refresh. Don't list generated build output or duplicate the full design file.

GUARDRAILS
Follow every behavior in docs/requirements.md, including stock badge text,
stock-zero button behavior and excluded features. Do not introduce accounts,
payments, tracking, cancellation or an additional product-detail page.

FINAL REPORT
Briefly report design files, implementation, checks actually run
(PASS/FAIL/NOT RUN), blockers and how to start the frontend.
```

After Prompt 3, compare typecheck/build output and the README's documented results. The browser walkthrough is a **manual test**, unless Claude explicitly reports performing it.

## Run and verify EcomStore

**The README generated inside your own EcomStore project is the primary runbook.** It contains the actual project tree, test results, configuration, and detailed Swagger/UI test steps. Use its Sections 2, 7, 8 and 9 during the demonstration rather than memorizing commands.

1. **MySQL — from the project root:** first check whether it is already running. Only start if required.

   ```powershell
   docker compose ps
   docker compose up -d --wait
   ```

2. **Backend — from `backend/`:** if `http://localhost:8080/v3/api-docs` doesn't respond, start Spring Boot (with JDK 25 active).

   ```powershell
   mvn spring-boot:run
   ```

   Swagger UI: <http://localhost:8080/swagger-ui/index.html>.

3. **Frontend — from `frontend/` in another terminal:** start it only if not already running.

   ```powershell
   npm install
   npm run dev
   ```

   `npm install` is needed only if dependencies are missing. Visit <http://localhost:5173>.

4. **Browser verification — follow generated README Section 9:** six products; search `Espresso` and filter Home & Kitchen; add two Wireless Headphones ($259.98); refresh to demonstrate cart persistence; place an order, check confirmation and stock decrement; request four Espresso Machines with only three in stock and verify the shortage error and unchanged cart.

## Demo preparation notes

- **Browser cart data** is stored in localStorage and is independent of the Docker database. A previously used browser may show saved cart items, even after app restart.
- A live backend test may already have reduced Wireless Headphones stock **10 → 8**. If you need the original starting stock for recording, follow the generated README's **Demo reset** steps, but be aware that `docker compose down -v` **deletes the entire local MySQL data volume**, including any saved orders.
- `mvn verify` does not keep a backend server running. Claude may also stop Vite after testing. Check each service before starting another instance.
- This requirements-first, six-product demonstration is **different from the repository's older blueprint-based EcomStore**, which included payments, tracking and cancellation. Do not treat an older finished-code snapshot as equivalent to these prompts.
