# MedCare – AI Symptom Checker & Disease Prediction System

MedCare is a small web app that takes a few basic symptoms and health details, runs them through a trained machine learning model, and suggests the most likely condition along with general medicine and care guidance.

I built it as a Flask backend with a simple HTML/CSS front end, and it's set up to deploy on Render with one click.

> **Heads up:** this is a learning project. It is not a medical device and shouldn't replace advice from a real doctor. If something feels wrong, please see a healthcare professional.

---

## What it does

- Takes inputs like fever, cough, fatigue, difficulty breathing, age, gender, blood pressure and cholesterol level
- Predicts a likely disease using a trained scikit-learn model
- Shows a risk level based on how many symptoms are present and the patient's age and blood pressure
- Suggests related medicines and basic care tips from a built-in medicine database
- Includes About and Contact pages, plus a health-check endpoint for monitoring

## Tech stack

- **Backend:** Python, Flask, Flask-CORS
- **ML:** scikit-learn (Random Forest, Gradient Boosting and Logistic Regression were compared; the best model is saved as `best_model.pkl`), pandas, NumPy, joblib
- **Frontend:** HTML templates and CSS
- **Server and hosting:** Gunicorn, Render

## Project structure

```
.
├── app.py                  # Flask app and prediction logic
├── best_model.pkl          # Trained model
├── disease_encoder.pkl     # Label encoder for disease names
├── medicine_database.pkl   # Medicine and care suggestions
├── requirements.txt        # Python dependencies
├── render.yaml             # Render deployment config
├── templates/
│   ├── index.html
│   ├── about.html
│   └── contact.html
└── static/
    └── site.css
```

## Run it locally

You'll need Python 3.12.

```bash
git clone https://github.com/tamil1208/MedCare-AI-Symptom-Checker-Disease-Prediction-System.git
cd MedCare-AI-Symptom-Checker-Disease-Prediction-System

python -m venv venv
source venv/bin/activate        # on Windows: venv\Scripts\activate

pip install -r requirements.txt
python app.py
```

Then open http://localhost:5000 in your browser.

Please keep `scikit-learn` at the pinned version (1.6.1). The `.pkl` files were saved with it, and a different version may fail to load them.

## API

| Method | Endpoint   | What it does                              |
|--------|------------|-------------------------------------------|
| GET    | `/`        | Main symptom checker page                 |
| GET    | `/about`   | About page                                |
| GET    | `/contact` | Contact page                              |
| GET    | `/health`  | Returns app status and whether models loaded |
| GET    | `/models`  | Lists available models and disease count  |
| POST   | `/predict` | Returns a prediction for the given inputs |

Example request:

```bash
curl -X POST http://localhost:5000/predict \
  -H "Content-Type: application/json" \
  -d '{
    "fever": 1,
    "cough": 1,
    "fatigue": 0,
    "breathing": 0,
    "age": 35,
    "gender": 1,
    "bloodPressure": 1,
    "cholesterol": 1,
    "model": "rf"
  }'
```

Symptom fields use `1` for yes and `0` for no. `model` can be `rf`, `gb` or `lr`.

## Deploy on Render

1. Push this repo to GitHub (the files need to sit at the root).
2. In Render, choose **New → Blueprint** and connect the repo.
3. Click **Apply**. Render reads `render.yaml` and handles the rest.
4. Once it's live, check `/health` to confirm everything loaded.

On Render's free plan the app goes to sleep after about 15 minutes of inactivity, so the first visit after a break can take around 30 to 50 seconds.

## Disclaimer

MedCare gives general information only. Predictions come from a model trained on limited data and can be wrong. Never use it to diagnose yourself or to decide on treatment.

## Author

**Tamilarasan**
GitHub: [@tamil1208](https://github.com/tamil1208)
LinkedIn: [tamilarasan-a2466b274](https://linkedin.com/in/tamilarasan-a2466b274)
