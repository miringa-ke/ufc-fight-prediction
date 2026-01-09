# UFC Fight Prediction API Documentation

Complete API reference for the UFC Fight Prediction System.

## 🌐 Base URL

```
https://your-ngrok-url.ngrok-free.app
```

**Note**: The ngrok URL changes each time you restart the Colab notebook. Update accordingly.

---

## 📡 Endpoints

### 1. Health Check

**Endpoint**: `GET /health`

**Description**: Check if the API is running and get model information.

**Request**: No parameters required

**Response**:
```json
{
  "status": "healthy",
  "model": "xgboost",
  "test_accuracy": "73.80%",
  "total_fighters": 2139
}
```

**cURL Example**:
```bash
curl https://your-ngrok-url.ngrok-free.app/health
```

---

### 2. Root / API Info

**Endpoint**: `GET /`

**Description**: Get API information and available endpoints.

**Response**:
```json
{
  "message": "UFC Fight Prediction API",
  "endpoints": [
    "GET /",
    "GET /health",
    "POST /predict",
    "POST /batch-predict"
  ]
}
```

---

### 3. Single Fight Prediction

**Endpoint**: `POST /predict`

**Description**: Predict the outcome of a single UFC fight.

#### Request Body

```json
{
  "red_fighter": "Jon Jones",
  "blue_fighter": "Tom Aspinall",
  "weight_class": "Heavyweight",
  "title_bout": true
}
```

**Fields**:
- `red_fighter` (string, required): Name of the red corner fighter
- `blue_fighter` (string, required): Name of the blue corner fighter
- `weight_class` (string, required): Weight class of the fight
- `title_bout` (boolean, optional): Whether it's a title fight (default: false)

**Valid Weight Classes**:
- `Heavyweight`
- `LightHeavyweight`
- `Middleweight`
- `Welterweight`
- `Lightweight`
- `Featherweight`
- `Bantamweight`
- `Flyweight`

#### Success Response

**Status Code**: `200 OK`

```json
{
  "success": true,
  "prediction": {
    "red_fighter": "Jon Jones",
    "blue_fighter": "Tom Aspinall",
    "weight_class": "Heavyweight",
    "title_bout": true,
    "predicted_winner": "Jon Jones",
    "red_win_probability": 87.75,
    "blue_win_probability": 12.25,
    "confidence": 75.5,
    "prediction_timestamp": "2026-01-06 10:30:45"
  },
  "fighter_stats": {
    "red_fighter": {
      "elo": 1842,
      "record": "27-1",
      "height_cm": 193.0,
      "reach_cm": 215.0,
      "win_streak": 3
    },
    "blue_fighter": {
      "elo": 1698,
      "record": "14-1",
      "height_cm": 196.0,
      "reach_cm": 203.0,
      "win_streak": 5
    }
  }
}
```

#### Error Response

**Status Code**: `400 Bad Request` (Missing fields)

```json
{
  "success": false,
  "error": "Missing required fields: red_fighter, blue_fighter, weight_class"
}
```

**Status Code**: `500 Internal Server Error` (Fighter not found)

```json
{
  "success": false,
  "error": "Fighter 'Unknown Fighter' not found"
}
```

#### cURL Example

```bash
curl -X POST https://your-ngrok-url.ngrok-free.app/predict \
  -H "Content-Type: application/json" \
  -d '{
    "red_fighter": "Jon Jones",
    "blue_fighter": "Tom Aspinall",
    "weight_class": "Heavyweight",
    "title_bout": true
  }'
```

#### Python Example

```python
import requests
import json

url = "https://your-ngrok-url.ngrok-free.app/predict"

payload = {
    "red_fighter": "Israel Adesanya",
    "blue_fighter": "Alex Pereira",
    "weight_class": "Middleweight",
    "title_bout": True
}

response = requests.post(url, json=payload)
result = response.json()

if result['success']:
    print(f"Predicted Winner: {result['prediction']['predicted_winner']}")
    print(f"Confidence: {result['prediction']['confidence']}%")
else:
    print(f"Error: {result['error']}")
```

#### JavaScript Example

```javascript
const url = 'https://your-ngrok-url.ngrok-free.app/predict';

const payload = {
  red_fighter: 'Islam Makhachev',
  blue_fighter: 'Charles Oliveira',
  weight_class: 'Lightweight',
  title_bout: true
};

fetch(url, {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify(payload)
})
  .then(response => response.json())
  .then(data => {
    if (data.success) {
      console.log('Winner:', data.prediction.predicted_winner);
      console.log('Confidence:', data.prediction.confidence + '%');
    } else {
      console.error('Error:', data.error);
    }
  });
```

---

### 4. Batch Predictions

**Endpoint**: `POST /batch-predict`

**Description**: Predict outcomes for multiple fights at once.

#### Request Body

```json
{
  "fights": [
    {
      "red_fighter": "Jon Jones",
      "blue_fighter": "Tom Aspinall",
      "weight_class": "Heavyweight"
    },
    {
      "red_fighter": "Islam Makhachev",
      "blue_fighter": "Charles Oliveira",
      "weight_class": "Lightweight",
      "title_bout": true
    },
    {
      "red_fighter": "Israel Adesanya",
      "blue_fighter": "Alex Pereira",
      "weight_class": "Middleweight"
    }
  ]
}
```

#### Success Response

```json
{
  "success": true,
  "total": 3,
  "predictions": [
    {
      "success": true,
      "prediction": {
        "predicted_winner": "Jon Jones",
        "confidence": 75.5,
        ...
      },
      ...
    },
    {
      "success": true,
      "prediction": {
        "predicted_winner": "Islam Makhachev",
        "confidence": 68.2,
        ...
      },
      ...
    },
    {
      "success": false,
      "error": "Fighter 'Unknown Fighter' not found"
    }
  ]
}
```

#### cURL Example

```bash
curl -X POST https://your-ngrok-url.ngrok-free.app/batch-predict \
  -H "Content-Type: application/json" \
  -d '{
    "fights": [
      {
        "red_fighter": "Fighter A",
        "blue_fighter": "Fighter B",
        "weight_class": "Welterweight"
      },
      {
        "red_fighter": "Fighter C",
        "blue_fighter": "Fighter D",
        "weight_class": "Lightweight"
      }
    ]
  }'
```

---

## 🔐 Authentication

Currently, the API **does not require authentication**. 

For production deployment, consider adding:
- API key authentication
- Rate limiting
- OAuth 2.0

---

## ⚡ Rate Limits

**Free ngrok tier limits**:
- 40 requests per minute
- 1 tunnel at a time

For higher limits, upgrade to ngrok paid plan or deploy to a dedicated server.

---

## 🎯 Response Fields Explained

### Prediction Object

| Field | Type | Description |
|-------|------|-------------|
| `predicted_winner` | string | Name of the fighter predicted to win |
| `red_win_probability` | float | Probability (0-100) that red fighter wins |
| `blue_win_probability` | float | Probability (0-100) that blue fighter wins |
| `confidence` | float | Model confidence (0-100). Distance from 50% |
| `prediction_timestamp` | string | When the prediction was made (ISO format) |

### Fighter Stats Object

| Field | Type | Description |
|-------|------|-------------|
| `elo` | int | Current ELO rating (1200-2000 typical range) |
| `record` | string | Win-Loss record (e.g., "27-1") |
| `height_cm` | float | Height in centimeters |
| `reach_cm` | float | Reach in centimeters |
| `win_streak` | int | Current winning streak |

### Confidence Levels

| Confidence | Interpretation | Expected Accuracy |
|------------|----------------|-------------------|
| 80-100% | Very High | ~85-90% |
| 60-80% | High | ~75-80% |
| 40-60% | Medium | ~65-70% |
| 20-40% | Low | ~60-65% |
| 0-20% | Very Low | ~55-60% |

---

## 🐛 Error Codes

| Status Code | Description | Solution |
|-------------|-------------|----------|
| 400 | Bad Request | Check that all required fields are present |
| 404 | Not Found | Endpoint doesn't exist - check URL |
| 500 | Internal Server Error | Fighter not found or server issue |
| 502 | Bad Gateway | ngrok tunnel is down - restart Colab |
| 503 | Service Unavailable | API is overloaded or Colab disconnected |

---

## 📊 Usage Examples

### Example 1: Get Prediction with Confidence Threshold

```python
def get_high_confidence_prediction(red, blue, weight_class, min_confidence=70):
    """Only return predictions above confidence threshold"""
    
    response = requests.post(
        "https://your-ngrok-url.ngrok-free.app/predict",
        json={
            "red_fighter": red,
            "blue_fighter": blue,
            "weight_class": weight_class
        }
    )
    
    result = response.json()
    
    if result['success']:
        confidence = result['prediction']['confidence']
        if confidence >= min_confidence:
            return result
        else:
            return {"message": f"Low confidence ({confidence}%), prediction not reliable"}
    
    return result

# Usage
prediction = get_high_confidence_prediction("Jon Jones", "Tom Aspinall", "Heavyweight")
print(prediction)
```

### Example 2: Analyze Fight Card

```python
def analyze_fight_card(fights):
    """Predict all fights on a card"""
    
    response = requests.post(
        "https://your-ngrok-url.ngrok-free.app/batch-predict",
        json={"fights": fights}
    )
    
    results = response.json()
    
    print(f"Fight Card Analysis ({results['total']} fights):\n")
    
    for i, pred in enumerate(results['predictions'], 1):
        if pred['success']:
            p = pred['prediction']
            print(f"{i}. {p['red_fighter']} vs {p['blue_fighter']}")
            print(f"   Winner: {p['predicted_winner']} ({p['confidence']:.1f}% confidence)\n")
        else:
            print(f"{i}. Error: {pred['error']}\n")

# Usage
ufc_300 = [
    {"red_fighter": "Fighter A", "blue_fighter": "Fighter B", "weight_class": "Heavyweight"},
    {"red_fighter": "Fighter C", "blue_fighter": "Fighter D", "weight_class": "Lightweight"},
]

analyze_fight_card(ufc_300)
```

---

## 🔄 Keeping API Running

### Colab Free Tier
- **Runtime**: 12 hours maximum
- **Auto-disconnect**: After ~90 minutes of inactivity
- **Solution**: Keep browser tab open, or use Colab Pro

### Colab Pro
- **Runtime**: 24 hours
- **Better GPUs**: Faster model loading
- **Cost**: $10/month

### Production Deployment
For 24/7 availability, deploy to:
- Railway.app (free tier)
- Render.com (free tier)
- AWS Lambda + API Gateway
- Google Cloud Run

---

## 📞 Support

**Issues**: [GitHub Issues](https://github.com/miringa-ke/ufc-fight-prediction/issues)  
**Email**: Available on GitHub profile  
**Response Time**: Usually within 24-48 hours

---

**API Version**: 1.0  
**Last Updated**: January 2026
