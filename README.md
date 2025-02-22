# Full Stack Web Development Assignment: Backend Development

### Objective

Your task is to create a simple backend server with a GET and a POST route. You are encouraged to use AI tools like ChatGPT to help you get the backend up and running quickly. However, at each step, you must answer assessment questions to demonstrate your understanding.

### Prerequisites

- Node.js installed

- PostgreSQL installed and running

- Basic understanding of JavaScript and Express

### Instructions

**1. Set Up Your Project**

1. Create a new directory for your backend project and initialize a Node.js project:

   ```
   mkdir simple-backend && cd simple-backend
   npm init -y
   ```

2. Install required dependencies:

   ```
   npm install express cors pg-promise dotenv
   ```

3. Create the following files in your project:

   - `server.js`

   - `.env`

   - `db/config.js`

**<small>Checkpoint 1: Understanding Project Setup</small>**

- What is the purpose of npm init -y?

- Why do we need express, cors, pg-promise, and dotenv?

- What does the .env file do?

---

**2. Configure the Database**

1. In `.env`, define the database connection parameters:

   ```
   PG_HOST=localhost
   PG_PORT=5432
   PG_DATABASE=your_database
   PG_USER=your_user
   PG_PASSWORD=your_password
   ```

2. In `db/config.js`, set up the database connection:

   ```
   const pgp = require("pg-promise")();
   require("dotenv").config();

   const { DATABASE_URL, PG_HOST, PG_PORT, PG_DATABASE, PG_USER, PG_PASSWORD } = process.env;

   const cn = DATABASE_URL
     ? {
     connectionString: DATABASE_URL,
     max: 30,
     ssl: {
     rejectUnauthorized: false,
     },
     }
     : {
     host: PG_HOST,
     port: PG_PORT,
     database: PG_DATABASE,
     user: PG_USER,
     password: PG_PASSWORD,
     };

   const db = pgp(cn);

   module.exports = db;
   ```

**<small>Checkpoint 2: Understanding Database Setup</small>**

- What does `pg-promise` do?

- Why do we use an object to configure the database connection instead of a single connection string?

---

**3. Create the Express Server**

1. In `server.js`, set up an Express server:

   ```
   const express = require("express");
   const cors = require("cors");
   const db = require("./db/config");

   const app = express();
   app.use(cors());
   app.use(express.json());

   app.listen(3000, () => {
     console.log("Server running on port 3000");
   });
   ```

**<small>Checkpoint 3: Understanding Express Server</small>**

- What does `app.use(cors())` do?

- Why do we need `express.json()`?

- How can you change the port number of the server?

---

**4. Implement GET and POST Routes**

1. Add a `GET` route to retrieve all items:

   ```
   app.get("/items", async (req, res) => {
     try {
       const result = await db.any("SELECT \* FROM items");
       res.json(result);
     } catch (err) {
       console.error(err.message);
       res.status(500).send("Server Error");
     }
   });
   ```

2. Add a `POST` route to add a new item:

   ```
   app.post("/items", async (req, res) => {
     try {
       const { name } = req.body;
       const result = await db.one(
         "INSERT INTO items (name) VALUES ($1) RETURNING \*",
         [name]
       );
       res.json(result);
     } catch (err) {
       console.error(err.message);
       res.status(500).send("Server Error");
     }
   });
   ```

**<small>Checkpoint 4: Understanding Routes</small>**

- What is the difference between `GET` and `POST` requests?

- What does `$1` do in the SQL query?

- What happens if a request is made to the `POST` route without a body?

---

**5. Test Your API**

1. Run the server:

   ```
   node server.js
   ```

2. Use Postman or `curl` to test the endpoints:

   ```
   curl -X GET http://localhost:3000/items

   curl -X POST http://localhost:3000/items -H "Content-Type: application/json" -d '{"name": "Sample Item"}'
   ```

**<small>Checkpoint 5: Understanding API Testing</small>**

- How can you test the `GET` route using Postman?

- What is the purpose of the `-H` and `-d` flags in the `curl` command?

- What should you expect in the response when adding an item successfully?

---

**6. Submit Your Work**

- Push your code to a GitHub repository and submit the link.

- Answer all the checkpoint questions in a separate `answers.md` file.

**Bonus**

- Add an `UPDATE` and `DELETE` route.

- Use AI tools like ChatGPT to refine error handling.

- Deploy the backend using Render or Railway.

---

**<small>Evaluation Criteria</small>**

- **<small style="font-size: 13px">Functionality:</small>** The backend runs and the routes work as expected.

- **<small style="font-size: 13px">Understanding:</small>** Clear and correct answers to checkpoint questions.

- **<small style="font-size: 13px">Code Quality:</small>** Clean and organized code with proper error handling.

Good luck, and happy coding!
