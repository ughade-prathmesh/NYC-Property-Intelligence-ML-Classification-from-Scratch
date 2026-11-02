<div align="center">

# NYC Property Intelligence

### ML Classification from Scratch

Predict whether an NYC Airbnb listing is an entire home, a private room or a shared room, with a trained model served through a FastAPI backend and a clean web interface.

![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-Pipeline-F7931E?logo=scikitlearn&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-API-009688?logo=fastapi&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-Front--end-F7DF1E?logo=javascript&logoColor=black)
![License](https://img.shields.io/badge/License-MIT-green)

</div>

---

## Overview

This project covers the complete machine learning lifecycle on the public NYC Airbnb Open Data: exploring and cleaning the data, training a classification pipeline, exposing it as a REST API, and presenting it through a responsive web interface.

Given a listing's location, price, stay requirements, review activity and host profile, the application returns the **most likely room type** along with the **probability of each class**.



## Features

- **Three-class prediction**: Entire home/apt, Private room, Shared room
- **Confidence scores**: probability for every class, visualised in the interface
- **Single pipeline artifact**: preprocessing and model saved together, so training and serving stay consistent
- **Validated API**: Pydantic enforces types and ranges and returns clear errors for bad input
- **Auto-generated docs**: interactive Swagger UI at `/docs`
- **No build step**: the front-end is plain HTML, CSS and JavaScript

## Tech Stack

| Area | Technologies |
| --- | --- |
| Data and modelling | Python, pandas, scikit-learn, joblib, Jupyter |
| Backend | FastAPI, Uvicorn, Pydantic |
| Front-end | HTML5, CSS3, JavaScript |
| Deployment | Render |

## How It Works

```
Browser form  ──►  FastAPI  ──►  Validation  ──►  scikit-learn pipeline  ──►  Room type + probabilities
(index.html)       /predict      (Pydantic)        (Model_Pipeline.pkl)         returned as JSON
```

1. The user enters listing details in the web form.
2. The front-end sends them as JSON to the `/predict` endpoint.
3. FastAPI validates the input and builds a DataFrame.
4. The saved pipeline preprocesses the data and predicts the class.
5. The response is displayed as the predicted room type and a probability breakdown.

## Project Structure

```
.
├── main.py                                   # FastAPI application
├── Model_Pipeline.pkl                        # Trained preprocessing + classifier pipeline
├── nyc_airbnb_room_type_classification.ipynb # EDA, cleaning, training and evaluation
├── index.html                                # Web interface
├── style.css                                 # Styling
├── script.js                                 # Front-end logic
├── the_build_line_guide.html                 # Interactive build and deployment guide
├── screenshots/                              # Images used in this README
├── requirements.txt                          # Python dependencies
├── runtime.txt                               # Python runtime for deployment
├── LICENSE
└── README.md
```

## Getting Started

### Prerequisites

- Python 3.10 or newer
- Git

### Installation

```bash
git clone https://github.com/ughade-prathmesh/NYC-Property-Intelligence-ML-Classification-from-Scratch.git
cd NYC-Property-Intelligence-ML-Classification-from-Scratch

python -m venv .venv
.venv\Scripts\activate          # macOS / Linux: source .venv/bin/activate

pip install -r requirements.txt
```

### Run the API

```bash
uvicorn main:app --reload
```

The API is served at `http://127.0.0.1:8000` and the interactive documentation at `http://127.0.0.1:8000/docs`.

### Run the front-end

In a second terminal, from the project folder:

```bash
python -m http.server 5500
```

Open `http://127.0.0.1:5500/index.html`. For local use, the endpoint in `script.js` should be `http://127.0.0.1:8000/predict`.

## API Reference

### `GET /`

Health check.

### `POST /predict`

**Request body**

| Field | Type | Constraint |
| --- | --- | --- |
| `latitude` | float | -90 to 90 |
| `longitude` | float | -180 to 180 |
| `price` | float | greater than 0 |
| `minimum_nights` | int | 1 to 365 |
| `number_of_reviews` | int | 0 or more |
| `reviews_per_month` | float | 0 or more |
| `calculated_host_listings_count` | int | 0 or more |
| `availability_365` | int | 0 to 365 |
| `neighbourhood_group` | string | non-empty |
| `neighbourhood` | string | non-empty |

**Example request**

```bash
curl -X POST http://127.0.0.1:8000/predict \
  -H "Content-Type: application/json" \
  -d '{
    "latitude": 40.758,
    "longitude": -73.9855,
    "price": 150,
    "minimum_nights": 2,
    "number_of_reviews": 10,
    "reviews_per_month": 0.5,
    "calculated_host_listings_count": 1,
    "availability_365": 180,
    "neighbourhood_group": "Manhattan",
    "neighbourhood": "Midtown"
  }'
```

**Example response**

```json
{
  "Predicted_room_type": "Entire home/apt",
  "Probability": [0.81, 0.17, 0.02]
}
```

Probabilities follow the order of the model's classes: Entire home/apt, Private room, Shared room. Invalid input returns `422 Unprocessable Entity` with the offending field.

## Model Development

The notebook `nyc_airbnb_room_type_classification.ipynb` walks through the full workflow:

1. Load and inspect the dataset
2. Explore distributions and relationships
3. Clean the data and prepare features
4. Split into training and test sets
5. Train and evaluate classification models
6. Export the final pipeline with joblib as `Model_Pipeline.pkl`

Dataset: [New York City Airbnb Open Data (Kaggle)](https://www.kaggle.com/datasets/dgomonov/new-york-city-airbnb-open-data)

## Deployment

The app is designed for [Render](https://render.com) as two services:

| Service | Type | Details |
| --- | --- | --- |
| API | Web Service | Start command: `uvicorn main:app --host 0.0.0.0 --port $PORT` |
| Front-end | Static Site | Serves `index.html`, `style.css` and `script.js` |

After the API is live, set the endpoint in `script.js` to its public URL followed by `/predict`.

## Roadmap

- [ ] Restrict CORS to the deployed front-end origin
- [ ] Add automated API tests
- [ ] Provide a Dockerfile
- [ ] Add a model card with a confusion matrix

## Author

**Prathmesh Ughade**
GitHub: [@ughade-prathmesh](https://github.com/ughade-prathmesh)

## License

Released under the MIT License. See [LICENSE](LICENSE) for details.
