# mini_project — DBMS Query Convertor

Talk to your SQL database in plain English.

Getting answers out of a SQL database usually means writing long, complex queries by hand. This project removes that step: the user asks a question in natural language, and the system **generates the SQL, executes it against the database, and returns the results**.

> ⚠️ **Local use only.** This tool runs generated SQL directly against a real database. For security reasons it is designed to run on your own machine and **must not be deployed** to a public server or exposed to the internet.

---

## How it works

```
 User question (natural language)
            │
            ▼
   ┌─────────────────┐
   │ React frontend  │  — chat-style input, results table
   └────────┬────────┘
            │ HTTP (localhost)
            ▼
   ┌─────────────────┐
   │ FastAPI backend │  1. understand the question
   │                 │  2. generate SQL
   │                 │  3. execute SQL
   └────────┬────────┘
            │
            ▼
   ┌─────────────────┐
   │  SQL database   │
   └─────────────────┘
            │
            ▼
   Results returned to the user
```

**Example**

| You ask | System generates |
| --- | --- |
| "Show me the top 5 customers by total orders" | `SELECT customer_name, COUNT(*) AS total_orders FROM orders GROUP BY customer_name ORDER BY total_orders DESC LIMIT 5;` |

---

## Tech stack

| Layer | Technology |
| --- | --- |
| Frontend | React + Vite |
| Backend | FastAPI (Python) |
| Database | SQL database |

---

## Project structure

```
mini_project/
├── backend/
│   ├── main.py            # FastAPI app (entry point)
│   ├── requirements.txt   # Python dependencies
│   └── .gitignore
├── frontend/
│   ├── src/               # React source code
│   ├── public/
│   ├── index.html
│   ├── package.json
│   └── vite.config.js
└── README.md
```

---

## Getting started

### Prerequisites

- Python 3.11+
- Node.js 18+ and npm
- A SQL database running locally

### 1. Backend (FastAPI)

```bash
cd backend

# create and activate a virtual environment
python -m venv venv
venv\Scripts\activate        # Windows
# source venv/bin/activate   # macOS / Linux

# install dependencies
pip install -r requirements.txt

# start the server
uvicorn main:app --reload
```

The backend runs at **http://127.0.0.1:8000**.

- Health check: http://127.0.0.1:8000/health → `{"status": "ok"}`
- Interactive API docs: http://127.0.0.1:8000/docs

### 2. Frontend (React)

```bash
cd frontend
npm install
npm run dev
```

The frontend runs at the URL Vite prints (by default **http://localhost:5173**).

---

## API

| Method | Endpoint | Description |
| --- | --- | --- |
| `GET` | `/health` | Checks that the backend is running |

More endpoints (natural-language query → SQL → results) will be added as the project grows.

---

## Security notes

Because the system runs AI-generated SQL against a real database:

- Run it **only on localhost**. Do not deploy it or open it to a network.
- Use a database user with **read-only** permissions where possible.
- Keep credentials in a `.env` file (already git-ignored). Never commit them.
- Point it at a test or copy database, not production data.

---

## Roadmap

- [x] FastAPI backend with health endpoint
- [x] React frontend scaffold
- [ ] Database connection
- [ ] Natural language → SQL generation
- [ ] Safe SQL execution and result retrieval
- [ ] Frontend UI for asking questions and viewing results
