# Frontend Tech Specs:

## Pages

### Dashboard

### url
/dashboard

#### purpose
The purposeof this page is to provide a summary of learning ad act as the default page when a user visits the site.

#### components
- Last study session
    shows last activity used
    shows when last activity was used
    shows last activity score
    shows summary of last activity (wrong vs correct)
    has a link to the group

- Study progress
    -total words study example: 3/125
        - across all study session show the total words studied out of total words in the database
        - display a mastery progress bar

    -quick stats
        - acuuracy example: 80%
        - total study sessions eg- 4
        - total active groups eg- 2
        - study streak eg- 2

    -start studying button
        - goes to study activities page

#### Required API Endpoints
    - GET /api/dashboard/last_study_session
    - GET /api/dashboard/study_progress
    - GET /api/dashboard/quick_stats

### Study Activities Index

#### url
/study-activities

#### Purpose
 to show a collection of study activities with a thumbnail and ists name, to either launch or view the study activity.

#### components
- study actvities card
    - show a thumbnail of the study activity
    - the name of the activity
    - a launch button to take us to the launch page
    - a view page to view more information about past study sessions


#### Required API Endpoints
    - GET /api/study_activities
        

### Study Activity Show

#### url
/study_activities/:id

#### Purpose
To show the details of a study activity and its past study sessions.

#### components
-Name of Study activity
-Thumbnail of Study activity
-Description of Study activity
-Launch button
-Study Activities Paginated List
    -id
    -activity name
    -group name
    -start time
    -end time(inferred by the last word_review_item submitted)
    -no of review items

#### Required API Endpoints
    - GET /api/study_activities/:id
    - GET /api/study_activities/:id/study_sessions

### Study Activity Launch

#### url
/study_activities/:id/launch

#### Purpose
To launch a study activity.

#### components
- Name of Study activity
- Launch from
    - Select field for group
    - launch now button

## Behaviour
After the form is submitted a new tab opens with the study activity based on its URL provided in the database
Also the after form is submitted the page will redirect to the study session page
#### Required API Endpoints
- POST /api/study_activities
    
### Words Index

#### url
/words

#### Purpose
The purpose of this page is to show all words in our database
#### components
- words paginated list
    - Columns
        - japanese
        - romaji
        - english
        - correct count
        - wrong count
    - Pagination with 100 items per page
    - clicking the japanese word will take us to the word show page

#### Required API Endpoints
    - GET /api/words


### word show

#### url
/words/:id

#### Purpose
To show the details of a specific word.

#### components
-Japanese
-Romaji
-English
-Study Statistics
    -Correct count
    -Wrong count
-Word Groups
    - show a series of pills
    - when group name is clicked it will take us to the show page


#### Required API Endpoints
   
    - GET /api/words/:id/groups


### Word Groups Index

#### url
/groups

#### Purpose
To show a list of groups in our database
#### components
- groups paginated list
    -Columns
        - name
        - words count
    - clicking the group name will take us to the group show page

#### Required API Endpoints
    - GET /api/groups


### Group Show

#### url
/groups/:id

#### Purpose
To showthe information about a specific group

#### components
- group name
- group Statistics
    - Total words count
- words in group (Paginated ist of words)
    - should use the same component as the words index page
- Study sessions (Paginated List of study sessions)
    -should use the same component as the study sessions index page

#### Required API Endpoints
    -GET /api/groups/:id (the name and group stats)
    -GET /api/groups/:id/words
    -GET /api/groups/:id/study_sessions
    
### Study Sessions Index

#### url
/study_sessions

#### Purpose
To show a list of study sessions in our database
#### components
- study session paginated list
    -Columns
        - id
        - study activity name
        - group name
        - start time
        - end time
        - no of review items
    - Clicking the study session id will take us to the study session show page

#### Required API Endpoints
    - GET /api/study_sessions


### Study Session Show

#### url
/study_sessions/:id

#### Purpose
To show details of a specific study session
#### components
- study session id
    - study activity name
    - group name
    - start time
    - end time
    - no of review items
- Word Review Items (Paginated list of word review items)
    - should use the same component as the word review items index page

#### Required API Endpoints
    - GET /api/study_sessions/:id
    - GET /api/study_sessions/:id/words

### Settings

#### url
/settings

#### Purpose
To make configuration of the study portal.
#### components
- Theme Selection eg- Light, Dark, System Default
- reset history Button
    -this will delete all study sessions and word review items
- Full Reset Button
    - this will drop all tables and re-create them with seed data

#### Required API Endpoints
    - POST /api/settings/theme
    - POST /api/settings/reset_history
    - POST /api/settings/full_reset

`

### Page_Name

#### url

#### Purpose

#### components

#### Required API Endpoints
 
`