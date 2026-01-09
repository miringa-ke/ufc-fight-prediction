# Data Directory

This directory contains all datasets used for the UFC fight prediction system.

## 📁 Directory Structure

```
data/
├── raw/                    # Original, unprocessed datasets
│   ├── data.csv
│   ├── preprocessed_data.csv
│   ├── raw_fighter_details.csv
│   └── raw_total_fight_data.csv
├── processed/              # Cleaned and feature-engineered data
│   ├── cleaned_data.csv
│   ├── elo_ratings.csv
│   └── feature_engineered_data.csv
└── predictions/            # Prediction history
    └── prediction_history.csv
```

## 📊 Required Data Files

### 1. data.csv
**Description**: Aggregated fight data with pre-calculated fighter averages

**Columns** (139 total):
- Fighter identifiers: `R_fighter`, `B_fighter`
- Fight metadata: `Referee`, `date`, `location`, `Winner`, `title_bout`, `weight_class`
- Fighter stats: `B_avg_*` and `R_avg_*` for various metrics
- Physical attributes: `B_Height_cms`, `R_Reach_cms`, etc.
- Fight history: `B_wins`, `R_losses`, win streaks, etc.

**Expected Size**: ~2-5 MB  
**Rows**: ~6,000 fights

---

### 2. preprocessed_data.csv
**Description**: Feature-engineered version of fight data with encoded categorical variables

**Columns** (155 total):
- All numeric features from `data.csv`
- One-hot encoded weight classes: `weight_class_Heavyweight`, etc.
- One-hot encoded stances: `B_Stance_Orthodox`, `R_Stance_Southpaw`, etc.

**Expected Size**: ~3-6 MB  
**Rows**: ~5,900 fights

---

### 3. raw_fighter_details.csv
**Description**: Individual fighter profiles with physical and career statistics

**Columns**:
- `fighter_name`: Fighter's full name
- `Height`: Height in format (e.g., "6' 4\"")
- `Weight`: Weight in lbs
- `Reach`: Reach in inches
- `Stance`: Fighting stance (Orthodox, Southpaw, Switch)
- `DOB`: Date of birth
- `SLpM`: Significant strikes landed per minute
- `Str_Acc`: Striking accuracy percentage
- `SApM`: Significant strikes absorbed per minute
- `Str_Def`: Strike defense percentage
- `TD_Avg`: Average takedowns per 15 minutes
- `TD_Acc`: Takedown accuracy
- `TD_Def`: Takedown defense
- `Sub_Avg`: Average submissions per 15 minutes

**Expected Size**: ~500 KB - 1 MB  
**Rows**: ~3,500-4,000 fighters

---

### 4. raw_total_fight_data.csv
**Description**: Detailed round-by-round fight statistics

**Columns** (42 total):
- Fighter identifiers: `R_fighter`, `B_fighter`
- Per-fight stats: `R_KD`, `B_SIG_STR.`, `R_TD_pct`, etc.
- Striking breakdown: Head, body, leg strikes (attempted/landed)
- Position-specific: Distance, clinch, ground strikes
- Fight outcome: `Winner`, `win_by`, `last_round`, `last_round_time`
- Metadata: `date`, `location`, `Referee`, `Format`, `Fight_type`

**Expected Size**: ~2-4 MB  
**Rows**: ~6,000 fights

**Delimiter**: Semicolon (`;`)

---

## 🔍 Data Sources

### Community Sources
- **Kaggle**: Search for "UFC fight statistics"

---

## ⚠️ Important Notes

### Why Data is Not Included

Data files are **NOT included in this repository** because:
1. **Size**: Combined size is 10-20 MB (too large for Git)
2. **Copyright**: May contain proprietary UFC statistics
3. **Updates**: Data becomes stale; users should get latest version
4. **Privacy**: Fighter information should be obtained through official channels

### Data Quality Checklist

Before using the data, verify:

- ✅ All 4 CSV files are present in `data/raw/`
- ✅ File encodings are UTF-8
- ✅ Date columns are in YYYY-MM-DD format
- ✅ No missing winner information
- ✅ Fighter names are consistent across files
- ✅ Numeric columns have no text values
- ✅ Delimiter in `raw_total_fight_data.csv` is semicolon (`;`)

### Data Validation Script

Run this to validate your data:

```python
import pandas as pd

# Check if files exist and load them
files = {
    'data': 'data/raw/data.csv',
    'preprocessed': 'data/raw/preprocessed_data.csv',
    'fighter_details': 'data/raw/raw_fighter_details.csv',
    'fight_data': 'data/raw/raw_total_fight_data.csv'
}

for name, path in files.items():
    try:
        if name == 'fight_data':
            df = pd.read_csv(path, sep=';')
        else:
            df = pd.read_csv(path)
        print(f"✅ {name}: {len(df)} rows, {len(df.columns)} columns")
    except FileNotFoundError:
        print(f"❌ {name}: File not found at {path}")
    except Exception as e:
        print(f"❌ {name}: Error - {e}")
```

---

## 📝 Data Schema

### Key Relationships

```
raw_fighter_details.csv (fighter_name)
         ↓
    Used to enrich
         ↓
raw_total_fight_data.csv (R_fighter, B_fighter)
         ↓
    Aggregated to create
         ↓
data.csv (with averages)
         ↓
    Further processed to
         ↓
preprocessed_data.csv (with encoding)
```

### Feature Categories in Final Dataset

1. **ELO Features**: Dynamic skill ratings
2. **Physical**: Height, reach, age, weight
3. **Performance**: Striking %, takedown %, control time
4. **Experience**: Total fights, win rate, streaks
5. **Categorical**: Weight class, stance, title bout

---

## 🔄 Data Updates

To add new fight data:

1. Append new fights to `raw_total_fight_data.csv`
2. Add new fighters to `raw_fighter_details.csv` (if needed)
3. Re-run Phase 1-3 notebooks to:
   - Recalculate ELO ratings
   - Re-engineer features
   - Retrain models

**Frequency**: Monthly (after major UFC events)

---

## 📧 Need Help?

If you need assistance obtaining or preparing data:
1. Check the [Setup Guide](../docs/SETUP_GUIDE.md)
2. Open an issue on GitHub
3. Contact: [@miringa-ke](https://github.com/miringa-ke)

---

**Last Updated**: January 2026
