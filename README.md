# Encrypt Bytes CRM v3

Internal CRM for managing clients, tasks, payments, and team operations.

---

## What's New in v3

### Financial Tracking
- Total Revenue, Amount Received, Amount Pending per task
- Payment status: Unpaid / Partial / Paid / Overdue
- Payment due dates with overdue detection
- Dashboard financial KPIs and monthly revenue charts
- Collection rate percentage

### Task Enhancements
- **Priority levels**: Low / Medium / High / Critical
- **Task assignment** to specific admins
- **Tags** for categorization
- **Subtasks** with progress bar
- **Comments/Activity log** per task
- Overdue detection for both task and payment dates
- 3 views: Grid, Kanban (4-column), Table

### Admin Notifications
- In-app notification bell with unread count
- Notifications for: task created, assigned, status changed, completed, comment added, task overdue, payment due
- Mark all read, clear read notifications
- Clicking notification navigates to task
- Auto-check overdue every 60 seconds

### Client Enhancements
- Added: address, industry, website, source fields
- Industry filter on clients page
- Color-coded avatar cards

### Dashboard
- Financial KPIs row + Task KPIs row
- Alert strip (overdue tasks, payment overdue, critical tasks)
- Revenue vs Received monthly bar chart
- Status pie, Payment pie, Priority distribution pie
- Top 10 clients by tasks
- Admin workload chart

---

## Quick Start

### 1. Backend

```bash
cd backend

# Create & activate venv (Windows)
python -m venv venv
venv\Scripts\activate
# (venv) must appear in your prompt before next step

pip install -r requirements.txt

# Copy and edit env
copy .env.example .env
# Edit .env and set your MongoDB URL

uvicorn app.main:app --reload --port 8000
```

### 2. Frontend

```bash
cd frontend
npm install

# Copy env
copy .env.example .env

npm run dev
# → http://localhost:5173
```

### 3. First Run

1. Go to http://localhost:5173/register — create your account as `super_admin`
2. Go to **Domains** → click **Seed Defaults** (adds all EB Labs + EB Academy services)
3. Go to **Clients** → add your clients
4. Go to **Tasks** → create tasks linked to clients
5. Dashboard populates automatically

---

## Environment Variables

### backend/.env
```
MONGODB_URL=mongodb+srv://<user>:<pass>@<cluster>.mongodb.net/<dbname>?retryWrites=true&w=majority
DATABASE_NAME=encrypt_bytes_crm
SECRET_KEY=your-super-secret-key-here
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=1440
```

### frontend/.env
```
VITE_API_URL=http://localhost:8000
```

---

## MongoDB Collections

| Collection | Purpose |
|---|---|
| users | Internal admins (admin / super_admin) |
| clients | Customer/client records |
| domains | EB Labs / EB Academy services |
| tasks | Main task records |
| subtasks | Nested under tasks |
| comments | Activity log per task |
| notifications | Per-user notification feed |

---

## API Endpoints

- `POST /api/auth/register` — create account
- `POST /api/auth/login` — login, returns JWT
- `GET/POST/PUT/DELETE /api/clients/`
- `GET/POST/PUT/DELETE /api/domains/` + `POST /api/domains/seed`
- `GET/POST/PUT/DELETE /api/tasks/`
- `GET /api/tasks/stats` — dashboard analytics
- `GET /api/tasks/overdue` — overdue tasks
- `GET /api/tasks/{id}/subtasks` + CRUD
- `GET /api/tasks/{id}/comments` + CRUD
- `GET /api/notifications/`
- `GET /api/notifications/unread-count`
- `PUT /api/notifications/mark-all-read`
- `DELETE /api/notifications/clear`
- `POST /api/notifications/check-overdue`

---

## Tech Stack

- **Frontend**: React 18, Vite, React Router 6, Recharts, Axios
- **Backend**: FastAPI, Motor (async MongoDB), Pydantic v2, JWT auth
- **Database**: MongoDB Atlas
- **Fonts**: Syne + Mulish + JetBrains Mono

---

## Common Issues

### bcrypt error on login/register
```bash
pip install bcrypt==4.0.1
```
This pins bcrypt to the version compatible with passlib 1.7.4.

### 401 on all requests
Token expired or invalid. Log out and log back in.

### CORS error
Make sure `VITE_API_URL` in frontend `.env` matches your backend URL exactly.
