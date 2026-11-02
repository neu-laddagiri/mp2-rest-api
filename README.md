# MP2: Data Processing API

A FastAPI app that turns my MP1 data-cleaning pipeline into a REST API.

- `GET /health` reports the app's status and version.
- `POST /upload` accepts a CSV file and stores it in memory under a new ID.
- `GET /datasets/{dataset_id}` returns the stored rows (with a `limit` query parameter) and the total row count.
- `POST /datasets/{dataset_id}/process` takes a JSON config and runs the reused MP1 validation and cleaning functions from `data_pipeline/`. The config covers required and numeric columns, duplicate removal, missing-value handling, and outlier settings. The cleaned data replaces the stored dataset, and the endpoint returns a cleaning report.

Bad uploads and bad settings return 400, unknown IDs return 404, and invalid input returns 422.

A middleware logs every request with a request ID, status code, and duration, and sends the ID back in the `X-Request-ID` header. The API is tested with pytest and FastAPI's TestClient.

## Run

```powershell
python -m uvicorn main:app --reload
```

Then open http://127.0.0.1:8000/docs.

## Test

```powershell
python -m pytest tests -v
```