# streaming anomaly detector

a real-time streaming anomaly detection system for time-series and metric streams, combining online running statistics with sliding-window isolation forest machine learning.

## architecture

1. **stream broker**: asynchronous in-memory pub-sub event broker with queue depth controls.
2. **online detector**:
   - running mean, variance, and z-score tracking via welford algorithm.
   - sliding window feature extraction (rolling mean, rolling std, first-difference delta, rolling z-score).
   - periodic retrain of isolation forest model on sliding feature history.
   - inlier-filtered statistics to prevent baseline variance corruption from extreme outliers.
3. **storage layer**: sqlite-backed event logging and anomaly query store with index optimization.
4. **streaming engine**: asynchronous coordinator processing stream events, computing anomaly scores, and persisting records.
5. **fastapi web service**: rest api for ingestion, batch ingestion, status metrics, and anomaly inspection.
6. **streamlit dashboard**: interactive live visualization and manual anomaly injection interface.

## installation

```bash
pip install -r requirements.txt
```

## running tests

```bash
python -m unittest discover -s tests -p "test_*.py"
```

## running the demo

```bash
python run_demo.py
```

## starting the api service

```bash
uvicorn src.app:app --host 0.0.0.0 --port 8000
```

api endpoints:
- `get /api/v1/health`: service health check
- `post /api/v1/events`: ingest a single metric event
- `post /api/v1/events/batch`: ingest multiple metric events
- `get /api/v1/anomalies`: retrieve detected anomalies
- `get /api/v1/stats`: summary statistics and detector status
- `post /api/v1/stream/start`: start synthetic stream generator
- `post /api/v1/stream/stop`: stop synthetic stream generator
- `post /api/v1/stream/inject-anomaly`: inject a simulated spike or drop

## starting the monitoring dashboard

```bash
streamlit run src/dashboard.py
```
