# Lecture 2 - Flask

## Python
- Indentation is necessary 
- "Live code" - just type `python` to terminal

### Interpreter
`src $ python hello.py`

### f strings
``` python
name = input()
print(f"hello, {name}!")
```
### Variables
- integer, float, boolean (`True`/ `False`), `None`

### Conditions
- if, else, elif

### Sequences
- string - `name = "Alice"`
- tuple - `coordinates = (10.0, 20.0)`
- list - `names = ["Alice", 12, "Bob"]`
<br>
- Indexing 
  - `name[0] = 'A'`
  - `name[5]` - IndexError

### Loops
```python
for i in range(5):  # not including 5
    print(i)
```

```python
names = ["Alice", "Bob", "Charlie"]
for name in names:  
    print(name)
```
**Output:** <br>
Alice  <br>
Bob  <br>
Charlie <br>

### Sets
- unique values 
- no order

```python
s = set()
s.add(1)
s.add(3)
s.add(3)
```

**Output:** <br>
`s = {1, 3}`

### Dictionaries
``` python
ages = {"Alice": 22, "Bob": 27}
ages["Charlie"] = 30
ages["Alice"] += 1
```

**Output:** <br>
`ages = {"Alice": 23, "Bob": 27, "Charlie": 30}`

### Functions
- Functions should be declared before using it
``` python
# function v1
def square(x):
    return x ** 2

for i in range(10):
    print("{} squared is {}".format(i, square(i)))
```

### Modules
- add `def main()` and `if __name__ == "__main__"` part to avoid printing out the for loop while calling the square function
``` python
# function v2
def square(x):
    return x ** 2

def main():
    for i in range(10):
        print("{} squared is {}".format(i, square(i)))

if __name__ == "__main__":
    main()
```

``` python
from functions import square  # sqaure from functions.py

print(square(10))
```
- In function v1, the output will include 1 squared is 1, 2 squared is 4... then only print 100
- In function v2, only 100 is printed out

### Classes
``` python 
class Point:
    def __init__(self, x, y):
        self.x = x    # access a particular attribute
        self.y = y

p = Point(3, 5)  # Point is an object
print(p.x)
print(p.y)
```

## Flask
- Micro framework written in Python
- Run the flask application by typing `flask run` in the directory
- If running for the first time, run `export FLASK_APP = application.py`

``` python
# application.py

from flask import Flask

app = Flask(__name__) # Create a new Flask web application

 @app.route("/") # `/` represents the default page
 def index():
    return "Hello, world!"
```

### Routes
``` python
# Adding a new route

from flask import Flask

app = Flask(__name__) 

 @app.route("/")
 def index():
    return "Hello, world!"

@app.route("/david")  # new route, add /david on URL
 def david():
    return "Hello, david!"
```

``` python
# Generalised routes

from flask import Flask

app = Flask(__name__) 

 @app.route("/")
 def index():
    return "Hello, world!"

@app.route("/<string:name>") # Dynamic and can take any string
 def hello(name):
    name = name.capitalize()
    return f"Hello, {name}!"
```

- Can add HTML elements, eg. `return f"<h1>Hello, {name}!</h1>"`

### Templates
``` python
# Display templates (HTML files) instead of Python strings

from flask import Flask, render_template

app = Flask(__name__) application

 @app.route("/")
 def index():
    return render_template("index.html") # Take an index.html file and reder that to display to the user
```

```
templates
└── index.html       
│  
└ application.py
```

### Variables 
``` python
# Adding variables

from flask import Flask, render_template

app = Flask(__name__) application

 @app.route("/")
 def index():
    headline = "Hello, world!"
    return render_template("index.html", headline = headline) 
    # Let index.html know about 'headline' variable,  headline (HTML file) = headline (Python file)

@app.route("/bye")
def bye():
    headline = "Goodbye!"
    return render_template("index.html", headline = headline) 
```
``` HTML
<!-- index.html -->
<html>
    <head>
        <title>Website</title>
    </head>
    <body>
        <h1>{{ headline }}</h1> <!-- Jinja 2 placeholder -->
    </body>
</html>
```

### Conditions
``` Python
# application.py
import datetime

from flask import Flask, render_template

app = Flask(__name__)

@app.route("/")
def index():
    now = datetime.datetime.now() # Get current date and time
    new_year = now.month == 1 and now.day == 1
    return render_template("index.html", new_year = new_year)
```

``` HTML
<!-- index.html -->
<html>
    <head>
        <title>Is it New Year?</title>
        <style>
            body {
                text-align: center;
            }
        </style>
    </head>
    <body>
        {% if new_year %}
            <h1>Yes! Happy New Year</h1> 
        {% else %}
            <h1>NO</h1>
        {% endif %}
    </body>
</html>
```

### Loops
``` Python
# Using for loops
from flask import Flask, render_template

app = Flask(__name__)

@app.route("/")
def index():
    names = ["Alice", "Bob", "Charlie"]
    return render_template("index.html", names = names)
```

``` HTML
<!-- index.html -->
<html>
    <head>
        <title>Website</title>
    </head>
    <body>
        <h1>Names</h1>
        <ul>
            {% for name in names %}
                <li>{{ name }}</li>  <!-- Dynamically generate list item -->
            {% endfor %}
        </ul>
    </body>
</html>
```

### Links 
``` Python
# Linking to different routes
from flask import Flask, render_template

app = Flask(__name__)

@app.route("/")
def index():
    return render_template("index.html")

@app.route("/more")
def more():
    return render_template("more.html")
```

``` HTML
<!-- index.html -->
<html>
    <head>
        <title>Website</title>
    </head>
    <body>
        <h1>First Page</h1>
        <p>This is a paragraph</p>
        <a href = "{{ url_for('more') }}">See more...</a> <!-- more is the name of the python function -->
    </body>
</html>
```

``` Jinja
<!-- more.html -->
<html>
    <head>
        <title>Website</title>
    </head>
    <body>
        <h1>Second Page</h1>
        <p>This is another paragraph</p>
        <a href = "{{ url_for('index') }}">Go back</a> <!-- index is the name of the python function -->
    </body>
</html>
```

### Inheritence 
- Share the same layour across different webpages
```
templates
└── index.html  
└── layout.html  
└── more.html       
│  
└ application.py
```

``` Jinja
<!-- layout.html -->
<html>
    <head>
        <title>Website</title>
    </head>
    <body>
        <h1>{% block heading %}{% endblock %}</h1>
        {% block body %}
        {% endblock %}
    </body>
</html>
```

``` Jinja
<!-- index.html -->
{% extends "layout.html" %}

{% block heading %}
    First Page
{% endblock %}

{% block body %}
    <p>This is a pragraph</p>
    <a href = "{{ url_for('more') }}">See more...</a>
{% endblock %}
```

### Forms
``` Python
# Getting information from forms
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route("/")
def index():
    return render_template("index.html")

@app.route("/hello", methods = ["POST"]) # User will be submitting data to this route via POST method
def hello():
    name = request.form.get("name")
    return render_template("hello.html", name = name)
```

``` Jinja
<!-- index.html -->
{% extends "layout.html" %}

{% block heading %}
    First Page
{% endblock %}

{% block body %}
    <form action = "{{ url_for('hello') }}" method = "post"> 
    <!-- Use post request method to submit data as a form to the web server -->
        <input type = "text" name = "name" placeholder = "Enter your name">
        <button>Submit</button>
    <form>
{% endblock %}
```

``` Jinja
<!-- hello.html -->
{% extends "layout.html" %}

{% block heading %}
    Hello!
{% endblock %}

{% block body %}
    Hello, {{ name }}!
{% endblock %}
```

- If you go to /hello, you will get an error saying Method Not Allowed. This is because in our Flask code, we only allowed for the post method, `methods = ["POST"]`
- By adding the get method, users can then access the /hello webpage
``` Python
# Getting information from forms
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route("/")
def index():
    return render_template("index.html")

@app.route("/hello", methods = ["GET", "POST"]) # Accept GET requests as well
def hello():
    if request.method == "GET":
        return "Please submit the form :("
    else: 
        name = request.form.get("name")
        return render_template("hello.html", name = name)
```

*Note: you can also have data submitted via a GET request, but the data will be put into the URL, hence it may not be suitable for getting sensitive information such as passwords*

### Sessions
- Access to data that is specific to your own account
- Store, retain and use information

``` Python
# Store information (notes)
from flask import Flask, render_template, request

app = Flask(__name__)

# List to store notes (global variable - shared accross the web server)
notes = []

@app.route("/")
def index():
    # Get note and add it to the notes list
    if request.method == "POST":
        note = request.form.get("note")
        notes.append(note)

    return render_template("index.html", notes = notes)
```

```Jinja
{% extends "layout.html" %}

{% block heading %}
    Notes
{% endblock %}

{% block body %}
    <ul>
        {% for note in notes %}
            <li>{{ note }}</li>
        {% endfor %}
    </ul>

    <!-- Submit data back to index.html -->
    <form action = "{{ url_for('index') }}" method = "post">
        <input type = "text" name = "note" placeholder = "Enter note here">
        <button>Add note</button>
    </form>
{% endblock %}
```
*Note: If you close the webpage and repoen it again, the notes that you have previously added to the webpage will still be there. However, if you restart the Flask server,your data will be gone.*

- When different people login to the website, they only want to see their own notes. Hence, sessions can prevent the overlapping of notes.
- Remove the global variable of notes and create a new local variable of session notes

``` Python
# Session - keep variables and values for a particular user
from flask import Flask, render_template, request, session
from flask_session import Session

app = Flask(__name__)

app.config["SESSION_PERMANENT"] = False
app.config["SESSION_TYPE"] = "filesystem"
Session(app)

@app.route("/")
def index():
    # If notes is empty - adding an initial note, else just keep it as it is
    if session.get("notes") is None:
        session["notes"] = []
    # Dictionry - User's particular session will have an empty note 
    if request.method == "POST":
        note = request.form.get("note")
        session["notes"].append(note)

    return render_template("index.html", notes = session["notes"])
```
