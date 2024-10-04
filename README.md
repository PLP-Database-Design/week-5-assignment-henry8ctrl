# Database Interacation in Web Applications

This demonstrates the cconnection of MySQL database and Node.js to create a simple API

## Requirements
- [Node.js](https://nodejs.org/) installed
-  MySQL installed and running
-  A code editor, like [Visual Studio Code](https://code.visualstudio.com/download)

## Setup
1. Clone the repository
2. Initialize the node.js environment
   ```
   npm init -y
   ```
3. Install the necessary dependancies
   ```
   npm install express mysql2 dotenv nodemon
   ```
4. Create a ``` server.js ``` and ```.env``` files
5. Basic ```server.js``` setup
   <br>
   
   ```js
   const express = require('express')
   const app = express()

   
   // Question 1 goes here


   // Question 2 goes here


   // Question 3 goes here


   // Question 4 goes here

   

   // listen to the server
   const PORT = 3000
   app.listen(PORT, () => {
     console.log(`server is runnig on http://localhost:${PORT}`)
   })
   ```
<br><br>

## Run the server
   ```
   nodemon server.js
   ```
<br><br>

## Setup the ```.env``` file
```.env
DB_USERNAME=root
DB_HOST=localhost
DB_PASSWORD=your_password
DB_NAME=hospital_db
```

<br><br>

## Configure the database connection and test the connection
Configure the ```server.js``` file to access the credentials in the ```.env``` to use them in the database connection

<br>

## 1. Retrieve all patients
Create a ```GET``` endpoint that retrieves all patients and displays their:
- ```patient_id```
- ```first_name```
- ```last_name```
- ```date_of_birth```

<br>

## 2. Retrieve all providers
Create a ```GET``` endpoint that displays all providers with their:
- ```first_name```
- ```last_name```
- ```provider_specialty```

<br>

## 3. Filter patients by First Name
Create a ```GET``` endpoint that retrieves all patients by their first name

<br>

## 4. Retrieve all providers by their specialty
Create a ```GET``` endpoint that retrieves all providers by their specialty

<br>


## NOTE: Do not fork this repository
// Initialize dependencies
const express = require('express');
const mysql = require('mysql2');
const dotenv = require('dotenv');
const cors = require('cors'); // Add this line

const app = express();
app.use(express.json());
app.use(cors());

dotenv.config();

// Connect to the database
const db = mysql.createConnection({
  host: process.env.DB_HOST,
  user: process.env.DB_USER,
  password: process.env.DB_PASSWORD,
  database: process.env.DB_NAME
});

// Check if db connection works 
db.connect((err) => {
  if (err) {
    return console.log("Error connecting to the MySQL database:", err);
  }
  console.log("Connected to MySQL successfully as id:", db.threadId);
});

// Start the server
const PORT = process.env.PORT || 3000; // Use environment variable or default to 3000
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});

// Send a message to the browser
app.get('/', (req, res) => {
  res.send('Server started successfully! Wedding can go ON!!!');
});

// Endpoint to get patients
app.get('/patients', (req, res) => {
  const query = 'SELECT patient_id, first_name, last_name, date_of_birth FROM patients';

  db.query(query, (err, results) => {
    if (err) {
      res.status(500).json({ error: 'Failed to retrieve patients' });
      return;
    }
    res.json(results);
  });
});

// Endpoint to get all providers
app.get('/providers', (req, res) => {
  const query = 'SELECT first_name, last_name, provider_specialty FROM providers';

  db.query(query, (err, results) => {
    if (err) {
      // If there's an error, send a 500 status with an error message
      res.status(500).json({ error: 'Failed to retrieve providers' });
      return;
    }
    // Send the results as a JSON response
    res.json(results);
  });
});

// Endpoint to filter patients by first name
app.get('/patients/filter', (req, res) => {
  const firstName = req.query.first_name; 

  if (!firstName) {
    return res.status(400).json({ error: 'First name is required' });
  }

  const query = 'SELECT patient_id, first_name, last_name, date_of_birth FROM patients WHERE first_name = ?';

  db.query(query, [firstName], (err, results) => {
    if (err) {
      return res.status(500).json({ error: 'Failed to retrieve patients' });
    }
    res.json(results);
  });
});
// Endpoint to filter providers by specialty
app.get('/providers/filter', (req, res) => {
  const specialty = req.query.specialty;

  if (!specialty) {
    return res.status(400).json({ error: 'Specialty is required' });
  }

  const query = 'SELECT first_name, last_name, provider_specialty FROM providers WHERE provider_specialty = ?';

  db.query(query, [specialty], (err, results) => {
    if (err) {
      return res.status(500).json({ error: 'Failed to retrieve providers' });
    }
    res.json(results);
  });
});



