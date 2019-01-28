# Lecture 4 - ORM and API

## Object-Oriented Programming
### Python Classes
``` Python
# classes1.py

class Flight:
    def __init__(self, origin, destination, duration) #init method, what happens when we want to fisrt create this class
        self.origin = origin # self refers to the object we are working with
        self.destination = destination
        self.duration = duration

def main():
    # Create flight
    f = Flight(origin = "New York", destination = "Paris", duration = 540)

    # Change the value of a variable
    f.duration += 10

    # Print details about flight
    print(f.origin)
    print(f.destination)
    print(f.duration)

if __name__ == "__main__":
    main()
```

- What's the point of `if __name__ == "__main__": main()`?
  - Python will execute code from top to bottom
  - When running the file, we want Python to run the main function
  -  Without the `if` statement and just having the `main()` - if we were to import this file to another file (ie. use the 'flight' class in another file), the other file will execute the main function and print the flight details, which we would not want because we only need the flight class 

``` Python
# classes2.py - print method

class Flight:
    def __init__(self, origin, destination, duration) 
        self.origin = origin 
        self.destination = destination
        self.duration = duration

    def print_info(self):
        print(f"Flight origin: {self.origin}")
        print(f"Flight destination: {self.destination}")
        print(f"Flight duration: {self.duration}")

def main():
    f1 = Flight(origin = "New York", destination = "Paris", duration = 540)
    f1.print_info()

    f2 = Flight(origin = "Tokyo", destination = "Shanghai", duration = 185)
    f2.print_info()

if __name__ == "__main__":
    main()
```

- Can just write `f1 = Flight("New York", "Paris", duration)` as it is in order

``` Python
# classes3.py - delay methid

class Flight:
        self.origin = origin 
        self.destination = destination
        self.duration = duration

    def print_info(self):
        print(f"Flight origin: {self.origin}")
        print(f"Flight destination: {self.destination}")
        print(f"Flight duration: {self.duration}")

    def delay(self, amount):
        self.duration += amount

def main():
    f1 = Flight(origin = "New York", destination = "Paris", duration = 540)
    f1.delay(10)
    f1.print_info()

if __name__ == "__main__":
    main()
```

``` Python
# classes4.py - counter and passengers

class Flight:

    counter = 1

    def __init__(self, origin, destination, duration) 
        
        # Keep track of id number
        self.id = Flight.counter # Set id to be the counter
        Flight.counter += 1 # Keep incrememnting id when creating a new flight

        # Keep track of passengers
        self.passengers = []
        
        # Details about flight
        self.origin = origin 
        self.destination = destination
        self.duration = duration

    def print_info(self):
        print(f"Flight origin: {self.origin}")
        print(f"Flight destination: {self.destination}")
        print(f"Flight duration: {self.duration}")

        print()
        print("Passengers:")
        for passenger in self.passengers:
            print("f"{passenger.name}")

    def delay(self, amount):
        self.duration += amount

    def add_passenger(self, p): # self = Flight
        self.passengers.append(p) # Add passenger to list
        p.flight_id = self.id 

class Passenger:

    def __init__(self, name):
        self.name = name


def main():

    # Create flight
    f1 = Flight(origin = "New York", destination = "Paris", duration = 540)
    
    # Create passengers object
    alice = Passenger(name="Alice")
    bob = Passenger(name="Bob")

    # Add passengers - appends to list and update flight id for alice and bob's flight id
    f1.add_passenger(alice)
    f1.add_passenger(bob)

if __name__ == "__main__":
    main()
```

## Object-Relational Mapping (ORM)
- Allow us to use Python classes, methods and objects to interact with a SQL database

### Flask- SQLAlchemy
- Package that ties SQLAlchemy with Flask

``` Python
# models.py

from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy()

class Flight(db.Model):
    __tablename__ = "flights"
    id = db.Column(db.Integer, primary_key = True)
    origin = db.Column(db.String, nullable = False) # equivalent to NOT NULL
    destination = db.Column(db.String, nullable = False)
    duration = db.Column(db.Integer, nullable = False)

class Passenger(db.Model):
    __tablename__ = "passengers"
    id = db.Column(db.Integer, primary_key = True)
```

``` Python
# create.py

import os

from flask import Flask, render_template, request
from models import * # the previous file containing all the classes etc. - import everthing

app = Flask(__name__)
app.config["SQLALCHEMY_DATABASE_URI"] = os.getenv("DATABASE_URL")
app.config["SQLALCHEMY_TRACK_MODIFICATIONS"] = False
db.init_app(app) # Tie this database with this flask application

def main():
    db.create_all() # Create tables based on models.py

if __name__ == "__main__":
    with app.app_context(): # Allow us to use command line to interact with Flask
        main()
```
- `db.create_all()` allows us to create a SQL table

**Stopped at 40m**