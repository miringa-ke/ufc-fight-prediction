# Notebooks

This directory contains the Jupyter/Colab notebooks for the UFC Fight Prediction System.

## 📚 Notebooks

1. **Phase1_Data_Preparation.ipynb** - Data cleaning and ELO rating calculation
2. **Phase2_Feature_Engineering.ipynb** - Creating 140+ predictive features
3. **Phase3_Model_Training.ipynb** - Training Decision Tree, Random Forest, and XGBoost
4. **Phase4_Prediction_API.ipynb** - Flask API deployment with ngrok

## 🚀 How to Use

### In Google Colab (Recommended)

1. Upload each notebook to Google Colab
2. Mount Google Drive
3. Run cells in order (top to bottom)
4. Save outputs to Drive

### Locally

1. Install Jupyter: `pip install jupyter`
2. Navigate to this directory: `cd notebooks/`
3. Start Jupyter: `jupyter notebook`
4. Open notebooks in browser
5. Run cells in order

## 📖 Notebook Details

### Phase 1: Data Preparation & ELO (30-45 min)

**Input**: 4 CSV files from `data/raw/`  
**Output**: 
- `data/processed/cleaned_data.csv`
- `data/processed/elo_ratings.csv`
- Visualizations in `visualizations/`

**Key Sections**:
- Data loading and exploration
- Missing value handling
- ELO system implementation
- ELO calculation for all fights
- ELO progression visualization

### Phase 2: Feature Engineering (30-45 min)

**Input**: `data/processed/elo_ratings.csv`  
**Output**:
- `data/processed/feature_engineered_data.csv`
- `data/processed/feature_list.json`
- Feature analysis visualizations

**Key Sections**:
- Physical attribute differences
- Performance metric differences
- Experience indicators
- Categorical encoding
- Feature correlation analysis

### Phase 3: Model Training (45-60 min)

**Input**: `data/processed/feature_engineered_data.csv`  
**Output**:
- `models/decision_tree_model.pkl`
- `models/random_forest_model.pkl`
- `models/xgboost_model.pkl`
- `models/model_metadata.json`
- Model comparison visualizations

**Key Sections**:
- Train/test split (time-based)
- Decision Tree training
- Random Forest training
- XGBoost training
- Model comparison
- Feature importance analysis

### Phase 4: API Deployment (15-30 min)

**Input**: Trained models from `models/`  
**Output**:
- Running Flask API
- ngrok public URL
- API ready for predictions

**Key Sections**:
- Model loading
- Prediction function creation
- Flask endpoint setup
- ngrok tunnel creation
- API testing

## 🔄 Execution Order

**IMPORTANT**: Notebooks must be run in order!

```
Phase 1 → Phase 2 → Phase 3 → Phase 4
```

Each phase depends on outputs from the previous phase.

## 💡 Tips

- **Save your work**: Download notebooks after major changes
- **Restart runtime**: If you get errors, try Runtime → Restart runtime
- **Check paths**: Verify Google Drive paths are correct
- **Monitor resources**: Colab has RAM/disk limits
- **Use GPU**: Runtime → Change runtime type → GPU (for faster training)

## 🐛 Common Issues

| Issue | Solution |
|-------|----------|
| `FileNotFoundError` | Check data files are in correct location |
| Out of memory | Use data subset or restart runtime |
| Runtime disconnected | Keep tab open, click cells periodically |
| Slow execution | Use GPU runtime type |

## 📞 Need Help?

See the main [Setup Guide](../docs/SETUP_GUIDE.md) for detailed instructions.

---

**Note**: The actual notebook files (.ipynb) should be created by following the implementation guides provided in the main documentation.
