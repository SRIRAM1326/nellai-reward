FROM python:3.11-slim

WORKDIR /app

# Copy requirements and install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application files
COPY . .

# Hugging Face Spaces listens on port 7860
EXPOSE 7860

# Start Gunicorn server on port 7860
CMD ["gunicorn", "-b", "0.0.0.0:7860", "app:app"]
