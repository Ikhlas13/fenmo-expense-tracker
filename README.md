# Expense Tracker

This project is a lightweight full-stack personal finance application developed as part of the assignment.

It includes:
- A backend API built using FastAPI
- A frontend interface using Streamlit
- SQLite database for persistent storage
- Safe expense creation using idempotency (to handle retries)
- Basic input validation, UI feedback (loading/error), and simple tests

---

## Running the Application Locally

### Install required packages:

pip install -r requirements.txt

### Start the backend server:

uvicorn backend.main:app --reload

### Launch the frontend (in a new terminal):

streamlit run streamlit_app.py

On Windows, you can also run `start_app.bat` to start both backend and frontend together.

If only the Streamlit app is started, it will automatically run an internal FastAPI server so the app can still function (useful for deployment on Streamlit Community Cloud).

By default, the frontend connects to:
http://127.0.0.1:8000

The database file is stored at:
data/expenses.db

---

## API Endpoints

### POST /expenses

Creates a new expense entry.

Example request:
{
  "amount": "125.50",
  "category": "Food",
  "description": "Lunch",
  "date": "2026-04-29"
}

You can include an Idempotency-Key header to make requests safe for retries.

- Same key + same data → returns the original expense (no duplicate)
- Same key + different data → returns 409 Conflict

---

### GET /expenses

Fetches all expenses.

Optional query parameters:
- category=Food (filter by category)
- sort=date_desc (sort by newest date first)

---

## Running Tests

pytest

The tests cover:
- Expense creation
- Retrieving expense list
- Category filtering
- Sorting by date
- Amount validation
- Idempotent request behavior

---

## Design Choices

- SQLite is used for persistence because it is simple, reliable, and suitable for local usage.
- Monetary values are stored in paise (amount_minor) as integers to avoid floating-point precision issues.
- API responses convert values back into rupees with two decimal places.
- Idempotency support ensures safe retries in cases like network failures, duplicate submissions, or page refreshes.

---

## Trade-offs

To keep the project focused within the given time:
- No authentication or user accounts
- No recurring expense feature
- No pagination
- No advanced deployment configuration

The frontend is intentionally simple using Streamlit components instead of a custom UI framework. Category filtering is based on exact matches from stored data.

---

## Deployment

The application is deployed on Streamlit Community Cloud:

https://fenmo-expense-tracker-btc9za7rsyrpxporp3f39j.streamlit.app/
