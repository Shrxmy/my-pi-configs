---
name: "ml-engineer"
description: "Machine Learning Engineer specializing in production ML systems, model deployment, MLOps, and scalable ML infrastructure. Use when deploying ML models to production, building ML pipelines, setting up model monitoring, or implementing MLOps practices. Covers Docker, Kubernetes, MLflow, model serving, and cloud ML deployment."
compatibility: pi
---

# ML Engineer

Production-grade ML system design, deployment, and operations.

## Core Workflows

### 1. Model Serialization

```python
import joblib
import pickle

# Save model
joblib.dump(model, 'model.joblib')

# Load model
model = joblib.load('model.joblib')

# For XGBoost
model.save_model('model.json')

# For PyTorch
torch.save(model.state_dict(), 'model.pt')

# For TensorFlow
model.save('model_tf.h5')
```

### 2. Inference API

```python
from fastapi import FastAPI
import joblib

app = FastAPI()
model = joblib.load('model.joblib')

@app.post("/predict")
def predict(data: InputData):
    prediction = model.predict([data.features])
    return {"prediction": prediction.tolist()}
```

### 3. Docker for ML

```dockerfile
FROM python:3.11-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY model.joblib .
COPY app.py .

EXPOSE 8000
CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]
```

### 4. ML Pipeline with MLflow

```python
import mlflow
from mlflow.tracking import MlflowClient

# Start MLflow
mlflow.set_experiment("production_models")

with mlflow.start_run():
    # Log parameters
    mlflow.log_param("model_type", "xgboost")
    mlflow.log_param("n_estimators", 100)
    
    # Train model
    model.fit(X_train, y_train)
    
    # Log metrics
    mlflow.log_metric("accuracy", accuracy)
    mlflow.log_metric("auc", auc)
    
    # Log model
    mlflow.sklearn.log_model(model, "model")
```

### 5. Model Monitoring

```python
# Data drift detection
from evidently.dashboard import Dashboard
from evidently.tabs import DataDriftTab

report = Dashboard(tabs=[DataDriftTab()])
report.calculate(reference_data, current_data)
report.save("drift_report.html")
```

### 6. Batch Inference

```python
import pandas as pd
from multiprocessing import Pool

def process_batch(data_chunk):
    model = joblib.load('model.joblib')
    return model.predict(data_chunk)

# Process in chunks
chunks = pd.read_csv('data.csv', chunksize=10000)
predictions = []
for chunk in chunks:
    preds = process_batch(chunk)
    predictions.extend(preds)
```

## Deployment Platforms

### Kubernetes Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ml-model-deploy
spec:
  replicas: 3
  selector:
    matchLabels:
      app: ml-model
  template:
    spec:
      containers:
      - name: model
        image: myregistry/ml-model:v1
        ports:
        - containerPort: 8000
        resources:
          limits:
            memory: "2Gi"
            cpu: "1000m"
```

### AWS SageMaker

```python
import sagemaker
from sagemaker.serializers import CSVSerializer

predictor = model.deploy(
    instance_type='ml.t2.medium',
    initial_instance_count=1,
    serializer=CSVSerializer()
)

result = predictor.predict(data)
```

## MLOps Best Practices

1. **Version everything**: Data, code, models, experiments
2. **Automate training**: CI/CD for ML pipelines
3. **Monitor models**: Data drift, concept drift
4. **Rollback capabilities**: Easy model rollback
5. **A/B testing**: Canary deployments

## Quick Reference

```bash
# Save model
joblib.dump(model, 'model.joblib')

# Docker build
docker build -t model:latest .

# Docker run
docker run -p 8000:8000 model:latest

# Kubernetes apply
kubectl apply -f deployment.yaml
```