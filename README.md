# Insurance Premium Prediction API

A FastAPI-based machine learning service that predicts insurance premium categories based on user demographics and lifestyle factors.

## 🚀 Features

- **Real-time Predictions**: Get instant insurance premium category predictions
- **RESTful API**: Clean and intuitive API endpoints
- **Data Validation**: Robust input validation using Pydantic models
- **Health Monitoring**: Built-in health check endpoint
- **Docker Support**: Containerized deployment ready
- **City Tier Classification**: Automatic city tier assignment for Indian cities

## 📋 API Endpoints

### `GET /`
Returns a welcome message for the API.

**Response:**
```json
{
  "message": "Insurance Premium Prediction API"
}
```

### `GET /health`
Health check endpoint that returns API status and model information.

**Response:**
```json
{
  "status": "OK",
  "version": "1.0.0",
  "model_loaded": true
}
```

### `POST /predict`
Predicts insurance premium category based on user input.

**Request Body:**
```json
{
  "age": 35,
  "weight": 70.5,
  "height": 1.75,
  "income_lpa": 8.5,
  "smoker": false,
  "city": "Mumbai",
  "occupation": "private_job"
}
```

**Response:**
```json
{
  "response": {
    "predicted_category": "Medium",
    "confidence": 0.8432,
    "class_probabilities": {
      "Low": 0.01,
      "Medium": 0.84,
      "High": 0.15
    }
  }
}
```

## 📊 Input Parameters

| Parameter | Type | Description | Validation |
|-----------|------|-------------|------------|
| `age` | integer | User's age in years | 1-119 |
| `weight` | float | User's weight in kg | > 0 |
| `height` | float | User's height in meters | 0-2.5 |
| `income_lpa` | float | Annual income in lakhs | > 0 |
| `smoker` | boolean | Whether user is a smoker | true/false |
| `city` | string | User's city of residence | Any Indian city |
| `occupation` | string | User's occupation | See allowed values below |

### Allowed Occupation Values
- `retired`
- `freelancer`
- `student`
- `government_job`
- `business_owner`
- `unemployed`
- `private_job`

## 🏙️ City Tier Classification

The API automatically classifies cities into tiers:

- **Tier 1**: Mumbai, Delhi, Bangalore, Chennai, Kolkata, Hyderabad, Pune
- **Tier 2**: 50+ major cities including Jaipur, Chandigarh, Indore, etc.
- **Tier 3**: All other cities

## 🧮 Computed Features

The API automatically calculates additional features:

- **BMI**: Calculated from weight and height
- **Lifestyle Risk**: Based on smoking status and BMI
  - High: Smoker + BMI > 30
  - Medium: Smoker OR BMI > 27
  - Low: Non-smoker + BMI ≤ 27
- **Age Group**: 
  - Young: < 25
  - Adult: 25-44
  - Middle-aged: 45-59
  - Senior: ≥ 60

## 🐳 Docker Deployment

### Pull from Docker Hub
```bash
docker pull adityakumar3594/insurance-premium-api:latest
```

### Run the Container
```bash
docker run -d -p 8000:8000 adityakumar3594/insurance-premium-api:latest
```

### Build Locally
```bash
# Clone the repository
git clone <repository-url>
cd insurance-premium-predictor

# Build the image
docker build -t insurance-premium-api .

# Run the container
docker run -d -p 8000:8000 insurance-premium-api
```

## 🛠️ Local Development

### Prerequisites
- Python 3.11+
- pip

### Installation
```bash
# Clone the repository
git clone <repository-url>
cd insurance-premium-predictor

# Create virtual environment
python -m venv myenv
source myenv/bin/activate  # On Windows: myenv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Run the application
uvicorn app:app --host 0.0.0.0 --port 8000 --reload
```

### Access the API
- **API Base URL**: http://localhost:8000
- **Interactive Docs**: http://localhost:8000/docs
- **ReDoc**: http://localhost:8000/redoc

## 📁 Project Structure

```
├── app.py                 # FastAPI application
├── Dockerfile            # Docker configuration
├── requirements.txt      # Python dependencies
├── model/
│   ├── model.pkl        # Trained ML model
│   └── predict.py       # Prediction logic
├── schema/
│   ├── user_input.py    # Input validation schema
│   └── prediction_response.py  # Response schema
└── config/
    └── city_tier.py     # City tier configuration
```

## 🔧 API Usage Examples

### Using cURL
```bash
curl -X POST "http://localhost:8000/predict" \
     -H "Content-Type: application/json" \
     -d '{
       "age": 35,
       "weight": 70.5,
       "height": 1.75,
       "income_lpa": 8.5,
       "smoker": false,
       "city": "Mumbai",
       "occupation": "private_job"
     }'
```

### Using Python requests
```python
import requests

url = "http://localhost:8000/predict"
data = {
    "age": 35,
    "weight": 70.5,
    "height": 1.75,
    "income_lpa": 8.5,
    "smoker": False,
    "city": "Mumbai",
    "occupation": "private_job"
}

response = requests.post(url, json=data)
print(response.json())
```

## 🚨 Error Handling

The API returns appropriate HTTP status codes:

- **200**: Successful prediction
- **422**: Validation error (invalid input)
- **500**: Internal server error

Example error response:
```json
{
  "detail": [
    {
      "loc": ["body", "age"],
      "msg": "ensure this value is greater than 0",
      "type": "value_error.number.not_gt"
    }
  ]
}
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## 📄 License

This project is licensed under the MIT License.

## 📞 Support

For issues and questions, please open an issue in the repository or contact the maintainer.

---

**Built with FastAPI, scikit-learn, and Docker** 🐍🚀