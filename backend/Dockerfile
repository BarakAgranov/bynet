FROM ubuntu:latest

# Install Python, pip, MongoDB, and other dependencies
# Install dependencies, Python, pip, and software-properties-common (to add new repository)
RUN apt-get update && \
    apt-get install -y python3 python3-pip software-properties-common && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Add the MongoDB 5.0 repository
RUN wget -qO - https://www.mongodb.org/static/pgp/server-5.0.asc | apt-key add - && \
    echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/5.0 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-5.0.list

# Install MongoDB
RUN apt-get update && \
    apt-get install -y mongodb-org && \
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
