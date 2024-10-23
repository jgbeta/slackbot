# Slack Bot To Read CSV Files in Channel (Python)

This guide explains how to set up a Slack bot in Python that can read messages from a Slack channel using the Slack API and `slack_sdk` library.

## Prerequisites
1. **Python 3.x** installed.
2. Install the Slack SDK:
   ```bash
   pip install slack_sdk
   ```
## Step 1: Create a Slack App
1. Go to Slack API Dashboard: Visit api.slack.com/apps and log in with your Slack account.
2. Create a new Slack App:
    - Click on the "Create New App" button.
    - Choose "From Scratch".
    - Name your app (e.g., ```ChannelBot```) and select the workspace where the bot will live.
3. Configure OAuth Permissions:
    - In your app’s settings, go to **OAuth & Permissions**.
    - Under Scopes, add the following:
       - Bot Token Scopes:
           - ```channels:history``` (to read messages in public channels)
           - ```channels:read``` (to view basic information about public channels)
           - ```files:read``` (to view files shared in channels and conversations)
           - ```files:write``` (to upload, edit, and delete files)
           - ```commands``` (to view basic information about public channels)
           - ```groups:history``` (to read messages in private channels, if needed)
           - ```groups:read``` (to view private channels your bot is invited to)
4. Install the App to Workspace:
    - Go to the **OAuth & Permissions** tab and scroll down to **OAuth Tokens & Redirect URLs**.
    - Click **Install to Workspace**.
    - After installation, you’ll get a Bot User OAuth Token (starts with ```xoxb-...```). Save this token.

## Step 2: Code the Slack Bot in Python
Below is a simple Python script that uses Slack’s Python SDK (```slack_sdk```) to read messages from a specific channel.

```python
import os
import requests
import pandas as pd
from slack_sdk import WebClient
from slack_sdk.errors import SlackApiError

# Replace with your own Bot User OAuth Token and Slack App Token
SLACK_BOT_TOKEN = "xoxb-your-bot-user-oauth-token"
SLACK_APP_TOKEN = "xapp-your-slack-user-app-token"
client = WebClient(token=SLACK_BOT_TOKEN)

channel_id = "C12345678"

# Fetch conversation history from the given channel
response = client.conversations_history(channel=channel_id, limit=100)

#get csv private link

csv_private = "https://files.slack.com/files-pri/enter-your-slack-link"

r=requests.get(csv_private, headers={"Authorization":f"Bearer {SLACK_BOT_TOKEN}"})
file_info = r.text

with open("temp_file.csv", "w") as text_file:
    text_file.write(file_info)
pd.read_csv("temp_file.csv")



