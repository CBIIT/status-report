# JIRA to DOCX Automation

Automated system for retrieving JIRA issues, summarizing them withpython3 jira_automation.py
```

### Scheduled Automation

python3 -m venv venv
source venv/bin/activate

For automated scheduled runs:

```bash
# Make automation script executable
chmod +x run_automation.sh

# Run manually
./run_automation.sh

# Or set up cron job (see cron_examples.txt)
crontab -eOllama LLM, and generating Word document reports.

## Features

- Fetch JIRA issues using JQL queries
- AI-powered summarization using Ollama (local LLM)
- Generate professional Word document reports
- Configurable via environment variables
- Error handling and validation

## Prerequisites

1. **Python 3.9+**
2. **Ollama** running locally with `llama3` model
3. **JIRA Personal Access Token**

## Quick Setup

### Option 1: Automated Setup

```bash
# Run the setup script
./setup.sh
```

### Option 2: Manual Setup

#### 1. Install Dependencies

```bash
pip install -r requirements.txt
```

#### 2. Configure Environment

Copy the example configuration and edit it:

```bash
cp .env.example .env
# Then edit .env with your actual values
```

Or edit the `.env` file directly with your JIRA credentials:

```env
JIRA_TOKEN=your_actual_token_here
JIRA_URL=https://yourdomain.atlassian.net
JIRA_JQL=project = "YOUR_PROJECT" AND updated >= "2025-07-01"
```

**How to get JIRA Token:**
1. Go to JIRA → Account Settings → Security → API tokens
2. Create new token
3. Copy the token to `.env` file

#### 3. Setup Ollama

Install and run Ollama with llama3 model:

```bash
# Install Ollama (if not already installed)
curl -fsSL https://ollama.ai/install.sh | sh

# Pull llama3 model
ollama pull llama3

# Start Ollama service
ollama serve
```

## Usage

### Validate Configuration

Before running the automation, validate your setup:

```bash
python3 validate_config.py
```

### Test Connections

Test JIRA and Ollama connectivity:

```bash
python3 test_setup.py
```

### Run the Automation

```bash
python jira_automation.py
```

### Expected Output

1. Fetches issues from JIRA based on your JQL query
2. Processes each issue through Ollama for AI summarization
3. Generates `JIRA_Summary_Report.docx` with formatted results

### Sample JQL Queries

```bash
# Recent issues from specific project
project = "ABC" AND updated >= "2025-07-01"

# Open bugs with high priority
project = "ABC" AND status != "Done" AND priority = "High"

# Issues assigned to you
assignee = currentUser() AND status != "Done"

# Issues created in last 30 days
created >= -30d
```

## File Structure

```
├── jira_automation.py          # Main automation script
├── requirements.txt            # Python dependencies
├── .env                       # Environment configuration
├── .env.example               # Example configuration template
├── README.md                  # This file
├── setup.sh                   # Automated setup script
├── validate_config.py         # Configuration validator
├── test_setup.py              # Connection tester
├── run_automation.sh          # Automation runner with logging
├── cron_examples.txt          # Cron job examples
├── logs/                      # Automation logs (created automatically)
├── reports/                   # Timestamped report backups (created automatically)
└── JIRA_Summary_Report.docx   # Generated report (after running)
```

## Configuration Options

### Environment Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `JIRA_TOKEN` | Your JIRA Personal Access Token | `ATBBxx...` |
| `JIRA_URL` | Your JIRA instance URL | `https://company.atlassian.net` |
| `JIRA_JQL` | JQL query to filter issues | `project = "ABC"` |

### Customization

You can modify the script to:
- Change the Ollama model (line 16)
- Adjust the maximum number of results (line 50)
- Customize the Word document formatting
- Add additional JIRA fields

## Troubleshooting

### Common Issues

1. **Import errors**: Run `pip install -r requirements.txt`
2. **JIRA authentication**: Check your token and URL in `.env`
3. **Ollama connection**: Ensure Ollama is running on `localhost:11434`
4. **No issues found**: Verify your JQL query returns results in JIRA

### Error Messages

- `JIRA_TOKEN must be set`: Update your `.env` file with actual token
- `Error connecting to Ollama`: Start Ollama service
- `JIRA API error`: Check your JIRA URL and token permissions

### Testing Connection

Test JIRA connection:
```bash
curl -H "Authorization: Bearer YOUR_TOKEN" \
     "https://yourdomain.atlassian.net/rest/api/3/myself"
```

Test Ollama connection:
```bash
curl -X POST http://localhost:11434/api/generate \
     -d '{"model":"llama3","prompt":"test","stream":false}'
```

## Optional Enhancements

- **Scheduling**: Use cron jobs for automated runs
- **Email Reports**: Add SMTP integration
- **Multiple Projects**: Support configuration files
- **Web Interface**: Build Flask/FastAPI frontend
- **Cloud Storage**: Upload reports to S3/Google Drive

## Security Notes

- Keep your `.env` file secure and never commit it to version control
- Use environment-specific tokens with minimal required permissions
- Ensure Ollama is not exposed to public networks

## License

This project is for internal use. Please ensure compliance with your organization's security policies.
