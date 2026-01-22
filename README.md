# Email Agent Backend

An intelligent email reply automation system powered by FastAPI and Gemini AI with Retrieval-Augmented Generation (RAG). This backend service automatically fetches unread emails from Gmail, generates professional replies using a company knowledge base, and manages email drafts.

# Demo
[![Watch the video](https://img.youtube.com/vi/-Wnwh3nbpDI/0.jpg)](https://youtu.be/-Wnwh3nbpDI)

## Features

- **Gmail Integration**: Seamlessly fetch and parse unread email threads from Gmail
- **RAG-Powered Draft Generation**: Automatically generate professional replies using Gemini AI with company knowledge base search
- **Knowledge Base Search**: Leverage Nexa Learn company information for contextual, accurate responses
- **Email Thread Management**: Fetch, organize, and track email conversations
- **Draft Creation**: Create Gmail draft messages without sending so the user can edit 
- **Email Sending**: Send generated replies directly through Gmail 


## Prerequisites

- Python 3.11 or higher
- Gmail account with API access enabled
- Google Cloud Project with Gmail API enabled
- Gemini API key (with file search capabilities)
- `uv` package manager or pip

## Installation

### 1. Clone the Repository
```bash
git clone https://github.com/HamnaCh456/AI_Inbox_Assistant.git
cd email_agent_backend
```

### 2. Create Virtual Environment
```bash
python -m venv create
source create/Scripts/activate  # On Windows
# or
source create/bin/activate  # On macOS/Linux
```

### 3. Install Dependencies
```bash
uv pip install -e .
# or
pip install -e .
```

### 4. Setup Credentials

#### Gmail API Setup:
1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project
3. Enable the Gmail API
4. Create OAuth 2.0 credentials (Desktop application)
5. Download credentials as `credentials.json` and place in the backend directory

#### Gemini API Setup:
1. Get your API key from [Google AI Studio](https://aistudio.google.com/apikey)
2. Create a `.env` file in the backend directory with your API key

### 5. Environment Configuration
Create a `.env` file in the backend directory:
```env
GEMINI_API_KEY=your_gemini_api_key_here
GEMINI_MODEL=gemini-2.0-flash
```

## Configuration

### Key Files

- `credentials.json` - Gmail API OAuth credentials (auto-generated on first run)
- `token.json` - Gmail API access token (auto-generated after authentication)
- `.env` - Environment variables (Gemini API key and model selection)
- `pyproject.toml` - Project configuration and dependencies
- `nexa_learn.txt` - Company knowledge base for RAG

## Running the Server

### Development Mode
```bash
uv run uvicorn main:app --host 0.0.0.0 --port 8001 --reload
```

### Production Mode
```bash
uv run uvicorn main:app --host 0.0.0.0 --port 8001
```

The server will start at `http://localhost:8001`

## Project Structure

```
email_agent_backend/
├── main.py                   # FastAPI application & API endpoints
├── agent.py                  # RAG agent with Gemini AI configuration
├── draft_generator_tool.py   # Gmail draft creation logic
├── email_fetcher_tool.py     # Gmail API integration & thread fetching
├── email_sender.py           # Email sending functionality
├── nexa_learn.txt            # Company knowledge base for RAG
├── credentials.json          # Gmail OAuth credentials (auto-generated)
├── token.json                # Gmail API token (auto-generated)
├── pyproject.toml            # Project dependencies & configuration
├── uv.lock                   # Dependency lock file
├── .python-version           # Python version specification
└── README.md                 # This file
```

## Dependencies

Key packages:
- `fastapi>=0.100.0` - Web framework
- `uvicorn>=0.23.0` - ASGI server
- `google-api-python-client>=2.188.0` - Gmail API client
- `google-auth-oauthlib>=1.2.4` - OAuth authentication for Gmail
- `google-genai>=1.59.0` - Gemini API client with file search support
- `pydantic` - Data validation
- `python-dotenv>=1.0.0` - Environment variable management

See `pyproject.toml` for complete dependency list.
