# n8n Workflows - Second Brain System

A sophisticated AI-powered personal assistant system built with n8n, featuring autonomous agents, memory management, calendar integration, and task automation.

## 🏗️ System Architecture

### Core Agent
- **Second Brain** - Main orchestration agent that routes requests to specialized tools based on user intent

### Sub-Agents
- **Daily Readiness Marshal** - Generates personalized daily readiness reports based on recovery metrics and schedule
- **Sub-Agent: Whoop Recovery** - Fetches recovery data from Whoop API

### Tools
- **Link Reader** - Extracts and reads webpage content
- **Web Search** - Performs real-time Google searches
- **Calendar View** - Retrieves Google Calendar events
- **Calendar Schedule** - Creates new calendar events with conflict checking
- **Add Task** - Creates tasks in Todoist
- **Memory Add** - Stores information in vector database (Pinecone)
- **Memory Search** - Retrieves information from vector database

## 🔄 Workflow Relationships

```
Second Brain (Main Agent)
├── Tools
│   ├── Link Reader
│   ├── Web Search
│   ├── Calendar View
│   ├── Calendar Schedule
│   ├── Add Task
│   ├── Memory Add
│   └── Memory Search
└── Sub-Agents
    ├── Daily Readiness Marshal
    │   └── Sub-Agent: Whoop Recovery
    └── Sub-Agent: Whoop Recovery
```

## 📋 Prerequisites

### Required Services
1. **n8n** (self-hosted or cloud)
2. **Google Gemini API** - For AI language model
3. **Pinecone** - Vector database for memory storage
4. **Google Calendar API** - Calendar integration
5. **Todoist API** - Task management
6. **SerpAPI** - Web search functionality
7. **Telegram Bot** - User interface
8. **Whoop API** - Recovery/fitness data (optional)

### Required n8n Nodes
- `@n8n/n8n-nodes-langchain` - LangChain integration
- Base n8n nodes (HTTP Request, Code, Set, etc.)
- Community nodes may be required depending on your n8n version

## 🚀 Setup Instructions

### 1. Import Workflows
1. Navigate to your n8n instance
2. Go to **Workflows** → **Import from File**
3. Import workflows in this order:
   - All tool workflows first (`workflows/tools/*.json`)
   - Sub-agents next (`workflows/agents/Sub-Agent*.json`)
   - Main agent last (`workflows/agents/Second Brain.json`)

### 2. Configure Credentials
Set up the following credentials in n8n:

#### Google Gemini API
- Type: `Google PaLM API`
- Obtain API key from [Google AI Studio](https://makersuite.google.com/app/apikey)

#### Pinecone
- Type: `Pinecone API`
- Create account at [Pinecone](https://www.pinecone.io/)
- Create index named `second-brain` with dimensions matching your embedding model

#### Google Calendar
- Type: `Google Calendar OAuth2`
- Follow [Google Cloud Console setup](https://console.cloud.google.com/)

#### Todoist
- Type: `Todoist API`
- Get API token from Todoist Settings → Integrations

#### SerpAPI
- Type: `SerpAPI`
- Get API key from [SerpAPI](https://serpapi.com/)

#### Telegram
- Type: `Telegram API`
- Create bot via [@BotFather](https://t.me/botfather)

#### Whoop (Optional)
- Type: `OAuth2 API`
- Register app at [Whoop Developer Portal](https://developer.whoop.com/)

### 3. Update Personal Configuration

Replace placeholders in the workflow files:

- `YOUR_EMAIL@example.com` → Your actual email
- `YOUR_TELEGRAM_CHAT_ID` → Your Telegram chat ID (get from [@userinfobot](https://t.me/userinfobot))

### 4. Configure Timezone
The system is configured for `Asia/Singapore` timezone. Update in:
- Second Brain workflow settings
- Calendar-related workflows
- Daily Readiness Marshal

### 5. Activate Workflows
1. Ensure all tool workflows are **active**
2. Activate sub-agent workflows
3. Finally, activate the Second Brain workflow

## 💡 Usage

### Telegram Commands

The Second Brain agent responds to natural language commands via Telegram:

#### Calendar Management
- "What's on my schedule today?"
- "Schedule a meeting with John tomorrow at 3pm"
- "Do I have any conflicts on Friday afternoon?"

#### Task Management
- "Remind me to buy groceries"
- "Add task: Email the report by 5pm"

#### Memory Operations
- "Remember that I prefer morning meetings"
- "What is my favorite restaurant?"

#### Web Search & Research
- "What's the weather in Singapore?"
- "Search for latest AI news"
- "Summarize this article: [URL]"

#### Daily Reports
- "Run my daily readiness report"
- "Show my recovery status"

### Automated Features
- **Daily Readiness Report**: Automatically triggered at 10:30 AM (configurable via Schedule Trigger)
- **Memory Auto-Save**: Automatically stores important information from conversations
- **Conflict Detection**: Automatically checks for calendar conflicts before scheduling

## 🛠️ Customization

### Adjust AI Behavior
Edit the system prompt in the **Second Brain** workflow's AI Agent node to modify:
- Response tone and style
- Decision-making logic
- Tool selection criteria

### Modify Schedule Trigger
In **Second Brain** workflow:
- Adjust the Schedule Trigger node to change when daily reports are sent

### Extend Functionality
Add new tools by:
1. Creating a new workflow with `Execute Workflow Trigger`
2. Adding it as a Tool Workflow node in the Second Brain agent
3. Updating the system prompt to include the new capability

## 🔒 Security Notes

- The JSON files in this repository contain **credential IDs only**, not actual secrets
- Actual credentials are encrypted in your n8n database
- Personal information (email, chat ID) has been redacted - update these after import
- The n8n instance ID is specific to your installation and is not sensitive

## 🧩 Technical Details

### Memory System
- Uses **Pinecone vector store** for semantic search
- Embeddings generated via **Google Gemini**
- Text chunked using recursive character splitter (500 chars, 50 overlap)

### AI Model
- Primary model: **Gemini 2.0 Flash Preview**
- Memory window: 12-15 messages (context-dependent)
- HTML formatting for Telegram compatibility

### Calendar Integration
- Timezone: Asia/Singapore
- ISO 8601 format for date/time inputs
- Automatic conflict detection

## 📊 Workflow Statistics

- **Total Workflows**: 10
- **Tool Workflows**: 7
- **Agent Workflows**: 3
- **API Integrations**: 7+
- **AI Nodes**: Multiple LangChain agents with memory

## 🐛 Troubleshooting

### Common Issues

**Workflow not executing**
- Ensure all sub-workflows are active
- Check credential configuration
- Verify workflow IDs match in tool references

**Memory not working**
- Confirm Pinecone index exists and is named correctly
- Check embedding model credentials
- Verify dimensions match (typically 768 or 1536)

**Calendar errors**
- Validate timezone settings
- Ensure ISO 8601 date format
- Check Google Calendar API quota

**Telegram not responding**
- Verify bot token is correct
- Ensure webhook is not conflicting
- Check chat ID is accurate

## 📝 License

These workflows are provided as-is for personal use. Please review and comply with the terms of service for all integrated third-party APIs.

## 🤝 Contributing

Feel free to fork, modify, and submit pull requests with improvements or additional tools!

## ⚠️ Disclaimer

This system handles personal data (calendar, tasks, memories). Ensure you:
- Keep your n8n instance secure
- Use strong credentials
- Regularly backup your workflows
- Review API access permissions