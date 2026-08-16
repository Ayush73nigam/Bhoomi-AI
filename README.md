# 🌿 BhoomiAI — Crop Recommendation System

Smart AI-powered crop advisory built on ICAR agroclimatic data and a **Random Forest** machine learning model. Recommends the best crop (Wheat, Rice, Maize, Sugarcane) from 16 soil & climate parameters. **99.5% test accuracy.**

---

## 📁 Project Structure

```
bhoomiai_project/
├── dataset/
│   └── crop_train.csv          ← 960-record ICAR training dataset
├── models/
│   └── rf_model.pkl            ← trained Random Forest (auto-generated)
├── static/
│   ├── css/
│   │   └── style.css
│   └── js/
│       └── main.js
├── templates/
│   ├── base.html
│   ├── index.html
│   ├── predict.html
│   ├── crops.html
│   ├── fertilizers.html
│   └── about.html
├── uploads/                    ← for future file uploads
├── results/
│   └── accuracy_report.txt     ← auto-generated after training
├── app.py                      ← Flask web application
├── functions.py                ← ML helpers, crop data, prediction logic
├── calculate_accuracy.py       ← Model trainer & evaluator
├── requirements.txt
└── README.md
```

---

## 🚀 Quick Start

### 1. Install dependencies
```bash
pip install -r requirements.txt
```

### 2. Train the model
```bash
python calculate_accuracy.py
```
This trains the Random Forest on `dataset/crop_train.csv`, saves `models/rf_model.pkl`, and writes a full accuracy report to `results/accuracy_report.txt`.

### 3. Run the web app
```bash
python app.py
```
Open `http://localhost:5000` in your browser.

---

## 🔬 Model Details

| Property | Value |
|---|---|
| Algorithm | Random Forest Classifier |
| Trees | 100 |
| Features | 16 (soil + climate) |
| Classes | Maize, Rice, Sugarcane, Wheat |
| Train / Test split | 80% / 20% (stratified) |
| Test accuracy | **99.48%** |
| 5-Fold CV accuracy | **99.69%** |

### Input Features
`Temp_min_C`, `Temp_max_C`, `Rain_min_cm`, `Rain_max_cm`, `Sow_temp_min`, `Sow_temp_max`, `Harvest_temp_min`, `Harvest_temp_max`, `Sand_pct`, `Clay_pct`, `Silt_pct`, `Nitrogen_N_kg_ha`, `Phosphorus_P_kg_ha`, `Potassium_K_kg_ha`, `Humidity_pct`, `pH`

---

## 🌐 API Endpoint (Raspberry Pi / IoT)

```bash
curl -X POST http://localhost:5000/api/predict \
  -H "Content-Type: application/json" \
  -d '{
    "tmin": 18, "tmax": 28,
    "rmin": 50, "rmax": 100,
    "stmin": 16, "stmax": 22,
    "htmin": 20, "htmax": 26,
    "sand": 40, "clay": 30, "silt": 30,
    "nitrogen": 90, "phosphorus": 45, "potassium": 45,
    "humidity": 65, "ph": 6.5
  }'
```

**Response:**
```json
{
  "crop": "Wheat",
  "confidence": 98.0,
  "votes": {"Wheat": 98.0, "Maize": 2.0, "Rice": 0.0, "Sugarcane": 0.0},
  "features_used": { ... }
}
```

---

## 🍓 Raspberry Pi Deployment

```bash
# On Raspberry Pi OS (64-bit)
pip install -r requirements.txt
python calculate_accuracy.py   # train once
gunicorn -w 2 -b 0.0.0.0:5000 app:app
```

Compatible with Raspberry Pi 3 / 4 / 5.

---

## 📊 Data Source

Dataset built on **ICAR Crop Production Guides** and **IMD Agroclimatic Zone** profiles. 960 records, 4 classes, perfectly balanced.

---

*© 2025 BhoomiAI · For educational & agricultural advisory use*
