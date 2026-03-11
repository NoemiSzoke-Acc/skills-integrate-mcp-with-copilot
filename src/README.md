# Mergington High School Activities API

A super simple FastAPI application that allows students to view and sign up for extracurricular activities.

## Features

- View all available extracurricular activities
- Sign up for activities

## Getting Started

1. Install the dependencies:

   ```
   pip install fastapi uvicorn
   ```

2. Run the application:

   ```
   python app.py
   ```

3. Open your browser and go to:
   - API documentation: http://localhost:8000/docs
   - Alternative documentation: http://localhost:8000/redoc

## API Endpoints

| Method | Endpoint                                                          | Description                                                         |
| ------ | ----------------------------------------------------------------- | ------------------------------------------------------------------- |
| GET    | `/activities`                                                     | Get all activities with their details and current participant count |
| POST   | `/activities/{activity_name}/signup?email=student@mergington.edu` | Sign up for an activity                                             |
| POST   | `/activities/import` (multipart/form-data CSV upload)            | Import activities from a CSV file and persist (see format below)    |

## Data Model

The application uses a simple data model with meaningful identifiers:

1. **Activities** - Uses activity name as identifier:

   - Description
   - Schedule
   - Maximum number of participants allowed
   - List of student emails who are signed up

2. **Students** - Uses email as identifier:
   - Name
   - Grade level

Activity data is loaded from `activities.json` if present and is
written back to that file whenever new activities are added or imported.
This gives the application a basic, file‑based persistence layer so that
data survives restarts and can be updated without editing the source code.

### CSV import format

The `/activities/import` endpoint accepts a UTF-8 CSV file with these headers:

```
name,description,schedule,max_participants,participants
```

* `name` is required and serves as the activity identifier.
* `participants` may be a comma‑ or semicolon‑separated list of emails.

Rows matching an existing activity will update it; otherwise a new
activity record will be created.  This simple pipeline lets administrators
prepare spreadsheets and push them into the system without touching code.
