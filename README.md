# Cookie Jar: Cookie storefront with cart and WhatsApp checkout

Cookie Jar is a storefront for De Mala e Cuia, a family-run cookie business. The team combined HTML, CSS, and JavaScript with a Node.js/Express backend and PostgreSQL to connect the product catalogue, shopping cart, and an order flow that prepares a WhatsApp message.

## 1. Academic context and scope

- **Institution:** Universidade Federal de Minas Gerais (UFMG)
- **Degree:** Ciência da Computação
- **Course:** Engenharia de Software 1
- **Assignment:** Trabalho Prático 1
- **Semester:** 2026-01
- **Collaboration:** Team

The project applies software-engineering practices to a practical sales application. The cart supports adding and removing products, quantities in grams, and subtotal, estimated shipping, and total calculations. Checkout saves an order and prepares a message for the customer to review and send.

## 2. How to run

Use Node.js, npm, and an existing PostgreSQL database. No Node.js version is pinned in `package.json`. Supply `DATABASE_URL`, or configure `PGHOST`, `PGPORT`, `PGUSER`, `PGPASSWORD`, and `PGDATABASE` for the chosen database through the environment or a local `.env` file.

From the repository root:

```sh
npm ci
npm start
```

Startup initializes the tables and seeds the catalogue when needed. The Express application serves the frontend and `/api` routes together. Open `http://localhost:3000/`; the server accepts a different port through `PORT`.

## 3. Usage example

Select a catalogue product, open the cart, adjust its quantity in grams, and inspect the calculated totals. Submit checkout to create an order through `POST /api/orders`. The frontend uses the server's returned items and totals to prepare a WhatsApp draft.

The customer reviews and sends that message in WhatsApp. Opening the draft does not demonstrate automatic message delivery, payment processing, or order confirmation. The current integration limitation described below means this flow should be checked before relying on it end to end.

## 4. Implementation decisions

- Browser modules separate cart state, checkout, and interface rendering. The current cart is maintained in memory.
- The order service validates submitted items and positive integer quantities, retrieves current active catalogue prices, and calculates values on the server.
- Order and line-item inserts share a PostgreSQL transaction, with rollback on an exception.
- Checkout formats the message from the saved order response, connecting the displayed order with server-calculated prices.

## 5. Tests and evaluation

Jest tests cover cart operations and totals, checkout requests and errors, message formatting, and order validation. Backend tests use mocked database/service boundaries and Supertest; Playwright declares browser scenarios for cart behavior and the WhatsApp popup, alongside other application flows.

The package exposes these commands:

```sh
npm test
npm run test:e2e
```

The browser suite needs its Playwright browser environment and configured PostgreSQL connection. Test declarations and a coverage command do not establish passing results or the documented coverage target.

## 6. Learning and limitations

User stories helped turn the need to build a cart into concrete behaviors, implementation responsibilities, and tests. UML modeling and a layered backend clarified integration points.

Static review identified a cart integration mismatch: `index.js` passes a numeric ID to `addToCart`, which expects a product object, while `ui.js` has a separate wrapper. End-to-end behavior therefore needs verification. Current modules do not establish persistent browser cart storage or automatic cart clearing after checkout.

## 7. Authorship and contribution

Developed as a team project. [João Vaz](https://github.com/jg2alv) implemented the cart across frontend and backend, developed WhatsApp checkout, and wrote tests for both features. The catalogue, administration, architecture, and documentation are not attributed solely to João.

<!--
### Team members

-->
