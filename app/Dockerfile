FROM python:3.9-slim

WORKDIR /app

# Install system dependencies for eBPF
RUN apt-get update && apt-get install -y \
    linux-headers-generic \
    build-essential \
    python3-dev \
    && rm -rf /var/lib/apt/lists/*

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]