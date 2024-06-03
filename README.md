# CS 340 Module 4 ReadMe Thompson

## Project Overview

The purpose of the CRUD Python module is to offer quick and easy create and read functions for working with MongoDB databases. This module is a component of a bigger project that aims to offer complete CRUD functionality, such as updating and deleting. The main goal of this module is to simplify database interactions for developers so that managing data in Python applications is simpler.

## Motivation

The goal of this project is to provide a reusable, user-friendly Python module that makes database operations easier. To ensure consistency and dependability across many projects, database operations must be handled using a standardized methodology. This module attempts to simplify database interaction management and increase developer productivity by offering explicit mechanisms for both creating and accessing data. The module also seeks to advance Python programming and database management best practices by promoting the usage of strong error-handling procedures and object-oriented concepts.

## Getting Started

Link to GitHub README: [CRUD-Python-Module-4](https://github.com/EmpressCatbug/CRUD-Python-Module-4)

### Database and User Authentication

Using SQLAlchemy, a SQLite database with user authentication was set up in the Module Three milestone. A user's table with columns for the username, email address, and password is part of the database.

### Creating the C and R Portions of the Module

While the `read_records` method returns every record from the provided table, the `create_record` function permits the creation of new records in the defined table. During development, challenges included ensuring data validation and managing database connections efficiently. These were overcome by implementing robust error handling and using SQLAlchemy's session management features.

## Installation

### Tools Used

- **Python**: The primary programming language used for this module.
- **MongoDB**: Data on animal shelters is stored in a NoSQL database.
- **PyMongo**: A Python distribution that includes MongoDB-related utilities for interacting with the MongoDB database.
- **Jupyter Notebook**: An open-source web tool for creating and sharing documents with narrative prose, mathematics, live code, and visualizations.

### Rationale for Using These Tools

- **Python**: Chosen for its simplicity and readability, making it an ideal choice for developing the CRUD module.
- **MongoDB**: Selected for its flexibility and scalability, which is perfect for handling large datasets.
- **PyMongo**: Provides a straightforward way to interact with MongoDB from Python, making database operations easier.
- **Jupyter Notebook**: Enables interactive development and testing of the module, enhancing the development process.

## Usage

### Examples of Code

#### Creating a Record

```python
from animal_shelter import AnimalShelter

USER = 'aacuser'
PASS = '******'
HOST = 'nv-desktop-services.apporto.com'
PORT = *****
DB = 'AAC'
COL = 'animals'

shelter = AnimalShelter(USER, PASS, HOST, PORT, DB, COL)

new_animal = {
    "age_upon_outcome": "1 year",
    "animal_id": "A812345",
    "breed": "Labrador Retriever Mix",
    "color": "Black/White",
    "date_of_birth": "2019-01-01",
    "datetime": "2020-01-10 16:30:00",
    "outcome_type": "Adoption",
    "sex_upon_outcome": "Neutered Male",
    "name": "Max"
}

result = shelter.create(new_animal)
print("Insert successful:", result)
```

#### Reading Records

```python
query = {"animal_id": "A812345"}
animals = shelter.read(query)
print("Query result:", animals)
```

## Tests

To ensure that the CRUD Python module functions correctly, you can run unit tests using the `pytest` framework. Below are the steps to set up and run the tests, along with some example test cases.

### Setting Up the Test Environment

1. **Install pytest**:
   Ensure you have `pytest` installed in your virtual environment. You can install it using the following command:

   ```bash
   pip install pytest
   ```

2. **Run the Tests**:
   Go to your project's root directory and enter the following command to launch the tests:

   ```bash
   pytest tests/
   ```

### Example Test Cases

#### Test for Creating a Record

Create a file named `test_create_record.py` in the `tests` directory with the following content:

```python
import pytest
from animal_shelter import AnimalShelter
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker
from models import Base, User

# Setting up an in-memory SQLite database for testing
@pytest.fixture(scope='module')
def test_engine():
    engine = create_engine('sqlite:///:memory:')
    Base.metadata.create_all(engine)
    return engine

@pytest.fixture(scope='module')
def test_session(test_engine):
    Session = sessionmaker(bind=test_engine)
    return Session()

def test_create_record(test_session):
    # Test data
    data = {
        "username": "janedoe",
        "email": "janedoe@example.com",
        "password": "securepassword123"
    }

    # Create record
    create_record("users", data, session=test_session)

    # Query the database
    user = test_session.query(User).filter_by(username="janedoe").first()

    # Assertions
    assert user is not None
    assert user.username == "janedoe"
    assert user.email == "janedoe@example.com"
    assert user.password == "securepassword123"
```

#### Test for Reading Records

Create a file named `test_read_records.py` in the `tests` directory with the following content:

```python
import pytest
from animal_shelter import AnimalShelter
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker
from models import Base, User

# Setting up an in-memory SQLite database for testing
@pytest.fixture(scope='module')
def test_engine():
    engine = create_engine('sqlite:///:memory:')
    Base.metadata.create_all(engine)
    return engine

@pytest.fixture(scope='module')
def test_session(test_engine):
    Session = sessionmaker(bind=test_engine)
    return Session()

def test_read_records(test_session):
    # Test data
    user1 = User(username="johndoe", email="johndoe@example.com", password="password1")
    user2 = User(username="janedoe", email="janedoe@example.com", password="password2")
    test_session.add(user1)
    test_session.add(user2)
    test_session.commit()

    # Read records
    records = read_records("users", session=test_session)

    # Assertions
    assert len(records) == 2
    assert records[0].username == "johndoe"
    assert records[1].username == "janedoe"
```

### Running the Tests

To run the tests, ensure you have the test files in the `tests` directory and then use the `pytest` command:

```bash
pytest tests/
```

This command will discover and run all the test files in the `tests` directory and display the results in the terminal.

## Proposed Features

### Revise the Features

1. **Update Functionality**:
   - Provide the option to amend already-existing database records.
   - Permit users to edit some fields in a record without changing other fields.

2. **Delete Functionality**:
   - Provide a way for records to be deleted from the database.
   - When records are identified as inactive, use the soft delete options rather than removing them completely.

3. **Authorization and Authentication of Users**:
   - Add role-based authorization and user authentication to improve the module.
   - Implement secure password storage and user session management.

4. **Advanced Query Capabilities**:
   - Add support for complex queries, such as filtering, sorting, and pagination.
   - Provide utility functions for common query patterns.

5. **Database Support Expansion**:
   - Extend support to additional databases like PostgreSQL and MySQL.
   - Ensure compatibility with various database systems through SQLAlchemy's ORM capabilities.

6. **Comprehensive Documentation**:
   - Create detailed documentation for all functions and features.
   - Include more code examples, use cases, and troubleshooting tips.

## Known Issues

1. **Error Handling**:
   - Current error handling is basic and needs improvement for better user feedback and debugging.
   - Plan to implement more robust error messages and logging mechanisms.

2. **Test Coverage**:
   - Existing tests cover basic functionality but need to be expanded to cover edge cases and exception handling.
   - Increase test coverage to ensure reliability and stability of the module.

3. **Performance Optimization**:
   - Identify and address any performance bottlenecks in the current implementation.
   - Optimize database interactions and query efficiency.

## Future Releases

1. **Version 1.1**:
   - Include update and delete functionalities.
   - Enhance error handling and logging.

2. **Version 1.2**:
   - Implement user authentication and authorization.
   - Expand database support to include PostgreSQL and MySQL.

3. **Version 1.3**:
   - Add advanced query capabilities.
   - Provide comprehensive documentation and more examples of use cases.

## Project Highlights

- **Usability**: The module's straightforward and intuitive design makes it easier for developers to incorporate CRUD operations into their projects.
- **Modular Design**: Constructed with modularity in mind, the design facilitates effortless expansion and personalization.
- **Robust ORM Integration**: Utilizes SQLAlchemy for powerful and flexible database interactions.
- **Community Driven**: Open to contributions and feedback from the community, encouraging collaborative development and continuous improvement.

## Contact

Your name: Takeria Thompson
