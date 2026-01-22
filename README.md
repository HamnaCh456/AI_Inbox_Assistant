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

## Key Components

### main.py
- FastAPI application setup with CORS middleware
- REST API endpoint definitions
- Request/response models using Pydantic
- Frontend static file serving

### email_fetcher_tool.py
- Gmail API service initialization and authentication
- Fetch unread email threads
- Parse and format email history with sender/subject information
- OAuth 2.0 token management

### draft_generator_tool.py
- Create Gmail draft messages
- Extract recipient and subject information from threads
- Mark threads as read after processing

### agent.py
- **RAGAgent class**: Manages RAG (Retrieval-Augmented Generation) using Gemini API
- **Knowledge Base Initialization**: Creates file search stores and uploads company knowledge base
- **File Search Integration**: Uses Google's file search API to retrieve relevant information
- **LLM Integration**: Communicates with Gemini AI model for response generation

### nexa_learn.txt
- Nexa Learn company information and FAQs
- Course offerings and career support programs
- Contact information and support details
- Used by RAG for generating accurate, company-specific responses

## How RAG Works

The system uses Retrieval-Augmented Generation (RAG) to provide accurate, company-specific responses:

1. **Knowledge Base Upload**: `nexa_learn.txt` is uploaded to Google's file search store during initialization
2. **Query Processing**: When an email arrives, the system creates a search query from the email content
3. **Information Retrieval**: The file search API retrieves relevant sections from the knowledge base
4. **Response Generation**: Gemini AI generates a professional response based on:
   - The customer's question
   - Retrieved company information
   - Professional customer service guidelines
5. **Output**: A professional, accurate reply ready for review or sending

## Error Handling

The API provides comprehensive error handling:
- Gmail API authentication errors with helpful messages
- Missing knowledge base file detection
- Invalid API key handling
- HTTP exceptions with detailed error information
- All errors logged to console for debugging

## Security Considerations

- **Credentials**: Stored securely in `credentials.json` and `token.json` (never commit these)
- **API Keys**: Managed through `.env` file (never commit to version control)
- **CORS**: Currently enabled for all origins (restrict in production)
- **File Access**: Requires proper OAuth scopes for Gmail access
- **Knowledge Base**: Only used internally, never exposed to clients

## Troubleshooting

### Gmail API Authentication Issues
- Ensure Gmail API is enabled in [Google Cloud Console](https://console.cloud.google.com/)
- Verify `credentials.json` is in the correct directory
- Delete `token.json` and restart to re-authenticate
- Check that OAuth consent screen is configured

### Knowledge Base Not Loading
- Verify `nexa_learn.txt` exists in the backend directory
- Check Gemini API key is valid and has file search capabilities
- Ensure GEMINI_API_KEY is set in `.env`
- Check console logs for file upload errors

### Gemini API Errors
- Verify API key is correct in `.env`
- Check that the API has file search enabled
- Ensure you have sufficient API quota
- Validate model name in `.env` (default: `gemini-2.0-flash`)

### Draft Generation Takes Too Long
- Check your internet connection
- Verify Gemini API is responsive
- Check if the knowledge base file is very large
- Monitor API usage limits

## Development Notes

- Backend runs on port 8001
- Frontend assets can be served from the same server
- Email threads are fetched as full conversation histories
- The 2-second delay in draft generation is intentional for UX feedback
- RAG store is created fresh on each backend restart (can be optimized for production)

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
