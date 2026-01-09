# n8n Workflow Integration

This directory contains n8n workflow configurations for automating UFC fight predictions.

## 📁 Files

- `ufc_prediction_workflow.json` - Main prediction workflow

## 🔧 Setup Instructions

### Prerequisites

1. **n8n Cloud account** (or self-hosted n8n)
2. **Running Flask API** (from Phase 4)
3. **ngrok URL** (public URL to your API)

### Step 1: Import Workflow

1. Open your n8n instance
2. Click **"Workflows"** → **"Add Workflow"**
3. Click the **menu (...)** → **"Import from File"**
4. Select `ufc_prediction_workflow.json`
5. Workflow will appear with 4 nodes

### Step 2: Configure ngrok URL

1. Click on **"Call Colab Prediction API"** node
2. Find the **URL** field
3. Replace `YOUR_NGROK_URL_HERE` with your actual ngrok URL
4. Add `/predict` to the end
5. Example: `https://abc123.ngrok-free.app/predict`
6. Click **"Save"**

### Step 3: Activate Workflow

1. Toggle the **"Active"** switch in top right
2. The workflow is now live!

### Step 4: Get Webhook URL

1. Click on the **"Webhook"** node
2. Copy the **"Production URL"**
3. It will look like: `https://your-instance.app.n8n.cloud/webhook/ufc-prediction`

## 🧪 Testing the Workflow

### Using cURL

```bash
curl -X POST https://your-instance.app.n8n.cloud/webhook/ufc-prediction \
  -H "Content-Type: application/json" \
  -d '{
    "red_fighter": "Jon Jones",
    "blue_fighter": "Tom Aspinall",
    "weight_class": "Heavyweight",
    "title_bout": true
  }'
```

### Using Python

```python
import requests

url = "https://your-instance.app.n8n.cloud/webhook/ufc-prediction"

payload = {
    "red_fighter": "Islam Makhachev",
    "blue_fighter": "Charles Oliveira",
    "weight_class": "Lightweight",
    "title_bout": True
}

response = requests.post(url, json=payload)
print(response.json())
```

### Using Postman

1. Create new POST request
2. URL: Your n8n webhook URL
3. Headers: `Content-Type: application/json`
4. Body: Raw JSON with fight details
5. Send!

## 📊 Workflow Structure

```
┌─────────────┐
│   Webhook   │  ← Receives fight prediction requests
└──────┬──────┘
       │
       ▼
┌─────────────┐
│ Validate &  │  ← Checks required fields, formats data
│   Format    │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│ Call Colab  │  ← Sends request to your Flask API
│     API     │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│   Format    │  ← Formats response nicely
│  Response   │
└─────────────┘
```

## 🔄 Workflow Details

### Node 1: Webhook

**Type**: Webhook Trigger  
**Method**: POST  
**Path**: `ufc-prediction`

**Expected Input**:
```json
{
  "red_fighter": "string",
  "blue_fighter": "string",
  "weight_class": "string",
  "title_bout": boolean (optional)
}
```

### Node 2: Validate & Format

**Type**: Code (JavaScript)

**Purpose**: 
- Validates required fields
- Formats data for API
- Sets defaults (title_bout = false)

**Code**:
```javascript
const input = $input.first().json.body;

if (!input.red_fighter || !input.blue_fighter || !input.weight_class) {
  throw new Error('Missing required fields');
}

return {
  json: {
    red_fighter: input.red_fighter,
    blue_fighter: input.blue_fighter,
    weight_class: input.weight_class,
    title_bout: input.title_bout || false
  }
};
```

### Node 3: Call Colab API

**Type**: HTTP Request  
**Method**: POST  
**URL**: `YOUR_NGROK_URL/predict`

**Body**: 
```
{{ $json }}
```

### Node 4: Format Response

**Type**: Code (JavaScript)

**Purpose**:
- Formats API response
- Makes it user-friendly
- Adds summary info

**Output Example**:
```json
{
  "success": true,
  "fight": {
    "matchup": "Jon Jones vs Tom Aspinall",
    "weight_class": "Heavyweight"
  },
  "prediction": {
    "winner": "Jon Jones",
    "confidence": "75.5%"
  },
  "probabilities": {
    "red_fighter": {
      "name": "Jon Jones",
      "win_probability": "87.75%"
    },
    "blue_fighter": {
      "name": "Tom Aspinall",
      "win_probability": "12.25%"
    }
  }
}
```

## 🚀 Advanced Usage

### Adding Email Notifications

1. Add **"Send Email"** node after "Format Response"
2. Configure email settings
3. Send prediction results to yourself

### Logging Predictions

1. Add **"Google Sheets"** node
2. Log each prediction to a spreadsheet
3. Track accuracy over time

### Batch Processing

1. Add **"Split in Batches"** node
2. Process multiple fights from a list
3. Aggregate results

## 🐛 Troubleshooting

### Workflow not triggering
- Check if workflow is **Active** (toggle in top right)
- Verify webhook URL is correct
- Test with cURL to isolate issue

### "Missing required fields" error
- Ensure JSON body has `red_fighter`, `blue_fighter`, `weight_class`
- Check spelling and capitalization
- Verify Content-Type header is `application/json`

### API timeout
- Check if Colab notebook is still running
- Verify ngrok URL hasn't changed
- Increase timeout in HTTP Request node settings (30-60 seconds)

### "Fighter not found" error
- Fighter name must match exactly with database
- Check spelling and capitalization
- Use exact names from your dataset

## 🔐 Security (Optional)

### Add Authentication

1. Click on Webhook node
2. Enable **Authentication**
3. Choose method (Header Auth, Basic Auth, etc.)
4. Set credentials
5. Update your requests to include auth

### Example with Header Auth:
```bash
curl -X POST https://your-webhook-url \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_SECRET_TOKEN" \
  -d '{"red_fighter": "...", ...}'
```

## 📝 Customization Ideas

1. **Add slack notifications** - Send predictions to Slack channel
2. **Database logging** - Store predictions in database
3. **Conditional routing** - Different actions based on confidence
4. **Schedule predictions** - Auto-predict upcoming fights
5. **Web scraping** - Fetch upcoming fight cards automatically

## 🆘 Need Help?

**n8n Documentation**: [docs.n8n.io](https://docs.n8n.io)  
**n8n Community**: [community.n8n.io](https://community.n8n.io)  
**Project Issues**: [GitHub Issues](https://github.com/miringa-ke/ufc-fight-prediction/issues)

---

**Last Updated**: January 2026
