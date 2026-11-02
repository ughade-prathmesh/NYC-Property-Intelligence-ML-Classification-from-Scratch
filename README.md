<div align="center">

# NYC Property Intelligence

**Predict whether an NYC Airbnb listing is an entire home, a private room or a shared room.**

![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?logo=scikitlearn&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?logo=fastapi&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green)

<br>

<img src="screenshots/home.png" alt="Home page" width="760">

<br><br>

<table>
  <tr>
    <td><img src="screenshots/form.png" alt="Listing form"></td>
    <td><img src="screenshots/prediction.png" alt="Prediction result"></td>
  </tr>
</table>

</div>

## About

A machine learning web app trained on the NYC Airbnb Open Data. Enter a listing's location, price, reviews and host details, and it returns the most likely room type with a probability for each class.

- scikit-learn pipeline saved with joblib
- FastAPI backend with validated input
- Plain HTML, CSS and JavaScript front-end

## Run Locally

```bash
git clone https://github.com/ughade-prathmesh/NYC-Property-Intelligence-ML-Classification-from-Scratch.git
cd NYC-Property-Intelligence-ML-Classification-from-Scratch

python -m venv .venv
.venv\Scripts\activate        # macOS/Linux: source .venv/bin/activate
pip install -r requirements.txt

uvicorn main:app --reload
```

In a second terminal, serve the front-end and open `http://127.0.0.1:5500/index.html`:

```bash
python -m http.server 5500
```

API docs are available at `http://127.0.0.1:8000/docs`.

## API

`POST /predict`

```json
{
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
}
```

## Author

**Prathmesh Ughade** · [@ughade-prathmesh](https://github.com/ughade-prathmesh)

Released under the [MIT License](LICENSE).
