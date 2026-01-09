# Setup Guide

Complete step-by-step guide to set up the UFC Fight Prediction System.

## 📋 Prerequisites

Before you begin, ensure you have:

- ✅ Python 3.8 or higher
- ✅ Google account (for Colab and Drive)
- ✅ UFC fight datasets (4 CSV files)
- ✅ ngrok account (free tier is fine)
- ✅ n8n Cloud account (optional, for automation)
- ✅ Basic understanding of Python and Jupyter notebooks

**Estimated Setup Time**: 2-3 hours (first time)

---

## 🚀 Quick Start (30 Minutes)

### Step 1: Get the Code

```bash
git clone https://github.com/miringa-ke/ufc-fight-prediction.git
cd ufc-fight-prediction
```

### Step 2: Prepare Data

1. Download or prepare your 4 UFC datasets
2. Place them in `data/raw/` folder
3. Verify files are named correctly:
   - `data.csv`
   - `preprocessed_data.csv`
   - `raw_fighter_details.csv`
   - `raw_total_fight_data.csv`

### Step 3: Open in Google Colab

1. Go to [Google Colab](https://colab.research.google.com)
2. Upload `notebooks/Phase1_Data_Preparation.ipynb`
3. Mount Google Drive
4. Run all cells

### Step 4: Continue Through Phases

Run notebooks in order:
- Phase 1: Data Preparation (30 min)
- Phase 2: Feature Engineering (30 min)
- Phase 3: Model Training (45 min)
- Phase 4: API Deployment (15 min)

---

## 📖 Detailed Setup

### Part 1: Environment Setup

#### Option A: Google Colab (Recommended for Beginners)

**Pros**:
- No local installation needed
- Free GPU/TPU access
- Pre-installed ML libraries
- Easy Google Drive integration

**Cons**:
- 12-hour runtime limit
- Requires internet connection
- Sessions disconnect after inactivity

**Steps**:

1. **Create Google Drive folder structure**:
```
MyDrive/
└── UFC_ML_Project/
    ├── data/
    │   ├── raw/
    │   ├── processed/
    │   └── predictions/
    ├── models/
    ├── visualizations/
    └── notebooks/
```

2. **Upload notebooks**:
   - Download all 4 notebooks from this repo
   - Upload to `UFC_ML_Project/notebooks/` in Google Drive

3. **Mount Drive in Colab**:
```python
from google.colab import drive
drive.mount('/content/drive')
```

#### Option B: Local Jupyter (Advanced)

**Pros**:
- No runtime limits
- Works offline
- Faster for large datasets
- Full control

**Cons**:
- Need to install dependencies
- Requires good hardware
- More setup complexity

**Steps**:

1. **Install Python** (3.8+):
   - Download from [python.org](https://python.org)
   - Verify: `python --version`

2. **Create virtual environment**:
```bash
python -m venv ufc_env
source ufc_env/bin/activate  # On Windows: ufc_env\Scripts\activate
```

3. **Install dependencies**:
```bash
pip install -r requirements.txt
```

4. **Start Jupyter**:
```bash
jupyter notebook
```

---

### Part 2: Data Preparation

#### Step 2.1: Obtain Data

**Option A: Use Your Existing Data**
- If you already have the 4 CSV files, skip to Step 2.2

**Option B: Download from Kaggle**
1. Go to [Kaggle UFC datasets](https://www.kaggle.com/search?q=ufc+fights)
2. Download appropriate datasets
3. Ensure column names match requirements (see `data/raw/README.md`)

**Option C: Web Scraping** (Advanced)
1. Install scraping tools: `pip install beautifulsoup4 selenium`
2. Scrape from [ufcstats.com](http://ufcstats.com)
3. Format to match required schema

#### Step 2.2: Validate Data

Run this validation script:

```python
import pandas as pd

# Check data.csv
data = pd.read_csv('data/raw/data.csv')
print(f"✅ data.csv: {len(data)} rows")
assert 'R_fighter' in data.columns
assert 'B_fighter' in data.columns
assert 'Winner' in data.columns

# Check raw_total_fight_data.csv (semicolon delimiter!)
fight_data = pd.read_csv('data/raw/raw_total_fight_data.csv', sep=';')
print(f"✅ raw_total_fight_data.csv: {len(fight_data)} rows")

# Check fighter details
fighter_details = pd.read_csv('data/raw/raw_fighter_details.csv')
print(f"✅ raw_fighter_details.csv: {len(fighter_details)} rows")

# Check preprocessed
preprocessed = pd.read_csv('data/raw/preprocessed_data.csv')
print(f"✅ preprocessed_data.csv: {len(preprocessed)} rows")

print("\n🎉 All data files validated!")
```

---

### Part 3: Running the Notebooks

#### Phase 1: Data Preparation & ELO System (30-45 minutes)

**Purpose**: Clean data and calculate ELO ratings

**Key Steps**:
1. Load and merge datasets
2. Handle missing values
3. Implement ELO rating system
4. Calculate ELO for all historical fights
5. Visualize ELO progression

**Output Files**:
- `data/processed/cleaned_data.csv`
- `data/processed/elo_ratings.csv`
- `visualizations/elo_progression.png`

**Common Issues**:

| Issue | Solution |
|-------|----------|
| `FileNotFoundError` | Check that CSV files are in `data/raw/` |
| ELO all same value | Ensure data is sorted by date |
| Missing dates | Convert date column to datetime format |

#### Phase 2: Feature Engineering (30-45 minutes)

**Purpose**: Create 140+ predictive features

**Key Steps**:
1. Calculate physical differences (height, reach, age)
2. Calculate performance differences (striking, grappling)
3. Create efficiency metrics
4. Encode categorical variables
5. Analyze feature correlations

**Output Files**:
- `data/processed/feature_engineered_data.csv`
- `data/processed/feature_list.json`
- `visualizations/feature_correlations.png`

**Common Issues**:

| Issue | Solution |
|-------|----------|
| Too many NaN values | Fill with median/mean or 0 |
| Feature explosion | Reduce to top correlated features |
| Memory error | Use data types optimization |

#### Phase 3: Model Training (45-60 minutes)

**Purpose**: Train and compare ML models

**Key Steps**:
1. Split data (80/20 time-based)
2. Train Decision Tree
3. Train Random Forest (200 trees)
4. Train XGBoost (300 estimators)
5. Compare performance
6. Analyze feature importance
7. Save best model

**Output Files**:
- `models/decision_tree_model.pkl`
- `models/random_forest_model.pkl`
- `models/xgboost_model.pkl`
- `models/model_metadata.json`
- Multiple visualizations

**Expected Results**:
- Decision Tree: ~65% accuracy
- Random Forest: ~70% accuracy
- XGBoost: ~73% accuracy

**Common Issues**:

| Issue | Solution |
|-------|----------|
| Low accuracy (<60%) | Check ELO calculation, verify data quality |
| Overfitting (train >> test) | Increase regularization, reduce max_depth |
| Slow training | Reduce n_estimators or use subset of data |

#### Phase 4: API Deployment (15-30 minutes)

**Purpose**: Deploy Flask API with ngrok

**Key Steps**:
1. Load trained model
2. Create prediction functions
3. Set up Flask endpoints
4. Create ngrok tunnel
5. Test API
6. (Optional) Integrate with n8n

**Output**:
- Running Flask API
- Public ngrok URL
- API ready for predictions

**Common Issues**:

| Issue | Solution |
|-------|----------|
| ngrok auth error | Add auth token: `ngrok.set_auth_token("YOUR_TOKEN")` |
| Model not found | Ensure Phase 3 completed successfully |
| Port already in use | Kill existing Flask servers: `ngrok.kill()` |

---

### Part 4: ngrok Setup

#### Step 4.1: Create ngrok Account

1. Go to [ngrok.com](https://ngrok.com)
2. Sign up for free account
3. Get your auth token from dashboard

#### Step 4.2: Configure ngrok

In your Colab notebook (Phase 4):

```python
from pyngrok import ngrok

# Add your auth token
ngrok.set_auth_token("YOUR_AUTH_TOKEN_HERE")

# Create tunnel
tunnel = ngrok.connect(5000)
print(f"Public URL: {tunnel.public_url}")
```

**Important**: 
- Free tier allows 1 tunnel at a time
- URL changes when Colab restarts
- 40 requests/minute limit

---

### Part 5: n8n Integration (Optional)

#### Step 5.1: Create n8n Account

1. Go to [n8n.io](https://n8n.io)
2. Sign up for n8n Cloud (free tier available)
3. Create new workflow

#### Step 5.2: Import Workflow

1. Download `n8n/ufc_prediction_workflow.json`
2. In n8n, click **Import from File**
3. Select the JSON file
4. Update ngrok URL in HTTP Request node
5. Activate workflow

#### Step 5.3: Test Workflow

```bash
curl -X POST https://your-n8n-instance.app.n8n.cloud/webhook/ufc-prediction \
  -H "Content-Type: application/json" \
  -d '{
    "red_fighter": "Jon Jones",
    "blue_fighter": "Tom Aspinall",
    "weight_class": "Heavyweight"
  }'
```

---

## 🔄 Monthly Maintenance

### Adding New Fight Data

1. **Obtain new fight results** (from ufcstats.com or Kaggle)

2. **Update `raw_total_fight_data.csv`**:
   - Open file in Google Sheets or Excel
   - Append new rows at the bottom
   - Ensure same format/delimiter

3. **Re-run notebooks**:
   - Phase 1: Recalculate ELO with new data
   - Phase 2: Re-engineer features
   - Phase 3: Retrain models
   - Phase 4: Restart API

4. **Verify improvements**:
   - Check if accuracy improved
   - Compare feature importance
   - Test predictions on new fights

**Estimated Time**: 15-20 minutes

---

## 🐛 Troubleshooting

### Common Errors

#### Error: "No module named 'xgboost'"
**Solution**:
```bash
pip install xgboost
```

#### Error: "Runtime disconnected"
**Solution**:
- Keep Colab tab active
- Run cells periodically
- Upgrade to Colab Pro for longer runtime

#### Error: "Fighter not found"
**Solution**:
- Check fighter name spelling (must match exactly)
- Verify fighter exists in your dataset
- Use `data['R_fighter'].unique()` to see all fighters

#### Error: "Model file not found"
**Solution**:
- Ensure Phase 3 completed successfully
- Check `models/` folder has `.pkl` files
- Re-run Phase 3 if needed

### Getting Help

1. **Check documentation**: Read this guide thoroughly
2. **Search issues**: [GitHub Issues](https://github.com/miringa-ke/ufc-fight-prediction/issues)
3. **Ask for help**: Open new issue with error details
4. **Community**: Tag `@miringa-ke` on GitHub

---

## 📊 Performance Optimization

### For Faster Training

```python
# Use fewer estimators
xgb_model = xgb.XGBClassifier(n_estimators=100)  # Instead of 300

# Use subset of data for testing
data_subset = data.sample(frac=0.5, random_state=42)
```

### For Better Accuracy

1. **Add more features**: Fighter age, injury history, camp data
2. **Tune hyperparameters**: Use GridSearchCV
3. **Ensemble methods**: Combine multiple models
4. **More data**: Collect historical fights
5. **Better ELO**: Adjust K-factor, add recency weighting

### For Production Deployment

1. **Deploy to Railway/Render**: Permanent hosting
2. **Use Redis**: Cache predictions
3. **Add monitoring**: Track API usage
4. **Implement auth**: API key authentication
5. **Rate limiting**: Prevent abuse

---

## ✅ Setup Checklist

- [ ] Python 3.8+ installed
- [ ] Google account created
- [ ] UFC datasets obtained (4 CSV files)
- [ ] Data placed in `data/raw/`
- [ ] Data validated (no errors)
- [ ] Phase 1 completed (ELO calculated)
- [ ] Phase 2 completed (features engineered)
- [ ] Phase 3 completed (models trained)
- [ ] ngrok account created
- [ ] ngrok auth token configured
- [ ] Phase 4 completed (API running)
- [ ] API tested successfully
- [ ] (Optional) n8n workflow configured

---

## 🎓 Next Steps

After setup:
1. **Explore visualizations**: Understand model behavior
2. **Test predictions**: Try different fighter matchups
3. **Improve model**: Add features, tune parameters
4. **Deploy permanently**: Move from Colab to production
5. **Share results**: Blog post, YouTube video, etc.

---

## 📞 Support

**Documentation**: [Full docs](../README.md)  
**API Reference**: [API docs](API_DOCUMENTATION.md)  
**Issues**: [GitHub Issues](https://github.com/miringa-ke/ufc-fight-prediction/issues)  
**Contact**: [@miringa-ke](https://github.com/miringa-ke)

---

**Last Updated**: January 2026  
**Version**: 1.0
