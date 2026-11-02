# NYC Airbnb Room Type Predictor

A machine learning web app that predicts the **room type** of an NYC Airbnb listing (`Entire home/apt`, `Private room` or `Shared room`) from details such as location, price, minimum stay, reviews and host activity.

The project covers the full path from data to a usable product: a trained scikit-learn pipeline, a FastAPI prediction service, and a browser front-end.

## Features

- Multi-class classification of NYC Airbnb listings into three room types
- Returns the predicted class **and** the probability for each class
- Preprocessing and model are saved together as one pipeline, so training and serving behave identically
- REST API with strict input validation (Pydantic) and automatic interactive docs
- Simple HTML/CSS/JavaScript front-end with a form for entering listing details

## Tech Stack

| Layer | Tools |
| --- | --- |
| Modelling | Python, pandas, scikit-learn, joblib |
| API | FastAPI, Uvicorn, Pydantic |
| Front-end | HTML, CSS, vanilla JavaScript |
| Deployment | Render (static site for the UI, web service for the API) |

## Project Structure

```
.
├── main.py                                   # FastAPI app: loads the model, serves /predict
├── Model_Pipeline.pkl                        # Trained scikit-learn pipeline (preprocessing + classifier)
├── nyc_airbnb_room_type_classification.ipynb # EDA, cleaning, training and evaluation
├── index.html                                # Front-end form
├── style.css                                 # Front-end styling
├── script.js                                 # Front-end logic, calls the API
├── requirements.txt                          # Python dependencies
└── runtime.txt                               # Python version for deployment
```

## Dataset

The model is trained on the public **New York City Airbnb Open Data** (2019) from Kaggle. Each row is one listing with its location, price, reviews and host information. The target column is `room_type`.

## Input Features

| Field | Type | Description |
| --- | --- | --- |
| `latitude` | float | Latitude of the listing (-90 to 90) |
| `longitude` | float | Longitude of the listing (-180 to 180) |
| `price` | float | Price per night, must be greater than 0 |
| `minimum_nights` | int | Minimum nights required (1 to 365) |
| `number_of_reviews` | int | Total number of reviews |
| `reviews_per_month` | float | Average reviews per month |
| `calculated_host_listings_count` | int | Number of listings owned by the host |
| `availability_365` | int | Days available per year (0 to 365) |
| `neighbourhood_group` | string | Borough, e.g. Manhattan, Brooklyn, Queens, Bronx, Staten Island |
| `neighbourhood` | string | Specific neighbourhood name |

## Getting Started

### Prerequisites

- Python 3.10 or newer
- pip

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/NYC-Airbnb-Room-Type-Predictor.git
cd NYC-Airbnb-Room-Type-Predictor
```

### 2. Create a virtual environment and install dependencies

Windows:

```bash
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

macOS / Linux:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### 3. Start the API

```bash
uvicorn main:app --reload
```

The API runs at `http://127.0.0.1:8000`. Interactive documentation is available at `http://127.0.0.1:8000/docs`.

### 4. Start the front-end

Open a second terminal in the project folder:

```bash
python -m http.server 5500
```

Then visit `http://127.0.0.1:5500/index.html`.

> Make sure the `fetch(...)` URL in `script.js` points to your API, for example `http://127.0.0.1:8000/predict`.

## API Reference

### `GET /`

Health check. Returns a greeting message.

### `POST /predict`

Predicts the room type for one listing.

**Request body**

```json
{
  "latitude": 40.7580,
  "longitude": -73.9855,
  "price": 150,
  "minimum_nights": 2,
  "number_of_reviews": 10,
  "reviews_per_month": 0.5,
  "calculated_host_listings_count": 1,
  "availability_365": 180,
  "neighbourhood_group": "Manhattan",
  "neighbourhood": "Midtown"
}
```

**Response**

```json
{
  "Predicted_room_type": "Entire home/apt",
  "Probability": [0.81, 0.17, 0.02]
}
```

`Probability` lists the class probabilities in the same order as the model's classes, which is alphabetical: `Entire home/apt`, `Private room`, `Shared room`.

**Example with curl**

```bash
curl -X POST http://127.0.0.1:8000/predict \
  -H "Content-Type: application/json" \
  -d '{"latitude":40.758,"longitude":-73.9855,"price":150,"minimum_nights":2,"number_of_reviews":10,"reviews_per_month":0.5,"calculated_host_listings_count":1,"availability_365":180,"neighbourhood_group":"Manhattan","neighbourhood":"Midtown"}'
```

## Model Training

The full workflow lives in `nyc_airbnb_room_type_classification.ipynb`:

1. Load and inspect the raw data
2. Exploratory data analysis
3. Cleaning and feature preparation
4. Train/test split
5. Compare candidate models and pick the best performer
6. Evaluate on held-out data
7. Save the final pipeline with joblib as `Model_Pipeline.pkl`

### Results

| Metric | Score |
| --- | --- |
| Accuracy | _add your test accuracy_ |
| Weighted F1 | _add your weighted F1_ |

_Fill in these numbers from the final cell of the notebook._

## Deployment

The project is designed to be deployed on [Render](https://render.com) as two pieces:

- **API**: a Web Service running `uvicorn main:app --host 0.0.0.0 --port $PORT`
- **Front-end**: a Static Site serving `index.html`, `style.css` and `script.js`

After deploying, update the API URL in `script.js` to your live service address.

## Possible Improvements

- Restrict CORS to the front-end's domain instead of allowing all origins
- Add automated tests for the API
- Containerise with Docker
- Add a short model card with class balance and a confusion matrix
- Use `model_dump()` instead of the deprecated `.dict()` in Pydantic 2

## Acknowledgements

Built from the open-source repository [tanishq-latent/NYC-Airbnb-Room-Type-Predictor](https://github.com/tanishq-latent/NYC-Airbnb-Room-Type-Predictor). Data from the Kaggle "New York City Airbnb Open Data" set.

## License

Add a license of your choice (for example MIT) before publishing.
