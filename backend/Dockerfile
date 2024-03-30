FROM ubuntu:latest

# Install Python, pip, MongoDB, and other dependencies
RUN apt-get update && \
    apt-get install -y python3 python3-pip mongodb && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Set up working directory
WORKDIR /app

# Copy the requirements file and install dependencies for the FastAPI app
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

# Copy supervisord configuration file
COPY supervisord.conf /etc/supervisord.conf

# Copy the rest of your application's code
COPY . .

# Set environment variables for MongoDB and FastAPI app
ENV MONGO_URL=mongodb://localhost:27017
ENV DATABASE_NAME=todo_app
ENV COLLECTION_NAME=todo_items

# Expose the port the app runs on
EXPOSE 8000

# Install supervisord (to manage multiple processes)
RUN apk add --no-cache supervisor
