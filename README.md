# UFC Fight Prediction System 🥊

An end-to-end machine learning system that predicts UFC fight outcomes using ELO ratings, fighter statistics, and ensemble learning. Built with Python, XGBoost, and deployed via Flask API with n8n workflow automation.

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Machine Learning](https://img.shields.io/badge/ML-XGBoost-orange)](https://xgboost.readthedocs.io/)

## 🎯 Overview

This project implements a comprehensive UFC fight prediction system that:
- Tracks fighter skill using **ELO rating system** (similar to chess)
- Engineers **140+ features** from historical fight data
- Trains **3 ML models** (Decision Tree, Random Forest, XGBoost)
- Achieves **~70-75% prediction accuracy**
- Provides **REST API** for real-time predictions
- Integrates with **n8n** for workflow automation

## 📊 Model Performance

| Model | Test Accuracy | AUC-ROC | Key Features |
|-------|---------------|---------|--------------|
| Decision Tree | ~65% | 0.68 | Fast, interpretable baseline |
| Random Forest | ~70% | 0.74 | Ensemble of 200 trees |
| **XGBoost** | **~73%** | **0.78** | **Best performer** |

**Prediction Confidence Analysis:**
- High confidence (>70%): **~80-85% accuracy**
- Medium confidence (50-70%): **~65-70% accuracy**
- Low confidence (<50%): **~55-60% accuracy**

## 🚀 Quick Start

### Prerequisites
- Python 3.8 or higher
- Google Colab account (or local Jupyter)
- 4 UFC datasets (see [Data section](#-data))
- ngrok account (for API deployment)
- n8n Cloud account (optional, for automation)

### Installation

1. **Clone the repository:**
```bash
git clone https://github.com/miringa-ke/ufc-fight-prediction.git
cd ufc-fight-prediction
```

2. **Install dependencies:**
```bash
pip install -r requirements.txt
```

3. **Prepare your data:**
   - Download or prepare your UFC fight datasets
   - Place CSV files in `data/raw/`
   - See `data/raw/README.md` for details

4. **Run the notebooks:**

Open Google Colab and upload the notebooks in order:

```
Phase 1: Data Preparation & ELO Rating System
↓
Phase 2: Feature Engineering (140+ features)
↓
Phase 3: Model Training (Decision Tree → Random Forest → XGBoost)
↓
Phase 4: API Deployment & n8n Integration
```

### Making Your First Prediction

Once the API is running:

```bash
curl -X POST https://your-ngrok-url/predict \
  -H "Content-Type: application/json" \
  -d '{
    "red_fighter": "Jon Jones",
    "blue_fighter": "Tom Aspinall",
    "weight_class": "Heavyweight",
    "title_bout": true
  }'
```

**Response:**
```json
{
  "success": true,
  "prediction": {
    "predicted_winner": "Jon Jones",
    "confidence": "75.5%",
    "red_win_probability": "87.75%",
    "blue_win_probability": "12.25%"
  },
  "fighter_stats": {
    "red_fighter": {
      "elo": 1842,
      "record": "27-1",
      "height_cm": 193.0,
      "reach_cm": 215.0
    },
    "blue_fighter": {
      "elo": 1698,
      "record": "14-1",
      "height_cm": 196.0,
      "reach_cm": 203.0
    }
  }
}
```

## 🏗️ Project Structure

```
ufc-fight-prediction/
├── README.md                          # This file
├── LICENSE                            # MIT License
├── requirements.txt                   # Python dependencies
├── .gitignore                        # Git ignore rules
│
├── notebooks/                        # Jupyter/Colab notebooks
│   ├── Phase1_Data_Preparation.ipynb
│   ├── Phase2_Feature_Engineering.ipynb
│   ├── Phase3_Model_Training.ipynb
│   └── Phase4_Prediction_API.ipynb
│
├── data/                             # Data directory (not in git)
│   ├── raw/                          # Raw CSV files
│   │   ├── .gitkeep
│   │   └── README.md                 # Data requirements
│   ├── processed/                    # Processed datasets
│   └── predictions/                  # Prediction history
│
├── models/                           # Trained models (not in git)
│   └── .gitkeep
│
├── visualizations/                   # Generated charts
│   └── .gitkeep
│
├── n8n/                             # n8n workflows
│   ├── ufc_prediction_workflow.json
│   └── README.md
│
└── docs/                            # Additional documentation
    ├── SETUP_GUIDE.md
    ├── API_DOCUMENTATION.md
    └── METHODOLOGY.md
```

## 📖 Documentation

- **[Setup Guide](docs/SETUP_GUIDE.md)** - Detailed installation and configuration
- **[API Documentation](docs/API_DOCUMENTATION.md)** - API endpoints and usage examples
- **[Methodology](docs/METHODOLOGY.md)** - How the prediction system works
- **[n8n Integration](n8n/README.md)** - Workflow automation setup

## 🔬 Methodology

### 1. ELO Rating System
Fighters start at 1500 ELO. Ratings update after each fight based on:
- Expected outcome (based on ELO difference)
- Actual outcome (win/loss)
- K-factor (32) - determines rating volatility

### 2. Feature Engineering
**140+ features** across 5 categories:

- **ELO Features** (4): Overall ELO, weight-class ELO, differences, averages
- **Physical Attributes** (8): Height, reach, age differences, ratios
- **Performance Metrics** (60+): Striking stats, takedown stats, control time
- **Experience Indicators** (20+): Total fights, win rate, streaks, title bouts
- **Efficiency Metrics** (40+): Strike accuracy, defense ratings, output

### 3. Model Training
- **Train/Test Split**: 80/20 time-based (older fights for training)
- **Models**: Decision Tree → Random Forest → XGBoost
- **Evaluation**: Accuracy, AUC-ROC, confusion matrix, calibration

### 4. Deployment
- **Flask API** with endpoints for single/batch predictions
- **ngrok** for public URL tunneling
- **n8n** for workflow automation

## 🎓 Key Learnings

This project demonstrates:
- ✅ Building ELO rating systems for sports analytics
- ✅ Feature engineering for time-series fight data
- ✅ Training and comparing ensemble learning models
- ✅ Deploying ML models as REST APIs
- ✅ Workflow automation with n8n
- ✅ Data visualization and model interpretation
- ✅ Handling imbalanced classification problems

## 📊 Sample Visualizations

The project generates comprehensive visualizations:

- **ELO Progression** - Track top fighters over time
- **Feature Importance** - What matters most in predictions
- **Model Comparison** - Decision Tree vs Random Forest vs XGBoost
- **Confusion Matrices** - Where models succeed and fail
- **Prediction Confidence** - Relationship between confidence and accuracy
- **ROC Curves** - Model discrimination ability

## 🔮 Future Enhancements

Planned improvements:

- [ ] **Neural Network Models** - Implement LSTM/GRU for sequential fight data
- [ ] **Fight Outcome Prediction** - Predict method of victory (KO, submission, decision)
- [ ] **Round Prediction** - Estimate when the fight will end
- [ ] **Betting Odds Integration** - Compare model predictions to market odds
- [ ] **Real-time Data Scraping** - Automated updates from UFC Stats
- [ ] **Web Interface** - User-friendly frontend for predictions
- [ ] **Mobile App** - iOS/Android app for on-the-go predictions
- [ ] **Multi-fight Analysis** - Predict entire fight cards

## 📝 Data

### Required Datasets

You need 4 CSV files with UFC fight data:

1. **data.csv** - Aggregated fight data with pre-calculated averages
2. **preprocessed_data.csv** - Feature-engineered dataset
3. **raw_fighter_details.csv** - Fighter physical attributes
4. **raw_total_fight_data.csv** - Individual fight records

**Note:** Due to size constraints, data files are not included in this repository. See `data/raw/README.md` for detailed requirements.

### Data Sources

- Official UFC Statistics: [ufcstats.com](http://ufcstats.com)
- Community datasets on Kaggle
- Custom web scraping (advanced)

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. **Fork the repository**
2. **Create a feature branch** (`git checkout -b feature/amazing-feature`)
3. **Commit your changes** (`git commit -m 'Add amazing feature'`)
4. **Push to the branch** (`git push origin feature/amazing-feature`)
5. **Open a Pull Request**

### Ideas for Contributions
- Improve model accuracy
- Add new features (fighter age, camp data, injury history)
- Create web interface
- Optimize API performance
- Add more visualizations
- Write tests

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

**TL;DR:** Free to use for learning and non-commercial purposes. Attribution appreciated.

## ⚠️ Disclaimer

This project is for **educational and research purposes only**. Predictions are probabilistic and should not be used for:
- Gambling or betting decisions
- Financial investments
- Professional fight analysis without expert verification

The model's accuracy (~70-75%) means it will be wrong about 1 in 4 predictions.

## 🙏 Acknowledgments

- **Inspiration**: Tennis prediction methodologies using ELO systems
- **Data**: UFC official statistics and community datasets
- **Tools**: Google Colab, scikit-learn, XGBoost, Flask, n8n
- **Community**: Stack Overflow, Kaggle, GitHub

## 📞 Contact

**Miringa** - [@miringa-ke](https://github.com/miringa-ke)

**Project Link**: [https://github.com/miringa-ke/ufc-fight-prediction](https://github.com/miringa-ke/ufc-fight-prediction)

## ⭐ Show Your Support

If you find this project helpful, please consider:
- Giving it a ⭐ star on GitHub
- Sharing it with others interested in ML/sports analytics
- Contributing improvements or ideas

---

**Built with ❤️ for sports analytics and machine learning**
