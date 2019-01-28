# Lecture 3 - SQL

## Databases
- Relational database: store data into a table
- SQL - structured query language

## PostgreSQL
- Need to start a PostgreSQL local server to use the database or use online platforms like Heroku
- `\d` shows the parts of the database 
- Data types: 
  - INTEGER
  - SERIAL (similar to int but counts automatically)
  - VARCHAR (variable length of character)
  - TIMESTAMP (date and time)
  - BOOLEAN (true/false)
  - ENUM (one of finite discrete numbers)

### Create table
``` SQL
-- Need to let the database know in advance what data types will be contained in the table
CREATE TABLE flights (
    id SERIAL PRIMARY KEY,  -- id can be used for referencing (eg. flight 28) -- primary key: primary way to reference a flight
    origin VARCHAR NOT NULL,  -- varchar: name of text  -- specify other property: NOT NULL (origin column can't be empty)
    destination VARCHAR NOT NULL,
    duration INTEGER NOT NULL
);
```

### Constraints
- NOT NULL - needs to have a value
- UNIQUE (eg. username)
- PRIMARY KEY - primary way to reference the table
- DEFAULT - give column a default value 
- CHECK (eg. only allow values more than 100)

### Insert data
- `INSERT INTO <table name>` - add data into the table
- `(origin, destination, duration)` - names of columns you want to add information for
``` SQL
INSERT INTO flights (origin, destination, duration) VALUES ('New York', 'London' 415);
```

### Reading from database 
- `SELECT` query
``` SQL
SELECT * FROM flights; -- select everything from the flights table
```

| id| origin        | destination  | duration |
|---|:-------------:| -------------|----------|
| 1 | New York      | London       | 415      |
| 2 | Shanghai      | Paris        | 760      |
| 3 | Istanbul      | Tokyo        | 700      |

- select certain **columns** only - `SELECT origin, destination FROM flights ` 

- select particular **rows** - `SELECT * FROM flights WHERE id = 3;`

- using conditions - `SELECT * FROM flights WHERE duration > 500;`

- using booleans - `SELECT * FROM flights WHERE destination = 'PARIS' AND duration > 500;`

### SQL functions
- SUM, COUNT, MIN, MAX, AVG...
- AVG - `SELECT AVG(duration) FROM flights WHERE origin = 'New York';`
- COUNT - `SELECT COUNT(*) FROM flights;` (how many rows are in the table)
- IN - `SELECT * FROM flights WHERE origin IN ('New York', 'Lima');`
- LIKE - `SELECT * FROM flights WHERE origin LIKE '%a%';` (find items that have an 'a' in it)

### UPDATE
- modify data in database
``` SQL
UPDATE flights -- table name
    SET duration = 430
    WHERE origin = 'New York' 
    AND destination = 'London';
    -- only update duration of flights which have origin New York and destination London
```

### DELETE
``` SQL
DELETE FROM flights
    WHERE duration = 430; 
```
- If table has id (SERIAL) and it gets deleted, the ids of other data remains the same (won't shift upwards)

### LIMIT
- Only show a number of rows 
``` SQL
SELECT * FROM flights LIMIT 2;
```

### ORDER BY 
- `ASC`, `DESC`
``` SQL
SELECT * FROM flights ORDER BY duration ASC LIMIT 3; -- ascending duration order
```

### GROUP BY
``` SQL
SELECT origin, COUNT(*) FROM flights GROUP BY origin; 
-- count the number of flights of each origin
```

### HAVING
- Similar to `WHERE`, but followed by `GROUP BY`
``` SQL
SELECT origin, COUNT(*) FROM flights GROUP BY origin HAVING COUNT(*) > 1; -- count the number of flights of each origin, but only display the ones which have > 1 count
```

### Foreign Key
- Connect multiple databases together
- Reference a key from another table
- Can keep things organised, save space

``` SQL
CREATE TABLE passengers (
    id SERIAL PRIMARY KEY,
    name VARCHAR NOT NULL,
    flight_id INTEGER REFERENCES flights -- reference the flights table's ID
);
```

### Join
**Inner join**
- After adding passenger details, if we want to find what their flight number is: 
``` SQL
SELECT * FROM passengers WHERE name = 'Alice';
```

**Output:** 

| id| name          | flight_id  | 
|---|:-------------:|------------|
| 1 | Alice         | 1          |

``` SQL
SELECT * FROM flights WHERE id = 1;
```

**Output:** 

| id| origin        | destination| duration |
|---|:-------------:|------------|----------|
| 1 | New York      | London     | 415      |

``` SQL
SELECT origin, destination, name FROM flights JOIN passengers ON passengers.flight_id = flights.id where NAME = 'Alice'
-- tell the query how the two tables are related
-- flight_id should match the primary key id for the flights table
```
**Left join**
- Take table on the left and make sure that all of the row on the table are included in the final result even though there are no matches to the right table

### INDEX
- Create index on individual column or row
- Speed up when want to select something from table
- Creating index for evrey column or row would be time consuming (slows down updating or inserting data) and waste space

### Nested queries
``` SQL
-- Group passengers based on their flight id and select the flight_id if it has more than 1 passenger 
SELECT flight_id FROM passengers GROUP BY flight_id HAVING COUNT(*) > 1;
```
| id| name          | flight_id  | 
|---|:-------------:| -----------|
| 1 | Alice         | 1          |
| 2 | Bob           | 1          |
| 3 | Charlie       | 2          |
| 4 | Dave          | 2          |
| 5 | Eric          | 4          |
| 6 | Frank         | 6          |
| 7 | Grace         | 6          |

**Output:** 

|flight_id |
|----------|
|1         |
|2         |
|3         |

``` SQL
-- Select all of the flights that have multiple passengers
SELECT * FROM flights WHERE id IN (SELECT flight_id FROM passengers GROUP BY flight_id HAVING COUNT(*) > 1;)
```

**Output:**


| id| origin        | destination  | duration |
|---|:-------------:| -------------|----------|
| 1 | New York      | London       | 415      |
| 2 | Shanghai      | Paris        | 760      |
| 6 | Lima          | New York     | 455      |


### SQL Injection
- Code injection technique that may be used by hackers

```SQL
SELECT * FROM users
    WHERE (username = <username>) -- <username> is what the user typed in 
    AND (password = <password>); -- theoratically passwords won't be stored like this (it'll use hash instead)
```
- Instead of typing the password, the user gives an SQL statement that will be run on the database
- `'1' = '1'` will always be true, hence SQL will return all rows from the password table
```SQL
-- Example: 
SELECT * FROM users
    WHERE (username = Alice) 
    AND (password = '1' OR '1' = '1'); 
```

- Solution to this: use escape characters of the input


### Race Conditions
- When database accessed by multiple people, and they try to do things at the same time to the database
- Two people trying to Withdraw $100 from the same bank account:
``` SQL
SELECT balance FROM bank 
    WHERE user_id = 1;

-- Update balance to subtract $100 - if two people withdraw $100 concurrently, the bank balance will be -$100
UPDATE bank
    SET balance = balance - 100
    WHERE user_id = 1;
```

- Solution to this: lock the transaction
- SQL Transactions - `BEGIN`, `COMMIT`

## SQLAlchemy
- Connect SQL and Python, allows Python to run SQL queries

``` Python
#list.py 
import os

from sqlalchemy import create_engine
from sqlalchemy.orm import scoped_session, sessionmaker

# Create a database engine (an object from sqlalchemy) to manage connections to the database
engine = create_engine(os.getenv("DATABASE_URL")) # DATABASE_URL is an environment variable
# Create sessions for different people
db = scoped_session(sessionmaker(bind = engine))

def main():
    flights = db.execute("SELECT origin, destination, duration FROM flights").fetchall() # fetchall - run the query and get all the results 
    for flight in flights:
        print(f"{flight.origin} to {flight.destination}, {flight.duration} minutes.")
```

### Importing CSV files into SQL table
``` Python 
import csv
import os

from sqlalchemy import create_engine
from sqlalchemy.orm import scoped_session, sessionmaker

engine = create_engine(os.getenv("DATABSE_URL"))
db = scoped_session(sessionmaker(bind = engine))

def main():
    f = open("flights.csv")
    reader = csv.reader(f) # Read file f as a csv file
    for ori, dest, dur in reader:
        db.execute("INSERT INTO flights(origin, destination, destination) VALUES (:origin, :destination, :duration)", 
        {"origin": ori, "destination": dest, "duration": dur)
    db.commit() # Tell the database to save the changes 
        
```
- :origin - placeholder for the origin
- `{"origin": ori, ...}` dictionary to tell the query what to fill in into the placeholder


### Flask & SQL
``` Python
import os

from flask import Flask, render_template, request
from sqlalchemy import create_engine
from sqlalchemy.orm import scoped_session, sessionmaker

app = Flask(__name__)

engine = create_engine(os.getenv("DATABASE_URL"))
db = scoped_session(sessionmaker(bind=engine))

@app.route("/")
def index():
    flights = db.execute("SELECT * FROM flights").fetchall()
    return render_template("index.html", flights=flights)

@app.route("/book", methods=["POST"])
def book():
    """Book a flight."""

    # Get form information.
    name = request.form.get("name")
    try:
        flight_id = int(request.form.get("flight_id"))
    # Handle the error to avoid Python crashing and giving the user an error
    except ValueError:
        return render_template("error.html", message="Invalid flight number.")

    # Make sure the flight exists.
    if db.execute("SELECT * FROM flights WHERE id = :id", {"id": flight_id}).rowcount == 0:
        return render_template("error.html", message="No such flight with that id.")
    db.execute("INSERT INTO passengers (name, flight_id) VALUES (:name, :flight_id)",
            {"name": name, "flight_id": flight_id})
    db.commit()
    return render_template("success.html")
```
