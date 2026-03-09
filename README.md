# Quiz Application - Spring Boot REST API

A RESTful Quiz Application built with **Spring Boot** that allows users to manage questions, create quizzes, and submit answers for scoring.

---

## Tech Stack

- **Java 17**
- **Spring Boot 3.5.7**
- **Spring Data JPA** — ORM and database access
- **PostgreSQL** — Relational database
- **Lombok** — Boilerplate code reduction
- **Maven** — Build and dependency management

---

## Project Structure

```
src/main/java/com/example/quizapp/
├── QuizappApplication.java          # Application entry point
├── controller/
│   ├── QuestionController.java      # REST endpoints for questions
│   └── QuizController.java          # REST endpoints for quizzes
├── dao/
│   ├── QuestionDao.java             # JPA repository for Question entity
│   └── QuizDao.java                 # JPA repository for Quiz entity
├── model/
│   ├── Question.java                # Question entity (JPA)
│   ├── QuestionWrapper.java         # DTO — hides correct answer from users
│   ├── Quiz.java                    # Quiz entity (JPA)
│   └── Response.java                # DTO — user's answer submission
└── service/
    ├── QuestionService.java         # Business logic for questions
    └── QuizService.java             # Business logic for quizzes
```

---

## Data Model

### Question

| Field           | Type    | Description                     |
| --------------- | ------- | ------------------------------- |
| id              | Integer | Auto-generated primary key      |
| questionTitle   | String  | The question text               |
| option1         | String  | First answer option             |
| option2         | String  | Second answer option            |
| option3         | String  | Third answer option             |
| option4         | String  | Fourth answer option            |
| rightAnswer     | String  | The correct answer              |
| difficultyLevel | String  | Difficulty (e.g., Easy, Medium) |
| category        | String  | Category (e.g., Java, Python)   |

### Quiz

| Field     | Type            | Description                            |
| --------- | --------------- | -------------------------------------- |
| id        | Integer         | Auto-generated primary key             |
| title     | String          | Quiz title                             |
| questions | List\<Question> | Many-to-Many relationship to questions |

---

## API Endpoints

### Question Endpoints (`/Question`)

| Method | Endpoint                        | Description                        |
| ------ | ------------------------------- | ---------------------------------- |
| GET    | `/Question/allQuestions`        | Retrieve all questions             |
| GET    | `/Question/category/{category}` | Get questions filtered by category |
| POST   | `/Question/add`                 | Add a new question                 |
| PUT    | `/Question/update/{id}`         | Update an existing question by ID  |
| DELETE | `/Question/delete/{id}`         | Delete a question by ID            |

#### Add a Question — `POST /Question/add`

**Request Body:**

```json
{
  "questionTitle": "What is the size of int in Java?",
  "option1": "1 byte",
  "option2": "2 bytes",
  "option3": "4 bytes",
  "option4": "8 bytes",
  "rightAnswer": "4 bytes",
  "difficultyLevel": "Easy",
  "category": "Java"
}
```

#### Update a Question — `PUT /Question/update/{id}`

**Request Body:**

```json
{
  "questionTitle": "What is the default value of int in Java?",
  "option1": "0",
  "option2": "null",
  "option3": "1",
  "option4": "undefined",
  "rightAnswer": "0",
  "difficultyLevel": "Easy",
  "category": "Java"
}
```

#### Delete a Question — `DELETE /Question/delete/{id}`

**Example:**

```
DELETE /Question/delete/5
```

**Response:** `"Question deleted successfully"` with HTTP 200, or `"Failed to delete Question"` with HTTP 400.

### Quiz Endpoints (`/quiz`)

| Method | Endpoint            | Description                                         |
| ------ | ------------------- | --------------------------------------------------- |
| POST   | `/quiz/create`      | Create a quiz with random questions from a category |
| GET    | `/quiz/get/{id}`    | Get quiz questions (without correct answers)        |
| POST   | `/quiz/submit/{id}` | Submit answers and get the score                    |

#### Create a Quiz — `POST /quiz/create`

**Query Parameters:**

| Parameter | Type   | Description                     |
| --------- | ------ | ------------------------------- |
| category  | String | Question category to pull from  |
| numQ      | int    | Number of questions in the quiz |
| title     | String | Title for the quiz              |

**Example:**

```
POST /quiz/create?category=Java&numQ=5&title=Java Basics Quiz
```

#### Get Quiz Questions — `GET /quiz/get/{id}`

Returns questions wrapped in `QuestionWrapper` (without the correct answer), so users cannot see the right answer before submitting.

**Response:**

```json
[
  {
    "id": 1,
    "questionTitle": "What is the size of int in Java?",
    "option1": "1 byte",
    "option2": "2 bytes",
    "option3": "4 bytes",
    "option4": "8 bytes"
  }
]
```

#### Submit Quiz — `POST /quiz/submit/{id}`

**Request Body:**

```json
[
  { "id": 1, "response": "4 bytes" },
  { "id": 2, "response": "JVM" }
]
```

**Response:** Returns the number of correct answers as an integer.

---

## Prerequisites

- **Java 17** or higher
- **PostgreSQL** installed and running
- **Maven** (or use the included Maven wrapper)

---

## Database Configuration

The application connects to a PostgreSQL database. Update [application.properties](src/main/resources/application.properties) with your database credentials:

```properties
spring.datasource.driver-class-name=org.postgresql.Driver
spring.datasource.url=jdbc:postgresql://localhost:5432/postgres
spring.datasource.username=postgres
spring.datasource.password=your_password
spring.jpa.hibernate.ddl-auto=update
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
```

> Hibernate's `ddl-auto=update` will automatically create/update tables based on the entity classes.

---

## Getting Started

1. **Clone the repository**

   ```bash
   git clone <repository-url>
   cd quiz-spring
   ```

2. **Set up PostgreSQL**
   - Create or use an existing PostgreSQL database.
   - Update the database credentials in `src/main/resources/application.properties`.

3. **Build the project**

   ```bash
   ./mvnw clean install
   ```

4. **Run the application**

   ```bash
   ./mvnw spring-boot:run
   ```

   The server starts on **http://localhost:8080** by default.

---

## Screenshots

| Screenshot                                                | Description                     |
| --------------------------------------------------------- | ------------------------------- |
| ![Add Question](Screenshot/add-question.png)              | Adding a new question           |
| ![All Questions](Screenshot/findall.png)                  | Fetching all questions          |
| ![By Category](Screenshot/findall-category.png)           | Filtering questions by category |
| ![Category Java](Screenshot/category_java.png)            | Java category questions         |
| ![Get Quiz](Screenshot/get-1.png)                         | Retrieving quiz questions       |
| ![201 Created](Screenshot/210-created.png)                | Successful creation response    |
| ![404 Not Found](Screenshot/404-not%20found.png)          | Not found error                 |
| ![Java Random](Screenshot/java%20random.png)              | Random Java questions           |
| ![Postgre Answer](Screenshot/answer%20from%20postgre.png) | Data from PostgreSQL            |

---

## License

This project is for educational purposes.
