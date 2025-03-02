FROM python:3.10-slim

WORKDIR /app

# Install required packages
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application files
COPY . .

# Environment variables for New Relic - these can be overridden at runtime
ENV NEW_RELIC_CONFIG_FILE=newrelic.ini
ENV NEW_RELIC_ENVIRONMENT=production
# NEW_RELIC_LICENSE_KEY should be passed at runtime

# Expose the application port
EXPOSE 4000

# Command to run the application with New Relic monitoring
CMD ["newrelic-admin", "run-program", "python", "app.py"]