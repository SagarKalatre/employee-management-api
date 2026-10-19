
# Employee Management REST API

A REST API built with Java 23, Spring Boot, Spring Data JPA,
Hibernate, MySQL, and Jakarta Bean Validation.

## Requirements

- JDK 23
- Maven 3.6.3 or later
- MySQL
- VS Code or another Java IDE
- Postman

## Features

- Create an employee
- Retrieve all employees
- Retrieve an employee by ID
- Update an employee
- Delete an employee
- Validate incoming request bodies
- Handle missing employees with HTTP 404
- Reject duplicate email addresses with HTTP 409
- Return consistent JSON error responses

## Employee fields

- id: auto-generated primary key
- name: required, maximum 100 characters
- email: required, valid email, unique
- department: required, maximum 100 characters
- salary: required, zero or greater

## Database setup

Log in to MySQL:

    mysql -u root -p

Run:

    CREATE DATABASE employee_management_db;

    CREATE USER 'employee_api_user'@'localhost'
    IDENTIFIED BY 'ReplaceWithYourOwnStrongPassword';

    GRANT ALL PRIVILEGES ON employee_management_db.*
    TO 'employee_api_user'@'localhost';

    EXIT;

Replace the sample password before running these statements.

## Environment variables

In your application terminal:

    export DB_USERNAME='employee_api_user'
    read -s "DB_PASSWORD?Enter the MySQL application password: "
    export DB_PASSWORD
    echo

Enter the password configured for the MySQL application user.

These variables apply to the current terminal session. Set them
again when opening a new terminal.

## Run the project

From the project root:

    mvn clean package

Then start the application:

    mvn spring-boot:run

The API runs at:

    http://localhost:8080/api/employees

## API endpoints

| Method | Endpoint | Success status |
| POST | /api/employees | 201 Created |
| GET | /api/employees | 200 OK |
| GET | /api/employees/{id} | 200 OK |
| PUT | /api/employees/{id} | 200 OK |
| DELETE | /api/employees/{id} | 204 No Content |

Additional status codes:

- 400 Bad Request: invalid request data or JSON
- 404 Not Found: employee ID does not exist
- 409 Conflict: duplicate email or database constraint conflict

## Example POST body

    {
      "name": "Sagar Kalatre",
      "email": "sagar@example.com",
      "department": "IT",
      "salary": 45000.00
    }

Use Postman with Body > raw > JSON.

For PUT, provide all editable employee fields. Do not provide an ID
in the request body; the path ID identifies the employee to update.

## Database schema

Hibernate creates or updates the employees table through:

    spring.jpa.hibernate.ddl-auto=update

This is intended for local development. For production, prefer
controlled database migrations rather than automatic schema updates.

## Troubleshooting

### Access denied by MySQL

Check DB_USERNAME, DB_PASSWORD, the MySQL user, and its privileges.

### Communications link failure

Check that MySQL is running and listening on localhost:3306.

### Maven uses the wrong Java version

Run:

    java -version
    mvn -version

Confirm that both use the intended JDK 23.

### Port 8080 is already in use

Stop the other process using the port or change server.port in
application.properties.

### Database schema changes

Back up your data before making schema changes. Do not use
ddl-auto=create or create-drop when you need to preserve data.

## Security notes

Never commit database passwords or other secrets to source control.
This project does not implement authentication or authorization and
should not be exposed publicly without appropriate security controls.
