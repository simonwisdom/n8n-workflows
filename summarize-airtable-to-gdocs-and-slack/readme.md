# Automated Fieldwork Summary Workflow

An n8n workflow that automatically exports recent fieldwork items from Airtable to Google Docs and posts notifications to Slack on a weekly schedule.

## 📋 Overview

This workflow automates the process of:
1. **Collecting** recent fieldwork items from an Airtable database
2. **Formatting** them into a structured document
3. **Creating** a Google Doc with the formatted content
4. **Notifying** your team via Slack with a link to the document

Perfect for teams that track fieldwork, research, or any time-sensitive data collection activities in Airtable.

## 🏗️ Workflow Components

### Nodes Included:
- **Schedule Trigger**: Runs every 7 days at 6 PM
- **Airtable Search**: Fetches items added in the last 7 days
- **JavaScript Code**: Formats data into a readable document structure
- **Google Docs Create**: Creates a new document
- **Google Docs Update**: Adds the formatted content
- **Data Processing**: Prepares notification data
- **Slack Notification**: Posts summary message with document link

## 🔧 Prerequisites

### Required Services:
- **n8n** (self-hosted or cloud)
- **Airtable** account with API access
- **Google Workspace** account with Docs access
- **Slack** workspace with bot permissions

### Required Airtable Structure:
Your Airtable table should have these fields:
- `date_added` - Date field for when items were added
- `summary` - Text field with item description
- `url` - URL field (optional)
- `tags` - Multiple select or text field (optional)

## 🚀 Setup Instructions

### 1. Import the Workflow
1. Download the `workflow.json` file
2. In n8n, go to **Workflows** → **Import from File**
3. Select the downloaded JSON file

### 2. Configure Credentials
Set up authentication for each service:

#### Airtable:
1. Go to **Credentials** → **Create New**
2. Select **Airtable Personal Access Token**
3. Generate a token in your [Airtable account settings](https://airtable.com/create/tokens)
4. Add token to n8n credentials

#### Google Docs:
1. Create **Google Docs OAuth2** credentials
2. Follow Google's OAuth setup process
3. Grant necessary permissions for document creation/editing

#### Slack:
1. Create a Slack app in your workspace
2. Add OAuth scopes: `chat:write`, `channels:read`
3. Install app to workspace and get OAuth token
4. Add token to n8n Slack OAuth2 credentials

### 3. Update Configuration

Replace the placeholder values in the workflow:

#### Airtable Node:
```json
"base": "YOUR_AIRTABLE_BASE_ID"
"table": "YOUR_AIRTABLE_TABLE_ID"
```

#### Google Docs Node:
```json
"folderId": "YOUR_GOOGLE_DRIVE_FOLDER_ID"
```

#### Slack Node:
```json
"channelId": "YOUR_SLACK_CHANNEL_ID"
```

### 4. Assign Credentials
1. Open each node that requires authentication
2. Select the appropriate credential from the dropdown
3. Test the connection

## 🎯 Finding Your IDs

### Airtable Base & Table IDs:
- Base ID: Found in your Airtable URL: `https://airtable.com/[BASE_ID]/[TABLE_ID]`
- Table ID: Also in the URL after the base ID

### Google Drive Folder ID:
- Open the target folder in Google Drive
- Copy the ID from the URL: `https://drive.google.com/drive/folders/[FOLDER_ID]`

### Slack Channel ID:
- Right-click on the channel name in Slack
- Select "Copy link"
- Extract ID from URL or use Slack's API

## 📅 Customization Options

### Change Schedule:
Modify the **Schedule Trigger** node to run at different intervals:
```json
"daysInterval": 7,  // Change to desired number of days
"triggerAtHour": 18 // Change to desired hour (24-hour format)
```

### Modify Time Range:
Update the Airtable filter formula to change the lookback period:
```javascript
"filterByFormula": "=IS_AFTER({date_added}, DATEADD(TODAY(), -7, 'days'))"
// Change -7 to desired number of days
```

### Customize Document Format:
Edit the JavaScript code in the **Format Document** node to:
- Change the document structure
- Add/remove fields
- Modify formatting and styling

### Update Slack Message:
Modify the Slack notification text in the **Post Notification to Slack** node to match your team's communication style.

## 📊 Expected Output

### Google Doc Format:
```
FIELDWORK EXPORT (X items)

1. [Summary of item 1]
URL: [URL if provided]
TAGS: [Tags if provided]

2. [Summary of item 2]
URL: [URL if provided]
TAGS: [Tags if provided]
```

### Slack Notification:
- 📄 Weekly Fieldwork Export Ready!
- 🗓️ Export Date: [Date]
- 📊 Items Exported: [Count]
- 📋 Time Period: Last 7 days
- 🔗 View Document: [Google Doc Link]

## 🔍 Troubleshooting

### Common Issues:

**Airtable Connection Failed:**
- Verify your API token has correct permissions
- Check that base and table IDs are correct
- Ensure date field name matches `date_added`

**Google Docs Permission Error:**
- Confirm OAuth credentials have document creation permissions
- Verify the target folder exists and is accessible
- Check that the service account has proper sharing permissions

**Slack Message Not Posting:**
- Verify bot has `chat:write` permission in the target channel
- Ensure channel ID is correct
- Check that the bot is added to the channel

**No Items Found:**
- Verify the date filter formula matches your Airtable date field format
- Check that items exist within the specified time range
- Confirm field names match exactly (case-sensitive)

## 🔄 Testing

1. **Manual Trigger**: Use n8n's manual execution to test the workflow
2. **Check Logs**: Review execution logs for any errors
3. **Verify Output**: Confirm the Google Doc is created and Slack message is posted
4. **Data Validation**: Ensure all expected items appear in the export

## 📈 Extensions

### Possible Enhancements:
- Add email notifications
- Include charts or analytics
- Filter by specific tags or categories
- Archive old documents automatically
- Add approval workflow before publishing
- Integration with project management tools

## 📝 License

This workflow template is provided as-is for educational and commercial use. Feel free to modify and adapt for your specific needs.

## 🤝 Contributing

Found a bug or have an improvement? Feel free to:
1. Fork this repository
2. Make your changes
3. Submit a pull request

## 📞 Support

For questions about:
- **n8n setup**: Check the [n8n documentation](https://docs.n8n.io/)
- **Airtable API**: Visit [Airtable's API documentation](https://airtable.com/developers/web/api/introduction)
- **Google Docs API**: See [Google's API documentation](https://developers.google.com/docs/api)
- **Slack API**: Reference [Slack's API documentation](https://api.slack.com/)
