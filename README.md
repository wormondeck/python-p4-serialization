# Flask-SQLAlchemy Lab

## Learning Goals

- Build and run a Flask application on your computer.
- Extend a Flask application to meet the unique requirements of different
  projects.

***

## Key Vocab

- **Web Framework**: software that is designed to support the development of
  web applications. Web frameworks provide built-in tools for generating web
  servers, turning Python objects into HTML, and more.
- **Extension**: a package or module that adds functionality to a Flask
  application that it does not have by default.
- **Request**: an attempt by one machine to contact another over the internet.
- **Client**: an application or machine that accesses services being provided
  by a server through the internet.
- **Web Server**: a combination of software and hardware that uses Hypertext
  Transfer Protocol (HTTP) and other protocols to respond to requests made
  over the internet.
- **Web Server Gateway Interface (WSGI)**: an interface between web servers
  and applications.
- **Template Engine**: software that takes in strings with tokenized
  values, replacing the tokens with their values as output in a web browser.

***

## Introduction

Serialization is a set of actions to converting data into a format that can be
easily shared between different computers, phones, or programs. Imagine you have
a cool video game that you want to play with your friends, but they live in a
different part of the world. Serialization allows you to share the game data
with them, so they can play the game on their own computer, even though they are
far away. It's like packing up your video game in a suitcase and sending it to
your friend!

SQLAlchemy-Serializer is a powerful tool for serializing data in Python using
the SQLAlchemy ORM. It provides an easy and efficient way to convert
SQLAlchemy database models into JSON or other data formats. With this tool, you
can easily transform complex database models into simpler representations that
can be easily shared or transmitted between different systems.
SQLAlchemy-Serializer offers a range of features, including customization of
serialization rules, filtering of unwanted data, and support for nested models.
It's a simple serializer compared to many others, but a great choice for
developers who want to simplify their data serialization process and improve
their application's performance.

In this lesson, we will explore how to serialize your SQLAlchemy data with
SQLAlchemy-Serializer so it can be easily interpreted by React frontends and
external applications.

***

## Setting up SQLAlchemy-Serializer

We've set up this repo with the solution code from the Flask-SQLAlchemy lab.
Run `pipenv install; pipenv shell` to create and enter your virtual environment.
Navigate to the `server/` directory and run `flask db upgrade head` to generate
your database, then `python seed.py` to seed it with data. Our serializer will
affect the visualization of our data, but it will not impact the database; we
won't need to touch Flask-Migrate again in this lesson.

### `SerializerMixin`

Navigate to `models.py` and you'll notice a new import at the top:

```py
# models.py
from sqlalchemy_serializer import SerializerMixin
```

The SerializerMixin class in SQLAlchemy-Serializer is a helpful feature that
allows developers to quickly add serialization capabilities to their SQLAlchemy
models. When a model class inherits from the SerializerMixin, it gains a range
of methods for serializing and deserializing data. These methods include
`to_dict()`, which converts the model object into a dictionary, and `to_json()`,
which converts it into a JSON string.

In short, the SerializerMixin simplifies the process of data serialization by
adding a set of predefined methods to SQLAlchemy models. Developers can
customize these methods as needed to achieve their desired serialization format,
and can use them to quickly transform complex database models into simpler, more
usable data structures. Most languages can't work with Python objects, after
all.

### Configuring our Models for Serialization

In `models.py`, we'll need to reconfigure each of our models to inherit from
`SerializerMixin`. Don't worry though- this only requires a small amount of new
code, and we won't have to run new migrations afterward.

```py
# models.py
# imports, config

class Zookeeper(db.Model, SerializerMixin):
    ...

class Enclosure(db.Model, SerializerMixin):
    ...

class Animal(db.Model, SerializerMixin):
    ...
```

By now you should have created your database and run `seed.py`; if you haven't
yet, do that now!

Once you have a populated database, navigate to the `server/` directory and
enter `flask shell` to start manipulating our models. Import all of your models
and retrieve a `Zookeeper` record. Let's run its brand new method, `to_dict()`:

```console
$ from models import *
$ z1 = Zookeeper.query.first()
$ z1.to_dict()
# => RecursionError: maximum recursion depth exceeded in comparison
```

While this isn't _quite_ what we were looking for, it introduces us to an
important concept in serialization: _recursion depth_.

#### Recursion Depth

Sometimes, the process of serialization can get very complex, especially if the
data we're working with has many layers of nested structures or relationships.

Recursion depth refers to how deeply nested the data structures are when we
serialize them. If the structures are very deeply nested, the serialization
process can require a lot of memory and computational resources, which can slow
down the program or even cause it to crash.

For example, imagine you have a data structure representing a family tree, with
each person having parents, grandparents, and so on. If we try to serialize this
structure and we don't set a limit on the recursion depth, the program might
keep going deeper and deeper into the family tree, creating more and more data
to process, until it runs out of memory or crashes.

To avoid this problem, we can set a limit on the recursion depth, so that the
program only goes a certain number of layers deep before stopping. This helps us
manage the memory and computational resources needed for the serialization
process and ensures that the program runs smoothly.

### `serialize_rules`

To avoid any errors involving recursion depth, we can set the `serialize_rules`
class attribute in each of our models. This is a tuple (so remember to include
trailing commas!) where we can specify which fields to exclude. To avoid diving
too many layers into each record's relationships, we will tell
SQLAlchemy-Serializer to not look back at the original record from within its
related records. Here's what that will look like:

```py
# models.py

```

***

## Instructions

This is a **test-driven lab**. Run `pipenv install` to create your virtual
environment and `pipenv shell` to enter the virtual environment. Then run
`pytest -x` to run your tests. Use these instructions and `pytest`'s error
messages to complete your work in the `server/` folder.

Instructions begin here:

- AAA

Once all of your tests are passing, commit and push your work using `git` to
submit.

### Examples

#### Animal View

![animal ID 1, name Logan, species Snake, zookeeper Dylan Taylor,
enclosure trees](
https://curriculum-content.s3.amazonaws.com/python/flask-sqlalchemy-lab-1.png
)

#### Zookeeper View

![zookeeper name Stephanie Contreras, birthday 1996-9-20, 6 animals](
https://curriculum-content.s3.amazonaws.com/python/flask-sqlalchemy-lab-2.png
)

#### Enclosure View

![enclosure with pond environment, not open to visitors, 8 animals](
https://curriculum-content.s3.amazonaws.com/python/flask-sqlalchemy-lab-3.png
)

***

## Resources

- [Quickstart - Flask-SQLAlchemy][flask_sqla]
- [Flask-Migrate](https://flask-migrate.readthedocs.io/en/latest/)
- [Flask Extensions, Plug-ins, and Related Libraries - Full Stack Python](https://www.fullstackpython.com/flask-extensions-plug-ins-related-libraries.html)

[flask_sqla]: https://flask-sqlalchemy.palletsprojects.com/en/2.x/quickstart/#
