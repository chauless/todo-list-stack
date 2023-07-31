# Todo list / Task tracker

A multiuser task scheduler. Users can use it as a TODO sheet. The source of inspiration for the project is Trello.

## Used technologies

- Java
- Maven
- Spring Boot, Spring Security, Spring Session, Spring AMQP, Spring Scheduler, Spring Mail, Spring Data JPA
- PostgreSQL
- RabbitMQ
- JWT
- Docker

## Microservices

A microservice approach was used in the project development. The project includes 3 microservices:

- [todo-list-api](https://github.com/chauless/todo-list-api)
- [todo-list-email-sender](https://github.com/chauless/todo-list-email-sender)
- [todo-list-scheduler](https://github.com/chauless/todo-list-scheduler)

### [todo-list-api](https://github.com/chauless/todo-list-api)
Spring Boot application that implements REST API for working with users and tasks.

Granting access to the functionality is implemented using JWT authentication.

Working with users:

  - Registration
  - Authorization

Working with tasks (available only for authorized users):

  - Creation
  - Reading
  - Editing (Changing title and description, marking task as done)
  - Deleting


### [todo-list-email-sender](https://github.com/chauless/todo-list-email-sender)
Spring Boot application with two modules - Spring Mail and Spring AMQP.

With Spring AMQP, the application connects to RabbitMQ and creates a queue, then subscribes to messages received from the scheduler and backend service.

For each received message, whose contents are deserialized into a model instance, the Spring Mail module is used to send an email.

### [todo-list-scheduler](https://github.com/chauless/todo-list-scheduler)
Spring Boot application with two modules - Spring Scheduler and Spring AMQP.

The task of the service is to iterate all users once a day, generate reports for them about the day, and generate emails for sending. The generated emails are sent to the RabbitMQ queue.

## Instructions for running the project on a local machine

- Install Docker on local machine - https://docs.docker.com/get-docker/
- Clone the task-tracker-stack repository
```bash
git clone git@github.com:chauless/todo-list-stack.git
```
- Fill in the SMTP configuration in .env file. Suggested using Brevo SMTP service - [Brevo](https://www.brevo.com/)
```bash
#Example of filling the configuration:
SMTP_HOST=smtp-relay.brevo.com
SMTP_USERNAME=test@test.test
SMTP_PASSWORD=testpassword
```
- Run the docker-compose file
```bash
docker-compose up
```
- The documentation page will be available at: http://localhost:8080/swagger-ui/index.html
