# Mergington High School Activities API

A super simple FastAPI application that allows students to view extracurricular activities and teachers to manage registrations.

## Features

- View all available extracurricular activities
- Teacher-only registration and unregistration
- Teacher login with short-lived sessions

## Getting Started

1. Install the dependencies:

   ```
   pip install fastapi uvicorn
   ```

2. Configure the teacher credentials in the environment (do not commit credentials):

   ```
   export TEACHER_USERNAME="teacher"
   export TEACHER_PASSWORD="replace-with-a-strong-password"
   ```

3. Run the application:

   ```
   uvicorn src.app:app --reload
   ```

4. Open your browser and go to:
   - Activities page: http://localhost:8000
   - API documentation: http://localhost:8000/docs
   - Alternative documentation: http://localhost:8000/redoc

## API Endpoints

| Method | Endpoint                                                             | Description                                                         |
| ------ | -------------------------------------------------------------------- | ------------------------------------------------------------------- |
| GET    | `/activities`                                                        | Get all activities with their details and current participant count |
| POST   | `/auth/login`                                                        | Log in with the configured teacher credentials                      |
| POST   | `/auth/logout`                                                       | Log out and invalidate the current teacher session                  |
| POST   | `/activities/{activity_name}/signup?email=student@mergington.edu`    | Register a student (teacher session required)                       |
| DELETE | `/activities/{activity_name}/unregister?email=student@mergington.edu` | Unregister a student (teacher session required)                    |

Teacher sessions expire after eight hours and are invalidated when the server
restarts. Students can view activities and participants without logging in,
but registration changes require a teacher session. The API enforces this
restriction independently of the UI.

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

All data is stored in memory, which means data will be reset when the server restarts.
