# Build a Full-Stack Spring Boot + React App with Claude Code in IntelliJ IDEA

Follow along with the EcomStore demonstration: an online store built from an empty IntelliJ IDEA project with three Claude Code prompts. The backend is a Spring Boot 4 REST API on Java 25 with MySQL in Docker, and the frontend is React with Vite, TypeScript and Tailwind CSS. The prompts below follow the recording order so you can copy each one into Claude Code.

Finished code: **[ecomstore-spring-boot-react](https://github.com/j2eeexpert2015/ecomstore-spring-boot-react/tree/video-1-full-stack-app)** (tag `video-1-full-stack-app`).

## Contents
- [Useful Resources](#useful-resources)
  - [Claude Code with IntelliJ IDEA](#claude-code-with-intellij-idea)
  - [Spring Boot](#spring-boot)
  - [springdoc-openapi](#springdoc-openapi)
  - [Docker Desktop](#docker-desktop)
  - [Node.js](#nodejs)
  - [Vite and Tailwind CSS](#vite-and-tailwind-css)
- [Prerequisites](#prerequisites)
- [Create the Project in IntelliJ IDEA](#create-the-project-in-intellij-idea)
  - [Empty Project](#empty-project)
  - [Project JDK](#project-jdk)
  - [Backend Module](#backend-module)
  - [Start Claude Code](#start-claude-code)
- [Full-Stack Development Prompts](#full-stack-development-prompts)
  - [Prompt 1 — Blueprint](#prompt-1--blueprint)
  - [Prompt 2 — Backend](#prompt-2--backend)
  - [Prompt 3 — Frontend](#prompt-3--frontend)
- [Run the Store](#run-the-store)
- [Troubleshooting](#troubleshooting)
- [Finished Code](#finished-code)

## Useful Resources

### Claude Code with IntelliJ IDEA

Installation and the JetBrains plugin are covered in the first lesson: [Claude Code with IntelliJ IDEA: Setup & Your First Java Project](../claude-code-installation-intellij-idea/README.md).

https://code.claude.com/docs/en/jetbrains

### Spring Boot

Official Spring Boot reference documentation.

https://docs.spring.io/spring-boot/index.html

### springdoc-openapi

The library that serves Swagger UI and `/v3/api-docs` for the backend.

https://springdoc.org

### Docker Desktop

Installation instructions for Windows, macOS and Linux. MySQL runs in Docker in this lesson.

https://docs.docker.com/desktop/

### Node.js

Download the LTS version; it includes npm.

https://nodejs.org/en/download

### Vite and Tailwind CSS

The frontend tooling Claude sets up in Prompt 3.

https://vite.dev/guide/

https://tailwindcss.com/docs/installation/using-vite

## Prerequisites

- **JDK 25**: `java -version` shows 25, and `JAVA_HOME` points to JDK 25.
- **IntelliJ IDEA** with the Claude Code plugin, and Claude Code logged in: `claude --version`.
- **Docker Desktop**, running ("Engine running").
- **Node.js LTS**: `node -v` and `npm -v`.

Ports 3306, 8080 and 5173 must be free. Use a project folder without spaces in its path.

## Create the Project in IntelliJ IDEA

### Empty Project

File → New → Project → **Empty Project**. Name `ecomstore`, location for example `C:\projects`. This folder is the project root; it will hold `backend/`, `frontend/`, `docs/` and `compose.yaml`.

### Project JDK

File → Project Structure → Project → SDK **25**.

### Backend Module

File → New → Module → **Spring Boot**, with these values:

| Field | Value |
|---|---|
| Name | `backend` (the location must end in `ecomstore\backend`) |
| Language / Type | Java / Maven |
| Group | `org.learningfromexperience` (or your own) |
| Artifact | `ecomstore` |
| Package name | `org.learningfromexperience.ecomstore` |
| JDK / Java | 25 / 25 |
| Packaging | Jar |
| Spring Boot | newest 4.1.x (not SNAPSHOT, M or RC) |
| Dependencies | **Spring Web** only. Claude adds the rest. |

### Start Claude Code

Open the Terminal in the `ecomstore` folder and run `claude`. Run all three prompts in the same session.

## Full-Stack Development Prompts

These prompts match the recording, in demonstration order. The whole lesson uses **Auto mode** (Shift+Tab to switch). Each prompt prints a `[Step N/…]` status line per step and ends with a report under fixed headings. Review what Claude built before moving on: the code, the tests, Swagger UI and the browser.

### Prompt 1 — Blueprint

Writes `docs/PROJECT_BLUEPRINT.md` and a first `README.md`. No application code yet.

```text
Create docs/PROJECT_BLUEPRINT.md in the current folder, containing the specification below, formatted with clear headings and tables where useful. Then create README.md at the project root as described in section 11; at this point only the blueprint exists. Do not write any application code yet. Finish with a short report under the headings "Created" and "Notes".

SPECIFICATION — EcomStore

1. WHAT WE ARE BUILDING
An online store with no user accounts. Customers:
- browse and search products;
- keep a cart in the browser;
- check out with their contact details and a simulated payment;
- view or cancel their order by order number.
Stack:
- Backend: Java 25, Spring Boot (version in backend/pom.xml), Maven, Spring Web MVC, Spring Data JPA, Bean Validation, Flyway, MySQL 8.4 in Docker, springdoc-openapi. No Lombok, no MapStruct.
- Frontend: React + Vite + TypeScript + Tailwind CSS + React Router.
Project layout (the current folder is the project root):
- backend/: the Spring Boot Maven project, created with Spring Initializr. All backend code, tests, resources and Maven files live here.
- frontend/: the React app, created later.
- docs/: this blueprint and the test report.
- compose.yaml and docker/mysql/init/: MySQL in Docker.
The backend keeps the Initializr project's groupId, artifactId and main class. The main class's package is the base package, with subpackages entity, repository, service, dto, mapper, controller, exception, config.

2. DOMAIN MODEL (package entity)
- Category (table categories): name (unique), description, active.
- Product (table products): name, description, price, stock, active, category.
- Customer (table customers): email (unique), fullName, phone.
- Order (table orders): orderNumber (ORD- + 8 uppercase alphanumeric chars, unique), customer, status, shippingAddress, totalAmount, items, payment.
- OrderItem (table order_items): product, productName and unitPrice copied at purchase time, quantity, subtotal.
- Payment (table payments): one per order; method, status, amount, transactionId.
- Enums:
  - OrderStatus: PENDING, CONFIRMED, PROCESSING, SHIPPED, DELIVERED, CANCELLED.
  - PaymentMethod: CREDIT_CARD, DEBIT_CARD, UPI, NET_BANKING, WALLET, CASH_ON_DELIVERY.
  - PaymentStatus: PENDING, COMPLETED, FAILED, REFUNDED, CANCELLED.
- Money is BigDecimal (DECIMAL(10,2)).
- Long ids; createdAt/updatedAt as Instant on every entity; no hard deletes.
- Saving an Order saves its items and payment.

3. REPOSITORIES (package repository)
- CategoryRepository: findByActiveTrueOrderByNameAsc().
- ProductRepository:
  - findCatalog(search, categoryId, pageable): active products in active categories; search is a case-insensitive "name contains"; search and categoryId are optional;
  - findActiveById(id): one active product in an active category;
  - findAllByIdInForUpdate(ids): products by ids with a pessimistic write lock, in ascending id order (used by checkout and cancel).
- CustomerRepository: findByEmail(email).
- OrderRepository: findByOrderNumber(orderNumber); existsByOrderNumber(orderNumber).

4. SERVICES AND BUSINESS RULES (package service)
CatalogService: listCategories(); listProducts(search, categoryId, page, size, sort) with default page size 12, maximum 50, and sort by name, price or createdAt (any other sort field returns 400); getProduct(id) (404 if missing or inactive). Inactive products, and products in inactive categories, are never returned.

OrderService.placeOrder (one transaction):
- Validate: items not empty, quantities positive, no duplicate productIds (otherwise 400).
- Find the customer by email or create them; update name and phone if they already exist.
- Lock the products. If any product is missing, inactive or lacks stock, reject with 409 listing every failing line, and change nothing.
- Calculate prices and totals on the server from current product prices; never trust the client.
- Create the order, items and payment; reduce stock.
- Order number collision: retry up to 3 times, then fail with 500 and log it. Order numbers come from an OrderNumberGenerator component.
- Simulated payment: CASH_ON_DELIVERY starts PENDING; all other methods are COMPLETED immediately with a generated transactionId. No card data.

OrderService.changeStatus. Allowed moves:
- PENDING -> CONFIRMED or CANCELLED
- CONFIRMED -> PROCESSING or CANCELLED
- PROCESSING -> SHIPPED or CANCELLED
- SHIPPED -> DELIVERED
DELIVERED and CANCELLED are final; anything else is 409. Moving to CANCELLED through changeStatus is allowed wherever the table allows it, including from PROCESSING, and has the same effects as cancelOrder: stock restored exactly once, and the payment refunded or cancelled. cancelOrder, the customer's cancel, stays limited to PENDING and CONFIRMED. When an order becomes DELIVERED, a PENDING cash-on-delivery payment becomes COMPLETED.

OrderService.cancelOrder (one transaction):
- Allowed only in PENDING or CONFIRMED (otherwise 409).
- Lock the products and restore stock exactly once.
- A COMPLETED payment becomes REFUNDED; a PENDING payment becomes CANCELLED.

OrderService.getOrder: by orderNumber (404 if not found).

5. CONTROLLERS AND API (packages controller, dto, mapper, exception, config)
Base path /api/v1. Controllers are thin: CategoryController, ProductController, OrderController. Requests and responses are Java records; mappers CatalogMapper and OrderMapper are hand-written; entities are never exposed. Services return these records, and mapping happens inside the service transaction, because open-in-view is off.
Endpoints:
- GET /categories -> List<CategoryResponse>
- GET /products?search=&categoryId=&page=&size=&sort= -> PageResponse<ProductResponse>
- GET /products/{id} -> ProductResponse
- POST /orders with CreateOrderRequest -> 201 with OrderResponse
- GET /orders/{orderNumber} -> OrderResponse
- POST /orders/{orderNumber}/cancel -> OrderResponse
- PATCH /orders/{orderNumber}/status with UpdateOrderStatusRequest -> OrderResponse (for demos in Swagger; intentionally unprotected in this version)
Records (exact field names):
- CategoryResponse {id, name, description}
- ProductResponse {id, name, description, price, stock, categoryId, categoryName}
- PageResponse<T> {content, page, size, totalElements, totalPages}
- CreateOrderRequest {customer: CustomerRequest {email, fullName, phone}, shippingAddress, paymentMethod, items: [OrderItemRequest {productId, quantity}]}
- UpdateOrderStatusRequest {status}
- OrderResponse {orderNumber, status, customer {email, fullName, phone}, shippingAddress, items: [{productId, productName, unitPrice, quantity, subtotal}], totalAmount, payment {method, status, amount, transactionId}, createdAt}
Errors: GlobalExceptionHandler (@RestControllerAdvice) returns ProblemDetail, using ResourceNotFoundException (404), InvalidRequestException (400), ConflictException (409), InsufficientStockException (409) and OrderNumberGenerationException (500):
- 400: title "Validation failed", plus errors: [{field, message}];
- 404: title "Not found";
- 409, stock: title "Insufficient stock", plus failures: [{productId, productName, requested, available}];
- 409, other conflicts: title "Conflict", with a detail sentence;
- 500: title "Order number generation failed".
OpenApiConfig: Swagger UI and /v3/api-docs via springdoc-openapi (a version compatible with this Spring Boot version). The API docs show the real status codes (for example 201 for POST /orders). The example body for POST /orders is exactly manual check 2 in section 8.
CorsConfig: allow only http://localhost:5173 on /api/**.

6. FRONTEND (frontend/)
- Vite React + TypeScript app with Tailwind CSS and React Router.
- Files:
  - frontend/src/api: client.ts (fetch wrapper and error handling), types.ts (types matching the API records), catalog.ts, orders.ts;
  - frontend/src/cart: CartContext.tsx (cart state saved in localStorage);
  - frontend/src/components: Header, CategoryPills, ProductCard, StockBadge, StatusBadge, Pagination, Footer, States (loading, empty and error);
  - frontend/src/pages: CatalogPage, ProductPage, CartPage, CheckoutPage, OrderPage, TrackOrderPage.
- The API URL comes from VITE_API_BASE_URL in frontend/.env.local; frontend/.env.example documents it.
- Pages:
  - / : product catalog with category filter and pagination; the search term comes from the header search box
  - /products/:id : product details with a quantity selector, capped at available stock, and Add to cart
  - /cart : change quantity or remove items, capped at available stock
  - /checkout : email, full name, phone, shipping address, payment method; on 409, show which items failed
  - /orders/:orderNumber : order details, with a Cancel button while cancellable
  - /track : a small form to enter an order number and open its order page
- Design: a clean marketplace look, without product images.
  - Header: dark navy bar with the store name "EcomStore" in white on the left, a wide search box in the middle, and "Track order" and a Cart link with an amber item-count badge on the right. Nothing else in the header.
  - Category filter: a row of pill buttons under the header (All plus each category); the active pill is navy with white text.
  - Page background light grey; product cards white with rounded corners and a soft shadow, in a responsive grid.
  - Product card: category as a small coloured label, product name in bold, a two-line description, the price large and bold, a stock badge (green "In stock", amber "Only N left", red "Out of stock"), and a full-width amber "Add to cart" button.
  - Order page: a coloured status badge for the order and for the payment.
  - Footer: a simple dark footer with the store name.
  - Colours: navy for structure, amber for actions, green/amber/red only for status. No gradients, no image placeholders, no sort control.
- After a successful order, clear the cart and open the order page.
- Prices formatted with Intl.NumberFormat in USD. Loading, empty and error states. Responsive down to mobile width.
- Behaviour details:
  - the catalog keeps search, category and page in the URL (/?search=&categoryId=&page=);
  - the stock badge shows "Only N left" when stock is 5 or less;
  - the cart badge in the header appears only when the cart has items;
  - the cart keeps each product's price and stock from when it was added; the server checks everything again at checkout;
  - the Cancel button asks for confirmation with the browser's confirm dialog;
  - keep the linter that comes with the Vite template; formatting and small helper functions live in frontend/src/api/client.ts.

7. DATABASE AND CONFIGURATION
- MySQL 8.4 runs in Docker via compose.yaml at the project root, with one service, mysql:
  - port 3306, a named volume, a healthcheck;
  - MYSQL_DATABASE=ecomstore_db; MYSQL_USER=${DB_USERNAME:-ecomstore}; MYSQL_PASSWORD=${DB_PASSWORD:-ecomstore}; a random root password;
  - docker/mysql/init/01-create-test-database.sh creates ecomstore_test and gives the app user full access to it.
- Local demo credentials: username ecomstore, password ecomstore. They can be overridden with DB_USERNAME and DB_PASSWORD, but nothing requires setting them.
- Flyway migrations in backend/src/main/resources/db/migration: V1__create_schema.sql creates the schema; V2__seed_catalog.sql seeds exactly the categories and products below, inserted in this order, so on a fresh database the ids are 1-4 and 1-12. Prices are in USD. No seed customers or orders.
  Categories (id | name | description):
  1 | Electronics | Audio, accessories and gadgets
  2 | Books | Technical and general reading
  3 | Home & Kitchen | Everyday essentials for the home
  4 | Sports & Outdoors | Gear for training and the outdoors
  Products (id | name | category | price | stock | description):
  1 | Wireless Headphones | Electronics | 129.99 | 25 | Over-ear Bluetooth headphones with 30-hour battery
  2 | Mechanical Keyboard | Electronics | 89.50 | 30 | Tactile switches, aluminium frame, backlit keys
  3 | Portable Power Bank | Electronics | 39.99 | 4 | 20,000 mAh fast-charging power bank
  4 | Clean Code | Books | 34.95 | 40 | A handbook of agile software craftsmanship
  5 | Spring Boot in Practice | Books | 44.00 | 25 | Hands-on guide to building Spring applications
  6 | Limited Edition Atlas | Books | 189.00 | 3 | Illustrated world atlas, collector's edition
  7 | Espresso Machine | Home & Kitchen | 249.99 | 12 | 15-bar pump espresso maker with milk frother
  8 | Ceramic Dinner Set | Home & Kitchen | 79.00 | 20 | 16-piece stoneware set for everyday dining
  9 | Stainless Steel Water Bottle | Home & Kitchen | 24.99 | 60 | Insulated 750 ml bottle, keeps drinks cold
  10 | Trail Running Shoes | Sports & Outdoors | 109.00 | 18 | Lightweight shoes with a grippy outsole
  11 | Two-Person Camping Tent | Sports & Outdoors | 159.00 | 5 | Waterproof dome tent with quick setup
  12 | Yoga Mat | Sports & Outdoors | 19.99 | 50 | Non-slip 6 mm mat with carry strap
- backend/src/main/resources/application.properties:
  - jdbc:mysql://localhost:3306/ecomstore_db, with username ${DB_USERNAME:ecomstore} and password ${DB_PASSWORD:ecomstore};
  - ddl-auto=validate; open-in-view off.
- Ports: MySQL 3306, backend 8080, frontend 5173.

8. TESTS (backend/src/test/java)
- Test configuration: backend/src/test/resources/application.properties points every Spring test, including the one Initializr generated, at ecomstore_test (same credentials). Tests never touch ecomstore_db. Flyway applies V1 and V2 to ecomstore_test as well.
- Write exactly the test classes, groups and tests below: no more, no fewer. Group tests with @Nested using the group names shown, and give every test the @DisplayName shown, so IntelliJ's test tree reads like a specification. Parameterized tests run one case per allowed or rejected move.
- OrderServiceTest (JUnit 5 + Mockito, no database):
  - Place order: "creates the order, items and payment with totals from current prices"; "reduces stock by the ordered quantities"; "rejects the whole order with 409 when any item lacks stock, and lists every failing item"; "rejects an inactive or unknown product with 409"; "rejects an empty item list, a non-positive quantity or a duplicate product with 400"; "reuses an existing customer by email and updates name and phone"; "marks cash on delivery PENDING and other methods COMPLETED with a transaction id"; "retries the order number on a collision and fails with 500 after 3 attempts".
  - Status changes: "allows every move in the status table" (parameterized); "rejects every other move with 409" (parameterized); "completes a pending cash-on-delivery payment when the order is delivered".
  - Cancel order: "restores stock for every item"; "refunds a completed payment"; "cancels a pending cash-on-delivery payment"; "rejects cancelling an order that is no longer PENDING or CONFIRMED with 409".
  - Get order: "returns 404 for an unknown order number".
- CatalogServiceTest (JUnit 5 + Mockito, no database):
  - Catalog: "uses page size 12 by default and caps it at 50"; "rejects an unknown sort field with 400"; "returns 404 for a missing or inactive product".
- OrderControllerTest (@WebMvcTest, no database):
  - Order API: "POST /orders returns 201 with the order"; "POST /orders returns 400 with the failing fields"; "POST /orders returns 409 with the failing items"; "GET /orders/{orderNumber} returns 404 for an unknown order"; "PATCH /orders/{orderNumber}/status returns 409 for a move not in the table".
- ProductControllerTest (@WebMvcTest, no database):
  - Product API: "GET /products returns a page with content, page, size, totalElements and totalPages"; "GET /products/{id} returns 404 for an unknown product".
- OrderFlowIntegrationTest (@SpringBootTest against ecomstore_test, through HTTP):
  - Order flow: "a checkout saves the order, items and payment and reduces stock in the database"; "an order with too little stock saves nothing and leaves stock unchanged"; "a cancel restores stock and refunds the payment in the database"; "the catalog hides inactive products and products in inactive categories".
  - Each integration test creates the categories and products it needs and removes what it created, so the suite can run repeatedly, in any order, and never depends on the seed data. Integration tests need the MySQL container running; run the suite with the Maven wrapper from backend/.
- Keep the Initializr-generated test that checks the application context starts.
- Test report: after running the suite, write docs/TEST_REPORT.md with:
  - a summary line: tests run, passed, failed, and the date;
  - a table with one row per test group: test class, group, what it proves (one sentence), number of tests, result;
  - a table mapping each business rule in section 4 to the test groups that cover it;
  - the command to run the tests again.
- Manual checks (used in the README and the demo; run against ecomstore_db in Swagger UI, starting from the seed data):
  - Check 1: GET /api/v1/products?categoryId=2 -> 200 with the 3 books.
  - Check 2: POST /api/v1/orders with {"customer":{"email":"riya@example.com","fullName":"Riya Sen","phone":"9876543210"},"shippingAddress":"12 Park Street, Kolkata 700016","paymentMethod":"UPI","items":[{"productId":1,"quantity":1},{"productId":4,"quantity":2}]} -> 201, total 199.89, payment COMPLETED; stock of product 1 goes 25 -> 24 and product 4 goes 40 -> 38.
  - Check 3: POST /api/v1/orders with {"customer":{"email":"not-an-email","fullName":"","phone":"9876543210"},"shippingAddress":"12 Park Street, Kolkata 700016","paymentMethod":"UPI","items":[]} -> 400 with errors for customer.email, customer.fullName and items.
  - Check 4: POST /api/v1/orders with {"customer":{"email":"riya@example.com","fullName":"Riya Sen","phone":"9876543210"},"shippingAddress":"12 Park Street, Kolkata 700016","paymentMethod":"UPI","items":[{"productId":4,"quantity":1},{"productId":3,"quantity":5}]} -> 409 listing Portable Power Bank, requested 5, available 4; no stock changes.
  - Check 5: PATCH /api/v1/orders/{orderNumber from check 2}/status with {"status":"DELIVERED"} -> 409.
  - Check 6: POST /api/v1/orders/{orderNumber from check 2}/cancel -> 200, status CANCELLED, payment REFUNDED; stock back to 25 and 40.

9. NOT IN THIS VERSION
Login and registration, Spring Security/JWT, per-user order history, server-side cart, admin pages, real payments, email, image upload, Dockerizing the backend or frontend, deployment. These come in later videos. Do not add code or placeholders for them.

10. RULES
- Build exactly what this blueprint specifies: no extra features, pages, controls, endpoints, fields, files or dependencies, and use the names given here. If something is unclear, choose the simplest option that fits and list it under "Decisions" in the report.
- Never weaken validation or change test assertions just to make tests pass; fix the code or explain the problem.
- Never drop databases, delete the Docker volume or edit an applied migration without asking. The one exception: this project's MySQL data is disposable demo data, so if a container or volume from an earlier run of this project exists before the first migration run, remove it with docker compose down -v and start clean, without asking.
- No TODOs or placeholder code.
- Only report checks you actually ran, with their real results.

11. DOCUMENTATION (README.md at the project root)
Every prompt that changes the project updates README.md before its final report. Describe only what actually exists; mark later parts "Not built yet". README.md always has these sections, in this order:
- Prerequisites: JDK 25; Maven (the Maven wrapper in backend/ is used, no separate install); Docker Desktop, running; Node.js LTS with npm; an IDE such as IntelliJ IDEA. Ports 3306, 8080 and 5173 must be free.
- Status: a short checklist of what is built (blueprint, backend, frontend).
- Architecture: a small text diagram (browser -> React frontend -> Spring Boot REST API -> MySQL in Docker) with one line per part.
- Project structure: the main folders and key files, each with one line on its purpose. No list of every class.
- Run it: the exact commands to start MySQL, the backend and the frontend, for Windows PowerShell and for macOS/Linux.
- Test it yourself: how to run the automated tests and where the test report is (docs/TEST_REPORT.md); then the six manual checks from section 8, each with its request, body, expected status and expected stock change; then browser steps once the frontend exists.
- Demo data: the database credentials and what the seed data contains.
```

Check that `docs/PROJECT_BLUEPRINT.md` has sections 1–11, and that the status in `README.md` ticks only the blueprint.

### Prompt 2 — Backend

Make sure Docker Desktop is running. Claude builds the backend, runs the tests and the six manual checks, and updates `README.md`.

```text
Read docs/PROJECT_BLUEPRINT.md, then build the complete backend. Before step 1, check that Docker is running; if it is not, start Docker Desktop and wait up to two minutes for the engine. Work through the steps below in order without stopping. Never pause to ask about decisions: when the blueprint does not settle something, choose the simplest option that fits, record it under Decisions and continue. Stop only for a blocker you cannot resolve yourself, and tell me what it is. After each step, print one status line in the form "[Step N/9] <step name>: <what was created or done> - <result of its check>".

Where things go: the current folder is the project root, and the Spring Boot project is in backend/. If backend/pom.xml does not exist, stop and tell me. All Java code, tests, resources and Maven changes stay inside backend/; never create src/ or pom.xml at the project root. Only compose.yaml and docker/mysql/init/ go at the project root. Run Maven with the wrapper inside backend/.

1. Dependencies: add to backend/pom.xml what the blueprint's stack needs and the project does not have yet: Spring Data JPA, Validation, Flyway with its MySQL module, the MySQL driver, and springdoc-openapi. Use the versions managed by Spring Boot; only springdoc needs an explicit version, one compatible with this Spring Boot version. Add nothing else. The project must still compile.
2. Domain model: create the entities and enums from blueprint section 2, with the names given there.
3. Repositories: create them from section 3, with the method names given there.
4. Services: implement the business rules from section 4.
5. Controllers and API: create the DTOs, mappers, controllers and error handling from section 5. The backend must compile.
6. Database and configuration (section 7):
   - Create compose.yaml and the init script at the project root. If a MySQL container or volume from an earlier run of this project exists, remove it with docker compose down -v (blueprint section 10 allows this). Start MySQL with docker compose up -d --wait, and verify both databases exist using the mysql client inside the container.
   - Write the Flyway migrations and application properties.
   - Configure CORS and Swagger.
   - Start the backend once to confirm Flyway runs and schema validation passes.
7. Tests: write exactly the tests listed in section 8 and run all of them. Fix real defects only. Then write docs/TEST_REPORT.md as described in section 8.
8. Run and verify. Start the backend and run the six manual checks from section 8 as HTTP requests, in order, and confirm Swagger UI loads. After checks 2, 4 and 6, confirm the stock in MySQL through the container. Then delete the customers, orders, order items and payments you created in ecomstore_db, so it again holds only the seed data, and confirm the stock is back to the seed values. Stop the backend and leave MySQL running.
9. Documentation: update README.md as described in blueprint section 11, now including the backend.

Final report, under exactly these headings:
- What was built: one line per step.
- Checks run: every check you actually ran, with its real result: the build, the start against MySQL, the test suite (tests run, passed, failed) and the six manual checks.
- Decisions: anything the blueprint did not settle and how you resolved it, or "None".
- Commands: how to start MySQL and the backend, and how to run the tests.
```

Check the report (What was built, Checks run, Decisions, Commands): all tests pass, `docs/TEST_REPORT.md` lists them by business rule, and the six manual checks match their expected results. Then repeat the checks yourself in Swagger UI at http://localhost:8080/swagger-ui.html, using the "Test it yourself" section of your `README.md`.

### Prompt 3 — Frontend

Claude checks the real API against the blueprint, builds the React app in `frontend/` and updates `README.md`.

```text
Read docs/PROJECT_BLUEPRINT.md. The backend in backend/ is complete. Build the frontend. Before step 1, check that Docker is running and that Node.js and npm are installed; start Docker Desktop if needed and wait up to two minutes. Work through the steps below in order without stopping. Never pause to ask about decisions: when the blueprint does not settle something, choose the simplest option that fits, record it under Decisions and continue. Stop only for a blocker you cannot resolve yourself, and tell me what it is. After each step, print one status line in the form "[Step N/8] <step name>: <what was created or done> - <result of its check>".

Where things go: the current folder is the project root. Create the React app in frontend/ at the project root, next to backend/. All frontend files and npm commands stay inside frontend/. Do not change backend/ unless a step below requires it, and tell me if you do.

1. Start MySQL (docker compose up -d --wait, from the project root) if it is not running, then start the backend. Fetch http://localhost:8080/v3/api-docs and check that the endpoints and JSON shapes match blueprint section 5. If they differ, follow the real API and list the difference under Decisions.
2. Create frontend/ with Vite's React + TypeScript template, using non-interactive flags. Add Tailwind CSS (current official Vite setup) and React Router.
3. Build the API client, cart, components and pages from blueprint section 6, with the file names given there.
4. Create frontend/.env.example and frontend/.env.local with VITE_API_BASE_URL=http://localhost:8080.
5. Make the type check, lint and production build pass.
6. Start the frontend dev server and confirm http://localhost:5173 can call the API (CORS). You cannot click through a browser, so do not claim you did.
7. Documentation: update README.md as described in blueprint section 11, now including the frontend.
8. Stop the frontend and the backend, and leave MySQL running.

Final report, under exactly these headings:
- What was built: one line per step.
- Checks run: every check you actually ran, with its real result: type check, lint, production build, dev server and CORS.
- Decisions: anything the blueprint did not settle and how you resolved it, or "None".
- Commands: how to start MySQL, the backend and the frontend.
- Browser checklist: browse, search, category filter, add to cart, cart limit, checkout, order page, cancel, track an order, and an insufficient-stock attempt.
```

Check that the type check, lint and production build pass and CORS is confirmed. Then work through the browser checklist from the report yourself at http://localhost:5173.

## Run the Store

From the `ecomstore` folder, start MySQL:

```text
docker compose up -d --wait
```

Start the backend (Swagger UI: http://localhost:8080/swagger-ui.html):

```powershell
cd backend
.\mvnw.cmd spring-boot:run
```

On macOS/Linux, use `./mvnw spring-boot:run`.

In a second terminal, start the frontend (http://localhost:5173):

```text
cd frontend
npm install
npm run dev
```

Your project's own `README.md` has the full run and test steps.

## Troubleshooting

| Problem | Fix |
|---|---|
| Claude stops because Docker isn't running | Start Docker Desktop, wait for "Engine running", then tell Claude to continue. |
| Flyway errors, or old data from an earlier attempt | In the old project folder, run `docker compose down -v`, then start again in a new folder. |
| Maven uses the wrong Java version | Set `JAVA_HOME` to JDK 25 and restart IntelliJ IDEA. |
| "Project JDK is not defined" in IntelliJ | File → Project Structure → Project → SDK 25. |
| A port is already in use | Stop whatever uses 3306, 8080 or 5173, for example a local MySQL. |

## Finished Code

The project built in this lesson lives in its own repository, **[ecomstore-spring-boot-react](https://github.com/j2eeexpert2015/ecomstore-spring-boot-react)**. The tag [`video-1-full-stack-app`](https://github.com/j2eeexpert2015/ecomstore-spring-boot-react/tree/video-1-full-stack-app) marks the code exactly as it is at the end of this lesson.

Build your own with the prompts first. Claude's output can differ in small details from run to run, so use the repository to compare, or to run the finished store directly:

```text
git clone --branch video-1-full-stack-app https://github.com/j2eeexpert2015/ecomstore-spring-boot-react.git
cd ecomstore-spring-boot-react
docker compose up -d --wait
```

Then follow "Run it" in that repository's `README.md`.
