## Backend Startup Guide

### 1. Activate the Conda Environment
```powershell
conda activate hackathon
```

### 2. Install Backend Dependencies
```powershell
cd backend
pip install -r requirements.txt
# or, if you use Poetry:
# poetry install
```

### 3. Configure Environment Variables
```powershell
cp env.template .env
```
Edit `.env` as needed (LLM provider, database URL, etc.).

### 4. Start the FastAPI Server
```powershell
python -m app.main
# or
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

### 5. Stop Work
```powershell
conda deactivate
```

