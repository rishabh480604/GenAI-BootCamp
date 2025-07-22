# Backend Tech Stack

## Business Goal: 
A language learning school wants to build a prototype of learning portal which will act as three things:
Inventory of possible vocabulary that can be learned
Act as a  Learning record store (LRS), providing correct and wrong score on practice vocabulary
A unified launchpad to launch different learning apps

## Technical requirements:

- The backend will be built using Node.js and Express.js
- The database will be built using SQLite3
- The API will be built using REST API
- The API does not require authentication and authorization
- The API will always return JSON response
- Everything is developed for single user

## Database Schema:
The databse will be a single sqlite database called `lang_portal.db` that will be in the root directory of the project folder of backend.
- The database will have following tables:
    - words - stored vocabulary words
        - id integer primary key autoincrement
        - japanese string
        - romaji string
        - english string
        - parts json

    - words-groups - join table for words and groups
    many-to-many
        - id integer primary key autoincrement
        - word_id integer
        - group_id integer
    
    - groups - thematic groups of words
    one-to-many
        - id integer primary key autoincrement
        - name string

    - study_sessions - record of study sessions grouping word_reviews_items
    one-to-many
        - id integer primary key autoincrement
        - study_session_id integer
        - word_id integer
        - correct boolean
        - created_at datetime

    - study-activities - a specific  study activity, linking a study_session to group
    one-to-many
        - id integer primary key autoincrement
        - study_session_id integer
        - group_id integer
        - created_at datetime

    - word_review_items - a record of word practice, determining if the word was correct or not
    one-to-many
        - id integer primary key autoincrement
        - study_session_id integer
        - word_id integer
        - correct boolean
        - created_at datetime

## API Endpoints:

### GET /api/dashboard/last_study_session
    return response about the most recent study session
    - Response:
    ```json
    {
      "id": 123,
      "group_id": 1,
      "group_name": "Chapter 1 Vocabulary",
      "study_activity_id": 1,
      "created_at": "2023-10-27T10:00:00Z"
    }
    ```

### GET /api/dashboard/study_progress
    return response about the study progress
    - Response:
    ```json
    {
      "total_words": 125,
      "total_study_sessions": 4,
      "total_active_groups": 2,
      "study_streak": 2
    }
    ```

### GET /api/dashboard/quick_stats
    return response about the quick stats
    - Response:
    ```json
    {
      "accuracy": 80,
      "total_study_sessions": 4,
      "total_active_groups": 2,
      "study_streak": 2
    }
    ```

### GET /api/study_activities/:id
    return response about the study activity
    - Response:
    ```json
    {
      "id": 123,
      "name": "vocabulary Quiz",
      "thumbnail_url":"https://example.com/thumbnail.jpg",
      "description":"A quiz to test your vocabulary knowledge",
      "created_at":Date.now()
    }
    ```
- GET /api/study_activities/:id/study_sessions
    return response about the study sessions for the study activity
    - Response:
    ```json
    {
        "items":[{:
          "id": 123,
          "activity_name":"vocabulary Quiz",
          "group_name": "Chapter 1 Vocabulary",
          "start_time": "2023-10-27T10:00:00Z",
          "end_time": "2023-10-27T11:00:00Z",
          "study_activity_id": 1,
          "created_at": "2023-10-27T10:00:00Z"
        }],
        "pagination":{
          "total_pages": 100,
          "current_page": 1,
          "total_items": 10,
          "items_per_page": 10
        }
    }
    ```
### POST /api/study_activities
#### required params:
- group_id integer
- study_activity_id integer

#### JSON Response:
```json
{
  "id": 123,
  "group_id": 1
}
```
### GET /api/words
- pagination with 100 items per page
#### JSON Response
```json
{
    "items":[{
        "id": 123,
        "japanese": "こんにちは",
        "romaji": "konnichiwa",
        "english": "hello",
        "correct_count": 1,
        "wrong_count": 0
    }],
    "pagination":{
        "total_pages": 100,
        "current_page": 1,
        "total_items": 10,
        "items_per_page": 10
    }
}
```

### POST /api/study_sessions/:id/words/:word_id/review
#### required param
- correct boolean
- id (study_session_id) integer
- word_id integer
#### Request Payload
```json
{
    "correct": true
}
```
#### JSON Response
```json
{
    "success": true,
    "word_id": 123,
    "study_session_id": 123,
    "correct": true,
    "created_at": "2023-10-27T10:00:00Z"
}
``` 
        

### GET /api/words/:id
#### JSON Response
```json
{
    "japanese": "こんにちは",
    "romaji": "konnichiwa",
    "english": "hello",
    "study_stats":{
        "correct_count": 1,
        "wrong_count": 0
    },
    "groups":[{
        "id": 1,
        "name": "Basic Greeting"
    }]
}
```
### GET /api/groups
    - pagination with 100 items per page
#### JSON Response
```json
{
    "items":[{
        "id": 1,
        "name": "Basic Greeting",
        "words_count": 10
    }],
    "pagination":{
        "total_pages": 100,
        "current_page": 1,
        "total_items": 10,
        "items_per_page": 10
    }
}
```
### GET /api/groups/:id
#### JSON Response
```json
{
    "id": 1,
    "name": "Basic Greeting",
    "stats":{
        "total_word_count": 10
        
    }
}
```

### GET /api/groups/:id/words
#### JSON Response
```json
{
    "items":[{
        "id":123,
        "activity_name":"vocabulary Quiz",
        "group_name": "Chapter 1 Vocabulary",
        "start_time": "2023-10-27T10:00:00Z",
        "end_time": "2023-10-27T11:00:00Z",
        "review_items_count": 10
    }],
    "pagination":{
        "total_pages": 100,
        "current_page": 1,
        "total_items": 10,
        "items_per_page": 10
    }
}
```

### GET /api/groups/:id/study_sessions
#### JSON Response
```json
{
    "items":[{
        "id":123,
        "activity_name":"vocabulary Quiz",
        "group_name": "Chapter 1 Vocabulary",
        "start_time": "2023-10-27T10:00:00Z",
        "end_time": "2023-10-27T11:00:00Z",
        "review_items_count": 10
    }],
    "pagination":{
        "total_pages": 100,
        "current_page": 1,
        "total_items": 10,
        "items_per_page": 10
    }
}
```
### GET /api/study_sessions
    - pagination with 100 items per page
#### JSON Response
```json
{
    "items":[{
        "id":123,
        "activity_name":"vocabulary Quiz",
        "group_name": "Chapter 1 Vocabulary",
        "start_time": "2023-10-27T10:00:00Z",
        "end_time": "2023-10-27T11:00:00Z",
        "review_items_count": 10
    }],
    "pagination":{
        "total_pages": 100,
        "current_page": 1,
        "total_items": 10,
        "items_per_page": 10
    }
}
```

### GET /api/study_sessions/:id
- pagination with 100 items per page
#### JSON Response
```json
{
    "id":123,
    "activity_name":"vocabulary Quiz",
    "group_name": "Chapter 1 Vocabulary",
    "start_time": "2023-10-27T10:00:00Z",
    "end_time": "2023-10-27T11:00:00Z",
    "review_items_count": 10
}
```


### GET /api/study_sessions/:id/words
- pagination with 100 items per page
#### JSON Response
```json
{
    "items":[{
        
        "japanese":"こんにちは",
        "romaji":"konnichiwa",
        "english":"hello",
        "correct_count": 1,
        "wrong_count": 0
    }],
    "pagination":{
        "total_pages": 100,
        "current_page": 1,
        "total_items": 10,
        "items_per_page": 10
    }
}
```

### POST /api/settings/reset_history
#### JSON Response
```json
{
    "success":true,
    "message":"History reset successfully"
}
```
### POST /api/settings/full_reset
#### JSON Response
```json
{
    "success":true,
    "message":"System has been fully reset"
}
```
### POST /api/settings/theme
#### required params
- theme string
#### JSON Response
```json
{
    "success":true,
    "theme": "input param",
    "message":"Theme updated successfully"
}
```
## npm Tasks
npm is task runner for nodejs.
Let's list our possible task we need for lang portal.

### Initialise Database
The task will initialise the sqlite database called `lang_portal.db`.

### Migrate Database
This task will run a series of migration sql files on the database
Migrations live in the 'migrations' folder.
The migration files will berun in order of their file name.
The file name should looks ike this: 

```sql
0001_init.sql
0002_create_words_table.sql
```

### Seed Data
This task will import json files and transform them into target data for our database.

All seed file live in the `seeds` folder.
.

In our task we should have DSL to  specific each seed file and its data for our database. 
```json
{
    {
        "kanji": "払う",
        "romaji": "harau",
        "english": "to pay",
        "parts": [
            {
              "kanji": "払",
              "romaji": ["hara"]
              
            },
            {
              "kanji": "う",
              "romaji": ["u"]
            }
    ]
    
    }
}