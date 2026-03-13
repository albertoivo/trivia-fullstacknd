# Full Stack Trivia API Backend

## Getting Started

### Installing Dependencies

#### Python 3.7

Follow instructions to install the latest version of python for your platform in the [python docs](https://docs.python.org/3/using/unix.html#getting-and-installing-the-latest-version-of-python)

#### Virtual Environment

We recommend working within a virtual environment whenever using Python for projects. This keeps your dependencies for each project separate and organized. Instructions for setting up a virtual environment for your platform can be found in the [python docs](https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/)

#### PIP Dependencies

Once you have your virtual environment setup and running, install dependencies by navigating to the `/backend` directory and running:

```bash
pip install -r requirements.txt
```

This will install all of the required packages we selected within the `requirements.txt` file, including `python-dotenv` for managing environment variables.

### Configuration

The application uses environment variables for configuration. Create a `.env` file in the `backend` directory with the following variables:

```env
DATABASE_URL=postgresql://user:password@localhost:5432/trivia
TEST_DATABASE_URL=postgresql://user:password@localhost:5432/trivia_test
FLASK_APP=flaskr
FLASK_ENV=development
```

## Database Setup
With Postgres running, restore a database using the `trivia.psql` file provided. From the backend folder in terminal run:
```bash
psql trivia < trivia.psql
```

## Running the server

From within the `backend` directory first ensure you are working using your created virtual environment.

To run the server, execute:

```bash
flask run
```

Note: Since we are using `python-dotenv`, the `FLASK_APP` and `FLASK_ENV` variables defined in your `.env` file will be automatically loaded.

## Testing
To run the tests, run
```bash
dropdb trivia_test
createdb trivia_test
psql trivia_test < trivia.psql
python test_flaskr.py
```

## API Documentation

### GET /categories
- Fetches a list of categories.
- **Request parameters**: None
- **Response body**:
```json
{
  "categories": [
    {
      "id": 1,
      "type": "Science"
    },
    {
      "id": 2,
      "type": "Art"
    },
    {
      "id": 3,
      "type": "Geography"
    }
  ]
}
```

### GET /questions
- Fetches a paginated set of questions, total number of questions, all categories, and current categories.
- **Request parameters**: `page` (query parameter, optional, defaults to 1)
- **Response body**:
```json
{
  "questions": [
    {
      "id": 1,
      "question": "This is a question",
      "answer": "This is an answer",
      "difficulty": 5,
      "category": 2
    }
  ],
  "total_questions": 100,
  "categories": [
    {"id": 1, "type": "Science"},
    {"id": 2, "type": "Art"}
  ],
  "current_category": [1, 2]
}
```

### DELETE /questions/<int:question_id>
- Deletes a specific question using the ID of the question.
- **Request parameters**: `question_id` (URL parameter)
- **Response body**:
```json
{
  "success": true
}
```

### POST /add-questions
- Adds a new question to the database.
- **Request parameters** (JSON): 
  - `question`: (string) The text of the question.
  - `answer`: (string) The answer to the question.
  - `difficulty`: (integer) The difficulty level (1-5).
  - `category`: (integer) The category ID.
- **Response body**:
```json
{
  "success": true
}
```

### POST /search-questions
- Searches for questions based on a search term (substring match, case-insensitive).
- **Request parameters** (JSON):
  - `searchTerm`: (string) The search term to filter questions.
- **Response body**:
```json
{
  "questions": [...],
  "total_questions": 10,
  "current_category": [1, 2]
}
```

### GET /categories/<int:cat_id>/questions
- Fetches paginated questions for a specific category.
- **Request parameters**: `cat_id` (URL parameter), `page` (query parameter, optional)
- **Response body**:
```json
{
  "questions": [...],
  "total_questions": 5,
  "current_category": 1
}
```

### POST /quizzes
- Fetches a random question for the quiz based on category and previously asked questions.
- **Request parameters** (JSON):
  - `previous_questions`: (list of integers) List of IDs of questions already asked.
  - `quiz_category`: (object) Object with `id` and `type` of the category. Use `id: 0` for all categories.
- **Response body**:
```json
{
  "question": {
    "id": 1,
    "question": "What is the capital of France?",
    "answer": "Paris",
    "difficulty": 1,
    "category": 3
  }
}
```

## Error Handling
Errors are returned as JSON objects in the following format:
```json
{
    "success": false,
    "error": 404,
    "message": "resource not found"
}
```
The API handles the following error types:
- 404: Resource Not Found
- 422: Unprocessable Entity