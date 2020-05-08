# Survey API
A simple survey application

## Prerequisites

	- Apache Maven 3+
	- [Martini Desktop](https://www.torocloud.com/martini/download)

## Building the Martini Package

```bash
$ mvn clean package
```

This will create a ZIP file named `survey-api-bin.zip` containing all the files (services, configurations, etc.) needed under the `target` folder. This ZIP file is what we call a [Martini Package](https://docs.torocloud.com/martini/latest/developing/package/) which then you can import in Martini Desktop to get started. You can learn more how to import a Martini Package by visiting our [documentation](https://docs.torocloud.com/martini/latest/developing/package/importing/)

## Usage

This package exposes operations for a simple survey REST API that allows you to do CRUD operations. You can find the [Gloop REST API](https://docs.torocloud.com/martini/latest/developing/gloop/api/rest/) file named `Survey` under the `code` folder after importing the package to your Martini Desktop application.

## Setup

This Martini Package automatically sets up the required resources for you. During the package startup, Martini will execute the configured _Startup Service_ that initializes the database  and creates the records for the `question_type` table.

The package uses HSQL as the default datastore but you can change this to MySQL, Postgres, MSSQL, or Oracle by updating the [package properties](https://docs.torocloud.com/martini/latest/developing/package/properties/) located at [conf](https://docs.torocloud.com/martini/latest/developing/package/directory/) folder. Below is what the contents of the properties file look like for HSQL and MySQL

```
# The database type to be used
database.type=hsql

# HSQL database configuration to be used for testing environment
hsql.driver=org.hsqldb.jdbc.JDBCDriver
hsql.connection.url=jdbc:hsqldb:file:${toroesb.home}data/hsql/<database-to-use>.db;hsqldb.tx=MVCC;sql.syntax_mys=true
hsql.username=sa
hsql.password=

# MySQL database configuration to be used in production environment
mysql.driver=com.mysql.cj.jdbc.Driver
mysql.connection.url=jdbc:mysql://<host>/<database-to-use>
mysql.username=root
mysql.password=
```

* `database.type` sets the database provider the Martini package will use. If you will use the default hsql config, you don't need to change anything in the hsql configuration. **Note**: If you will use a different hsql database, make sure you add `sql.syntax_mys=true` in the connection properties. This ensures that the SQL query from the SQL Services in this package will be compatible with hsql.

## Operations

The base url is `<host>/api/sm` where `host` is the location where Martini is deployed. By default, it's `localhost:8080`.

### Survey

`GET /survey`

Returns a list of surveys

**Sample Request**

**curl**

```bash
curl -X GET http://localhost:8080/api/sm/survey -H 'Accept: application/json'
```

**Sample Response**

If the request is sucessful, it will return an HTTP status code of `200` with the response payload below:

```json
{
    "result": "OK",
    "message": "OK",
    "warnings": [],
    "Survey": [
        {
            "survey_id": "e6e8138a-349a-4a9a-abb3-bf89db69e852",
            "title": "Favorite browser",
            "description": "A sample description",
            "created_date": "2020-03-25T15:02:19+0800"
        },
        ...
    ]
}
```

`GET /survey/{survey_id}`

Returns a survey record that matches the given `survey_id`

**Sample Request**

**curl**

```bash
curl -X GET \
	http://localhost:8080/api/sm/survey/eb876944-6618-48a2-b4f5-6ed45c732d8f \
	-H 'Accept: application/json'
```

**Sample Response**

If the request is successful, it will return an HTTP status code of `200` with the response payload below.

```json
{
    "result": "OK",
    "message": "OK",
    "warnings": [],
    "Survey": {
        "survey_id": "e6e8138a-349a-4a9a-abb3-bf89db69e852",
        "title": "Favorite browser",
        "description": "A sample description",
        "created_date": "2020-03-25T15:02:19+0800"
    }
}
```

`POST /survey`

Create a new survey

**Request Body**

`title` - Title of the survey
`description` - Description of the survey

**Sample Request**

**curl**

```bash
curl -x POST \
	http://localhost:8080/api/sm/survey \
	-H 'Accept: application/json' \
	-H 'Content-Type: application/json' \
	-d '{
		    "title": "Favorite browser",
		    "description": "A sample description"
		}'
```

**Sample Response**

If the request is successful, it will return an HTTP status code of `200` with the response payload below.

```json
{
    "result": "OK",
    "message": "OK",
    "warnings": [],
    "Survey": {
        "survey_id": "e6e8138a-349a-4a9a-abb3-bf89db69e852",
        "title": "Favorite browser",
        "description": "A sample description",
        "created_date": "2020-03-25T15:02:19+0800"
    }
}
```

`PUT /survey`

Updates the survey that matches the `survey_id`

**Request Body**

`survey_id` - The id of the survey to update
`title` - Title of the survey
`description` - Description of the survey

**Sample Request**

**curl**

```bash
curl -X PUT \
	http://localhost:8080/api/sm/survey \
	-H 'Accept: application/json' \
	-H 'Content-Type: application/json' \
	-d '{
	    "survey_id": "eb876944-6618-48a2-b4f5-6ed45c732d8f",
	    "description": "Updated description"
	}'
```

**Sample Response**

If the request is successful, it will return an HTTP status code of `200` with the response payload below.

```json
{
    "result": "OK",
    "message": "Successfully updated survey.",
    "warnings": []
}
```

`DELETE /survey/{survey_id}`

Deletes a survey that matches the `survey_id`

**Sample Request**

**curl**

```
curl -X DELETE \
	http://localhost:8080/api/sm/survey/eb876944-6618-48a2-b4f5-6ed45c732d8f
```

**Sample Response**

If the request is successful, it will return an HTTP status code of `200`

```json
{
    "result": "OK",
    "message": "Successfully deleted survey.",
    "warnings": [],
    "Survey": {
        "survey_id": "eb876944-6618-48a2-b4f5-6ed45c732d8f"
    }
}
```

`GET /survey/{survey_id}/questions`

Retrieves all questions associated with the given `survey_id`

**Sample Request**

**curl**

```bash
curl -X GET \
	http://localhost:8080/api/sm/survey/eb876944-6618-48a2-b4f5-6ed45c732d8f/questions \
	-H 'Accept: application/json'
```

**Sample Response**

If the request is successful, it will return an HTTP status code of `200` with the response payload below:

```json
{
    "result": "OK",
    "message": "OK",
    "warnings": [],
    "Question": [
        {
            "question_id": "9c32e2d0-74d9-4f3a-a6b7-daa5f18fe174",
            "question": "Which is your favorite browser?",
            "survey_id": "e6e8138a-349a-4a9a-abb3-bf89db69e852",
            "question_type_id": "be014217-f850-4d9d-9339-cffe3dd03beb",
            "required": true,
            "created_date": "2020-03-25T15:05:24+0800"
        },
        ...
    ]
}
```

### Question

`GET /question`

Returns a list of questions

**Sample Request**

**curl**

```bash
curl -X GET \
	http://localhost:8080/api/sm/question \
	-H 'Accept: application/json'
```

**Sample Response**

If the request is successful, it will return an HTTP status code of `200` with the response payload below:

```json
{
    "result": "OK",
    "message": "OK",
    "warnings": [],
    "Question": [
        {
            "question_id": "9c32e2d0-74d9-4f3a-a6b7-daa5f18fe174",
            "question": "Which is your favorite browser?",
            "survey_id": "e6e8138a-349a-4a9a-abb3-bf89db69e852",
            "question_type_id": "be014217-f850-4d9d-9339-cffe3dd03beb",
            "required": true,
            "created_date": "2020-03-25T15:05:24+0800"
        },
        ...
    ]
}
```

`GET /question/{question_id}`

Returns a question that matches the given `question_id`

**Sample Request**

**curl**

```bash
curl -X GET \
	http://localhost:8080/api/sm/question/9c32e2d0-74d9-4f3a-a6b7-daa5f18fe174 \
	-H 'Accept: application/json'
```

**Sample Response**

if the request is successful, it will return an HTTP status code of `200` with the response payload below:

```json
{
    "result": "OK",
    "message": "OK",
    "warnings": [],
    "Question": {
        "question_id": "9c32e2d0-74d9-4f3a-a6b7-daa5f18fe174",
        "question": "Which is your favorite browser?",
        "survey_id": "e6e8138a-349a-4a9a-abb3-bf89db69e852",
        "question_type_id": "be014217-f850-4d9d-9339-cffe3dd03beb",
        "required": true,
        "created_date": "2020-03-25T15:05:24+0800"
    }
}
```

`POST /question`

Creates a new question

**Request Body**

`question` - A question of the survey
`survey_id` - The id of the survey the question is in
`question_type_id` - The id of the question type
`required` - If the question is required to answer

**Sample Request**

**curl**

```bash
curl -X POST \
	http://localhost:8080/api/sm/question \
	-H 'Accept: application/json' \
	-H 'Content-Type: application/json' \
	-d '{
	    "question": "Which is your favorite browser?",
	    "survey_id": "e6e8138a-349a-4a9a-abb3-bf89db69e852",
	    "question_type_id": "be014217-f850-4d9d-9339-cffe3dd03beb",
	    "required": "true"
	}'
```

**Sample Response**

If the request is successful, it will return an HTTP status code of `200` with the response payload below:

```json
{
    "result": "OK",
    "message": "OK",
    "warnings": [],
    "Question": {
        "question_id": "9c32e2d0-74d9-4f3a-a6b7-daa5f18fe174",
        "question": "Which is your favorite browser?",
        "survey_id": "e6e8138a-349a-4a9a-abb3-bf89db69e852",
        "question_type_id": "be014217-f850-4d9d-9339-cffe3dd03beb",
        "required": true,
        "created_date": "2020-03-25T15:05:24+0800"
    }
}
```

`PUT /question`

**Request Body**

`question_id` - The id of the question to update
`question` - A question of the survey
`survey_id` - The id of the survey the question is in
`question_type_id` - The id of the question type
`required` - If the question is required to answer

**Sample Request**

**curl**

```bash
curl -X PUT \
	http://localhost:8080/api/sm/question_id \
	-H 'Accept: application/json' \
	-H 'Content-Type: application/json' \
	-d '{
		"question_id": "9c32e2d0-74d9-4f3a-a6b7-daa5f18fe174",
		"question": "Which is your favorite browser? (Updated)"
	}'
```

**Sample Response**

If the request is successful, it will return an HTTP status code of `200` with the response payload below.

```json
{
    "result": "OK",
    "message": "Successfully updated question.",
    "warnings": []
}
```

`DELETE /question/{question_id}`

Deletes a question that matches the `question_id`

**Sample Request**

**curl**

```bash
curl -X DELETE \
	http://localhost:8080/api/sm/question/9c32e2d0-74d9-4f3a-a6b7-daa5f18fe174
	-H 'Accept: application/json'
```

**Sample Response**

If the request is successful, it will return an HTTP status code of `200` with the response payload below:

```json
{
    "result": "OK",
    "message": "Successfully deleted question.",
    "warnings": [],
    "Question": {
        "question_id": "66390a7b-a30a-4718-9f81-1e53623bc57e"
    }
}
```

`GET /question/{question_id}/answers`

Retrieves all answers associated with the given `question_id`

**Sample Request**

**curl**

```bash
curl -X GET \
	http://localhost:7070/api/sm/question/9c32e2d0-74d9-4f3a-a6b7-daa5f18fe174/answers
	-H 'Accept: application/json'
```

**Sample Response**

If the request is successful, it will return an HTTP status code of `200` with the response payload below.

```json
{
    "result": "OK",
    "message": "OK",
    "warnings": [],
    "Answer": [
        {
            "answer_id": "39da345a-b8a8-4d7a-a474-505c4232bde1",
            "question_id": "9c32e2d0-74d9-4f3a-a6b7-daa5f18fe174",
            "answer": "Chrome",
            "user_answer_count": 0
        },
        {
            "answer_id": "5312678c-65c9-47f4-b021-32a825f3587e",
            "question_id": "9c32e2d0-74d9-4f3a-a6b7-daa5f18fe174",
            "answer": "Firefox",
            "user_answer_count": 0
        },
        {
            "answer_id": "982f794d-068a-4e82-84fe-1951fdca75ce",
            "question_id": "9c32e2d0-74d9-4f3a-a6b7-daa5f18fe174",
            "answer": "Edge",
            "user_answer_count": 0
        },
        {
            "answer_id": "bd90ea9b-ad38-49b7-b324-62c8b8b91b82",
            "question_id": "9c32e2d0-74d9-4f3a-a6b7-daa5f18fe174",
            "answer": "Safari",
            "user_answer_count": 0
        }
    ]
}
```

### Answer

`GET /answer`

Returns a list of answers

**Sample Request**

**curl**

```bash
curl -X GET \
	http://localhost:8080/api/sm/answer
	-H 'Accept: application/json'
```

**Sample Response**

If the request is successful, it will return an HTTP status code of `200` with the response payload below.

```json
{
    "result": "OK",
    "message": "OK",
    "warnings": [],
    "Answer": [
        {
            "answer_id": "39da345a-b8a8-4d7a-a474-505c4232bde1",
            "question_id": "9c32e2d0-74d9-4f3a-a6b7-daa5f18fe174",
            "answer": "Chrome",
            "user_answer_count": 0
        },
        {
            "answer_id": "5312678c-65c9-47f4-b021-32a825f3587e",
            "question_id": "9c32e2d0-74d9-4f3a-a6b7-daa5f18fe174",
            "answer": "Firefox",
            "user_answer_count": 0
        },
        {
            "answer_id": "bd90ea9b-ad38-49b7-b324-62c8b8b91b82",
            "question_id": "9c32e2d0-74d9-4f3a-a6b7-daa5f18fe174",
            "answer": "Safari",
            "user_answer_count": 0
        },
        {
            "answer_id": "982f794d-068a-4e82-84fe-1951fdca75ce",
            "question_id": "9c32e2d0-74d9-4f3a-a6b7-daa5f18fe174",
            "answer": "Edge",
            "user_answer_count": 0
        }
    ]
}
```

`GET /answer/{answer_id}`

Returns an answer that matches the given `answer_id`

**Sample Request**

**curl**

```bash
curl -X GET \
	http://localhost:8080/api/sm/answer/39da345a-b8a8-4d7a-a474-505c4232bde1
	-H 'Accept: application/json'
```

**Sample Response**

If the request is successful, it will return an HTTP status code of `200` with the respnse payload below.

```json
{
    "result": "OK",
    "message": "OK",
    "warnings": [],
    "Answer": {
        "answer_id": "39da345a-b8a8-4d7a-a474-505c4232bde1",
        "question_id": "9c32e2d0-74d9-4f3a-a6b7-daa5f18fe174",
        "answer": "Chrome",
        "user_answer_count": 0
    }
}
```

`POST /answer`

Creates a new answer

**Request Body**

`question_id` - The question the given answer will be an answer to
`answer` - The answer/choice of the question

**Sample Request**

**curl**

```bash
curl -X POST \
	http://localhost:8080/api/sm/answer \
	-H 'Accept: application/json' \
	-H 'Content-Type: application/json' \
	-d '{
	    "question_id": "9c32e2d0-74d9-4f3a-a6b7-daa5f18fe174",
	    "answer": "Chrome"
	}'
```

**Sample Response**

```json
{
    "result": "OK",
    "message": "OK",
    "warnings": [],
    "Answer": {
        "answer_id": "39da345a-b8a8-4d7a-a474-505c4232bde1",
        "question_id": "9c32e2d0-74d9-4f3a-a6b7-daa5f18fe174",
        "answer": "Chrome",
        "user_answer_count": 0
    }
}
```

`PUT /answer`

Updates an answer that matches the given `answer_id`

**Request Body**

`answer_id` - The id of the answer to update
`question_id` - The question the given answer will be an answer to
`answer` - The answer/choice of the question

**Sample Request**

**curl**

```bash
curl -X PUT \
	http://localhost:8080/api/sm/answer \
	-H 'Accept: application/json' \
	-H 'Content-Type: application/json' \
	-d '{
	    "answer_id": "982f794d-068a-4e82-84fe-1951fdca75ce",
	    "answer": "Chrome (Updated)"
	}'
```

**Sample Response**

```json
{
    "result": "OK",
    "message": "Successfully updated answer.",
    "warnings": []
}
```

`DELETE /answer/{answer_id}`

Deletes an answer that matches the given `answer_id`

**Sample Request**

**curl**

```bash
curl -X DELETE \
	http://localhost:8080/api/sm/answer/982f794d-068a-4e82-84fe-1951fdca75ce \
	-H 'Accept: application/json'
```

**Sample Response**

```json
{
    "result": "OK",
    "message": "Successfully deleted answer.",
    "warnings": [],
    "Answer": {
        "answer_id": "982f794d-068a-4e82-84fe-1951fdca75ce"
    }
}
```

### Question Type

The question type table has 2 default records, `Multiple Choice` and `Checkboxes`, they are inserted on package startup.

`GET /question_type`

Returns a list of question types

**Sample Request**

**curl**

```bash
curl -X GET \
	http://localhost:8080/api/sm/question_type \
	-H 'Accept: application/json'
```

**Sample Response**

If the request is successful, it will return an HTTP status code of `200` with the response playload below.

```json
{
    "result": "OK",
    "message": "OK",
    "warnings": [],
    "QuestionType": [
        {
            "question_type_id": "d56b0956-9cc2-4eb8-94cf-ada90ea727a1",
            "type": "Checkboxes"
        },
        {
            "question_type_id": "be014217-f850-4d9d-9339-cffe3dd03beb",
            "type": "Multiple Choice"
        },
        ...
    ]
}
```

`GET /question_type/{question_type_id}`

Returns a question type that matches the given `question_type_id`

**Sample Request**

**curl**

```bash
curl -X GET \
	http://localhost:8080/api/sm/question_type/d56b0956-9cc2-4eb8-94cf-ada90ea727a1 \
	-H 'Accept: application/json'
```

**Sample Response**

If the request is successful, it will return an HTTP status code of `200` with the response payload below.

```json
{
    "result": "OK",
    "message": "OK",
    "warnings": [],
    "QuestionType": {
        "question_type_id": "d56b0956-9cc2-4eb8-94cf-ada90ea727a1",
        "type": "Checkboxes"
    }
}
```

`POST /question_type`

Creates a new question type

**Request Body**

`type` - The type of the question

**Sample Request**

**curl**

```bash
curl -X POST \
	http://localhost:8080/api/sm/question_type \
	-H 'Accept: application/json' \
	-H 'Content-Type: application/json' \
	-d '{
	    "type": "Checkboxes"
	}'
```

**Sample Response**

If the request is successful, it wil lreturn an HTTP status code `200` with the response payload below.

```json
{
    "result": "OK",
    "message": "OK",
    "warnings": [],
    "QuestionType": {
        "question_type_id": "55eb4e64-bbec-409b-aba6-d23c5ec31d39",
        "type": "Checkboxes"
    }
}
```

`PUT /question_type`

Updates a question type that matches the given `question_type_id`

**Request Body**

`question_type_id` - The id of the question type to update
`type` - The type of the question

**Sample Request**

**curl**

```bash
curl -X PUT \
	http://localhost:8080/api/sm/question_type \
	-H 'Accept: application/json' \
	-H 'Content-Type: application/json' \
	-d '{
	    "question_type_id": "55eb4e64-bbec-409b-aba6-d23c5ec31d39",
	    "type": "Checboxes (Updated)"
	}'
```

**Sample Response**

If the request is successful, it will return an HTTP status code of `200` with the response payload below.

```json
{
    "result": "OK",
    "message": "Successfully updated question_type.",
    "warnings": []
}
```

`DELETE /question_type/{question_type_id}`

Deletes a question type that matches the `question_type_id`

**Sample Request**

**curl**

```bash
curl -X DELETE \
	http://localhost:8080/api/sm/question_type/55eb4e64-bbec-409b-aba6-d23c5ec31d39 \
	-H 'Accept: application/json'
```

**Sample Response**

If the request is successful, it will return an HTTP status code of `200` with the response payload below.

```json
{
    "result": "OK",
    "message": "Successfully deleted question_type.",
    "warnings": [],
    "QuestionType": {
        "question_type_id": "55eb4e64-bbec-409b-aba6-d23c5ec31d39"
    }
}
```

### Survey Answer

`POST /survey-answer`

Adds a count to a given answer's `user_answer_count`

**Request Body**

`id` - Array. List of answer ids

**Sample Request**

**curl**

```bash
curl -X POST \
	http://localhost:8080/api/sm/survey-answer \
	-H 'Accept: application/json' \
	-H 'Content-Type: application/json' \
	-d '[
	    {
	        "id": ["39da345a-b8a8-4d7a-a474-505c4232bde1"]
	    }
	]'
```

**Sample Response**

If the request is successful, it will return an HTTP status code of `200` with the response payload below.

```json
{
    "result": "OK",
    "message": "Successfully added user answer/s.",
    "warnings": []
}
```