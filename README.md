# Emotion Prediction

A lightweight web application for detecting emotional intent in text using a trained BiGRU neural network. The app exposes a FastAPI backend and a browser-based frontend that lets users enter a sentence and instantly see the predicted emotion along with the confidence score.

## Project overview

This project classifies text into six emotional categories:

- sadness
- joy
- love
- anger
- fear
- surprise

The pipeline includes:

- text preprocessing (lowercasing, punctuation cleanup, whitespace normalization)
- tokenization using a saved Keras tokenizer
- sequence padding to a fixed length
- emotion prediction with a saved BiGRU model
- REST API and UI integration

## Tech stack

- Python 3.11
- FastAPI
- Uvicorn
- TensorFlow (CPU)
- Keras
- NumPy
- Pydantic
- HTML/CSS/JavaScript for the frontend

## Project structure

```text
Emotion Prediction/
├── Artifacts/
│   ├── BiGRU_Model.keras
│   └── tokenizer.pkl
├── static/
│   ├── index.html
│   ├── style.css
│   └── script.js
├── main.py
├── requirements.txt
├── runtime.txt
├── final_clean.ipynb
└── README.md
```

## How it works

1. A user enters text in the browser UI or sends a request to the API.
2. The backend cleans the input by:
   - converting to lowercase
   - removing apostrophes
   - stripping punctuation and special characters
   - normalizing spaces
3. The tokenizer converts words to numeric sequences.
4. The sequence is padded to a maximum length of 50 tokens.
5. The saved BiGRU model predicts probabilities for each emotion label.
6. The app returns:
   - predicted emotion
   - confidence score
   - full probability breakdown

## Backend API

### Health check

- URL: `/health`
- Method: `GET`

Example response:

```json
{
  "status": "Server is running",
  "model_loaded": true
}
```

### Predict emotion

- URL: `/predict`
- Method: `POST`
- Request body:

```json
{
  "text": "I feel so happy and excited"
}
```

Example response:

```json
{
  "text": "I feel so happy and excited",
  "predicted_emotion": "joy",
  "confidence": 0.9274,
  "all_probabilites": {
    "sadness": 0.01,
    "joy": 0.93,
    "love": 0.02,
    "anger": 0.01,
    "fear": 0.01,
    "surprise": 0.03
  }
}
```

## Running the app locally

### 1. Create a virtual environment

```bash
python -m venv .venv
```

On Windows:

```bash
.venv\Scripts\activate
```

On macOS/Linux:

```bash
source .venv/bin/activate
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Start the application

```bash
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

Then open:

```text
http://localhost:8000/
```

The app serves the UI at the root path `/` and mounts static files under `/static`.

## Example curl request

```bash
curl -X POST "http://localhost:8000/predict" \
  -H "Content-Type: application/json" \
  -d '{"text":"I am so excited and proud of my work."}'
```

## Notes

- The model and tokenizer are loaded once at startup and kept in memory for reuse.
- CORS is enabled so the API can be used from different origins during development.
- The project is designed for a trained model artifact; if the model files are missing or incompatible, the prediction endpoint will return a 503 error.

## License

This project does not include a license file. If you plan to share or distribute it publicly, add an appropriate open-source license before release.
