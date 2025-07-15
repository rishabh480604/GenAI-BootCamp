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

- GET /api/dashboard/last_study_session
- GET /api/dashboard/study_progress
- GET /api/dashboard/quick_stats

- GET /api/study_activities/:id
- GET /api/study_activities/:id/study_sessions

- POST /api/study_activities
    - required params: group_id, study_activity_id



-POST /api/study_sessions/:id/words/:word_id/review
    - required params: correct
        
- GET /api/words
    - pagination with 100 items per page
- GET /api/words/:id
- GET /api/groups
    - pagination with 100 items per page
- GET /api/groups/:id
- GET /api/groups/:id/words
-GET /api/groups/:id/study_sessions
- GET /api/study_sessions
    - pagination with 100 items per page
-GET /api/study_sessions/:id


- GET /api/study_sessions/:id/words

    - POST /api/settings/theme
    - POST /api/settings/reset_history
    - POST /api/settings/full_reset
