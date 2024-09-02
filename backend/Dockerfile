# Use Python 3.11 slim-bullseye as the base image
FROM python:3.11-slim-bullseye

# Set environment variable for Python
ENV PYTHONUNBUFFERED=1

# Create a new user to run the application
RUN groupadd -r user && useradd --no-log-init -r -g user user

# Set the working directory inside the container
WORKDIR /app

# Install system dependecies
RUN apt-get update && apt-get install -y gcc build-essential libpq-dev git && \
    python3 -m pip install --no-cache-dir pip-tools && \
    apt-get purge -y libpq-dev && apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/* /var/cache/apt/*

# Copy the requirements.in file to the working directory
COPY backend/requirements.in /app/

# Compile the dependency file
RUN pip-compile requirements.in > requirements.txt

# Install Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the application to the working directory
COPY backend /app

# Switch to the created user
USER user

# Expose the port the application will run on
EXPOSE 8000

# Command to run the FastAPI application using uvicorn
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000", "--reload"]
