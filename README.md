# MailMind AI

MailMind AI is a full-stack AI-powered email automation platform that connects with Gmail, analyzes incoming messages, classifies their intent and urgency, generates summaries, and creates context-aware replies using Groq.

The project was developed as part of the Optimus Automate Virtual Internship Program.

## Overview

Managing a busy inbox can be time-consuming. MailMind AI helps users understand, organize, and respond to emails more efficiently.

The platform connects to Gmail, imports emails, analyzes their content using Groq, and generates editable reply suggestions in different tones.

MailMind AI does not automatically send AI-generated replies. The user must review and confirm every draft or outgoing message.

## Features

### User Authentication

- User registration
- Secure login
- JWT-based authentication
- Protected frontend routes
- Password reset support
- User profile management
- Profile picture upload

### Gmail Integration

- Gmail OAuth 2.0 authentication
- Connect and disconnect Gmail accounts
- Import Gmail messages
- Manual Gmail synchronization
- Scheduled Gmail synchronization
- Duplicate email prevention
- Gmail draft creation
- Connected account status
- Last synchronization time
- Imported email count

### AI Email Analysis

Groq is used to analyze email content and generate:

- Intent classification
- Urgency detection
- Email summaries
- Suggested reply subjects
- Spam classification
- Spam reasoning
- Context-aware email replies

### AI Reply Generation

MailMind AI supports multiple reply tones:

- Professional
- Friendly
- Apologetic
- Short

Users can:

- Generate a reply
- Change the reply tone
- Regenerate a different version
- Edit the generated reply
- Copy the reply
- Save it as a draft
- Create a Gmail draft

### Mailbox Organization

Emails are organized into different sections:

- Inbox
- Urgent
- Spam
- Drafts
- Compose

The application also supports:

- Email search
- Intent filters
- Urgency filters
- Read and unread states
- Email category badges
- AI provider badges

### Dashboard

The dashboard displays:

- Total inbox emails
- Urgent email count
- Spam email count
- Draft count
- Gmail connection status
- Groq configuration status
- Recent messages
- Last Gmail synchronization time

## AI Workflow

```text
Incoming Email
      ↓
Gmail Synchronization
      ↓
Groq Email Analysis
      ↓
Intent and Urgency Detection
      ↓
Summary and Subject Suggestion
      ↓
Groq Reply Generation
      ↓
User Review and Editing
      ↓
Save as Draft or Create Gmail Draft
```

## Groq Integration

Groq is used for all AI-related functionality, including:

- Email classification
- Intent detection
- Urgency analysis
- Summary generation
- Suggested subject generation
- Spam reasoning
- Reply generation
- Tone-based rewriting
- Reply regeneration

Local rule-based replies are only used as an emergency fallback when Groq is unavailable or returns an invalid response.

Example AI response:

```json
{
  "intent": "Inquiry",
  "urgency": "Low",
  "summary": "The sender is requesting information about the MailMind AI platform.",
  "suggested_subject": "Re: MailMind AI Information",
  "reply": "Hello,\n\nThank you for reaching out...",
  "analysis_provider": "groq",
  "reply_provider": "groq"
}
```

## Technology Stack

### Frontend

- React
- Vite
- React Router
- Axios
- Radix UI
- Lucide React
- CSS

### Backend

- Python
- FastAPI
- SQLAlchemy
- SQLite
- Pydantic
- JWT Authentication
- APScheduler
- Groq SDK
- Google Gmail API
- Google OAuth 2.0

## Project Structure

```text
AI-Powered Email Responder/
│
├── backend/
│   ├── app/
│   │   ├── core/
│   │   ├── models/
│   │   ├── routes/
│   │   ├── schemas/
│   │   ├── services/
│   │   └── utils/
│   │
│   ├── credentials/
│   ├── uploads/
│   ├── main.py
│   ├── requirements.txt
│   ├── .env
│   └── .env.example
│
└── Frontend/
    └── Kimi_Agent_MailMind AI Frontend Design/
        └── app/
            ├── src/
            │   ├── components/
            │   ├── pages/
            │   ├── services/
            │   ├── context/
            │   └── utils/
            │
            ├── package.json
            └── vite.config.js
```

## Prerequisites

Install the following before running the project:

- Python 3.11 or newer
- Node.js 18 or newer
- npm
- Git
- A Groq API key
- A Google Cloud project
- Gmail API enabled in Google Cloud

## Backend Setup

Navigate to the backend directory:

```bash
cd backend
```

Create a virtual environment:

```bash
python -m venv venv
```

Activate the virtual environment on Windows:

```bash
venv\Scripts\activate
```

Install backend dependencies:

```bash
python -m pip install -r requirements.txt
```

Create a `.env` file inside the backend directory.

Example:

```env
GROQ_API_KEY=your_private_groq_api_key
GROQ_MODEL=llama-3.1-8b-instant

DATABASE_URL=sqlite:///./mailmind.db

JWT_SECRET_KEY=replace_with_a_long_random_secret
JWT_ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=1440

FRONTEND_URL=http://localhost:3000

GMAIL_CLIENT_SECRET_FILE=credentials/client_secret.json
GMAIL_REDIRECT_URI=http://localhost:8000/api/gmail/callback

ENABLE_GMAIL_POLLING=true
GMAIL_POLL_INTERVAL_MINUTES=5
```

Start the backend server:

```bash
uvicorn main:app --host 0.0.0.0 --port 8000
```

The backend will run at:

```text
http://localhost:8000
```

FastAPI documentation will be available at:

```text
http://localhost:8000/docs
```

## Frontend Setup

Navigate to the frontend application directory:

```bash
cd "Frontend/Kimi_Agent_MailMind AI Frontend Design/app"
```

Install dependencies:

```bash
npm install
```

Start the frontend:

```bash
npm run dev
```

The frontend will run at:

```text
http://localhost:3000
```

## Google Gmail API Setup

### 1. Create a Google Cloud Project

Create a new project in Google Cloud Console.

Suggested project name:

```text
MailMind AI
```

### 2. Enable Gmail API

Open:

```text
APIs & Services
→ Library
→ Gmail API
→ Enable
```

### 3. Configure OAuth Consent Screen

Configure the OAuth consent screen with:

```text
App name: MailMind AI
User type: External
```

Add your Gmail account as a test user while the application remains in testing mode.

### 4. Add Gmail Scopes

Required scopes:

```text
https://www.googleapis.com/auth/gmail.readonly
https://www.googleapis.com/auth/gmail.compose
```

Optional sending scope:

```text
https://www.googleapis.com/auth/gmail.send
```

### 5. Create OAuth Credentials

Create an OAuth 2.0 Client ID using the Web Application type.

Authorized JavaScript origin:

```text
http://localhost:3000
```

Authorized redirect URI:

```text
http://localhost:8000/api/gmail/callback
```

Download the Google OAuth credentials file and save it inside:

```text
backend/credentials/client_secret.json
```

Do not upload this file to GitHub.

## Important API Endpoints

### Authentication

```text
POST /api/auth/signup
POST /api/auth/login
GET  /api/auth/me
POST /api/auth/forgot-password
POST /api/auth/reset-password
```

### Email Management

```text
GET  /api/emails
GET  /api/emails/{email_id}
POST /api/analyze-email
POST /api/emails/{email_id}/analyze
POST /api/emails/{email_id}/generate-reply
```

### Gmail Integration

```text
GET  /api/gmail/status
GET  /api/gmail/connect
GET  /api/gmail/callback
POST /api/gmail/sync
POST /api/gmail/disconnect
POST /api/gmail/create-draft
```

### Draft Management

```text
GET    /api/drafts
POST   /api/drafts
GET    /api/drafts/{draft_id}
PUT    /api/drafts/{draft_id}
DELETE /api/drafts/{draft_id}
```

### Profile

```text
GET    /api/profile
PUT    /api/profile
POST   /api/profile/picture
DELETE /api/profile/picture
```

### Dashboard and Health

```text
GET /api/mailbox/counts
GET /api/health/ai
```

## AI Health Check

The Groq configuration can be checked using:

```text
GET http://localhost:8000/api/health/ai
```

Expected response:

```json
{
  "configured": true,
  "groq_api_configured": true,
  "provider": "groq",
  "model": "llama-3.1-8b-instant"
}
```

The API key is never returned to the frontend.

## Reply Source Handling

When Groq successfully generates a reply:

```json
{
  "reply_provider": "groq",
  "reply_generated": true
}
```

When the fallback system is used:

```json
{
  "reply_provider": "local_rules",
  "reply_generated": true
}
```

The frontend displays the reply source so users can see whether the response was created by Groq or by the fallback system.

## Scheduled Gmail Synchronization

MailMind AI uses APScheduler to check for new Gmail messages automatically.

Default synchronization interval:

```text
5 minutes
```

The interval can be changed using:

```env
GMAIL_POLL_INTERVAL_MINUTES=5
```

The scheduler starts when the FastAPI application starts.

## Security

The following files must never be uploaded to GitHub:

```gitignore
.env
*.env
!.env.example

credentials/
client_secret*.json
token.json

*.db
uploads/
__pycache__/
.pytest_cache/
node_modules/
dist/
```

Security measures include:

- JWT authentication
- Password hashing
- Protected API routes
- User-specific email ownership
- User-specific draft ownership
- Backend-only API keys
- OAuth token protection
- Explicit confirmation before sending messages

Never place the Groq API key or Gmail client secret inside frontend code.

## Testing

Run backend tests:

```bash
python -m pytest
```

Run the frontend development server:

```bash
npm run dev
```

Test the production frontend build:

```bash
npm run build
```

## Manual Testing Flow

1. Create a MailMind AI account
2. Log in
3. Open the Integrations page
4. Connect Gmail
5. Synchronize Gmail
6. Open the Inbox
7. Select an email
8. Analyze the email
9. Generate a reply
10. Change the reply tone
11. Regenerate the reply
12. Save it as a draft
13. Open the Drafts page
14. Create a Gmail draft
15. Verify the draft in Gmail

## Troubleshooting

### Groq is configured but local replies are used

Check the backend terminal for:

```text
Groq reply failed validation
```

Groq may be returning a successful API response that is later rejected by the reply validator.

Check:

- Placeholder detection
- Duplicate greeting detection
- Duplicate closing detection
- HTML detection
- Tracking URL detection
- Reply length validation

### Generate Reply returns an empty response

An invalid response may look like:

```json
{
  "reply": "",
  "reply_generated": true
}
```

The backend should only set `reply_generated` to `true` when the reply contains text.

Correct logic:

```python
reply_generated = bool(reply.strip())
```

### Gmail OAuth redirect mismatch

Confirm that this exact URI exists in both Google Cloud and the backend configuration:

```text
http://localhost:8000/api/gmail/callback
```

### Unauthorized API requests

Log out and log back in to obtain a new JWT access token.

### Gmail scheduled sync returns a history error

If Gmail returns a `404` for an old history ID, the backend should clear the stored history ID and perform a recent synchronization while preventing duplicate messages.

## Demo Recording Order

Recommended order for presenting the application:

1. Landing page
2. Login
3. Dashboard
4. Integrations page
5. Gmail synchronization
6. Inbox
7. Open an email
8. Groq email analysis
9. Groq reply generation
10. Tone selection
11. Reply regeneration
12. Save Draft
13. Gmail draft creation
14. Urgent emails
15. Spam emails
16. Compose page
17. Profile
18. Settings
19. Final dashboard view

Use test emails during demonstrations to avoid exposing private information.

## Future Improvements

- Gmail push notifications using Google Cloud Pub/Sub
- Multiple Gmail account support
- Email thread summarization
- Attachment analysis
- Team collaboration
- Advanced spam detection
- PostgreSQL database support
- Cloud deployment
- AI usage analytics
- Reply quality evaluation
- Additional AI providers

## Internship Task Coverage

MailMind AI fulfills the AI-Powered Email Responder internship task by providing:

- Incoming email analysis
- AI-generated email replies
- Groq API integration
- Email intent classification
- Urgency detection
- Spam detection
- Gmail API integration
- Gmail draft creation
- Full frontend and backend implementation
- Authentication and user management
- Scheduled email synchronization

## Author

**Muhammad Ahmad Muzi**

Developed as part of the Optimus Automate Virtual Internship Program.

## License

This project is intended for educational, portfolio, and internship demonstration purposes.
