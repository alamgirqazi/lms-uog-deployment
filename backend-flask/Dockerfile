FROM python:3.13.2-slim

WORKDIR /app

# Set environment variables
ENV NEW_RELIC_APP_NAME="LMS System - Backend"
ENV NEW_RELIC_ENVIRONMENT=production
ENV NEW_RELIC_CONFIG_FILE=newrelic.ini

# Install required packages
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application files
COPY . .

# Create uploads directory for file submissions
RUN mkdir -p uploads && chmod 755 uploads

# Expose the application port
EXPOSE 4000

# Create an entrypoint script to configure New Relic at runtime
RUN echo '#!/bin/bash' > /app/entrypoint.sh && \
    echo 'echo "[newrelic]" > newrelic.ini' >> /app/entrypoint.sh && \
    echo 'echo "license_key = $NEW_RELIC_LICENSE_KEY" >> newrelic.ini' >> /app/entrypoint.sh && \
    echo 'echo "app_name = $NEW_RELIC_APP_NAME" >> newrelic.ini' >> /app/entrypoint.sh && \
    echo 'echo "monitor_mode = true" >> newrelic.ini' >> /app/entrypoint.sh && \
    echo 'echo "log_level = info" >> newrelic.ini' >> /app/entrypoint.sh && \
    echo 'echo "log_file = stderr" >> newrelic.ini' >> /app/entrypoint.sh && \
    echo 'echo "distributed_tracing.enabled = true" >> newrelic.ini' >> /app/entrypoint.sh && \
    echo 'echo "transaction_tracer.enabled = true" >> newrelic.ini' >> /app/entrypoint.sh && \
    echo 'echo "transaction_tracer.transaction_threshold = 0.5" >> newrelic.ini' >> /app/entrypoint.sh && \
    echo 'echo "error_collector.enabled = true" >> newrelic.ini' >> /app/entrypoint.sh && \
    echo 'echo "browser_monitoring.auto_instrument = true" >> newrelic.ini' >> /app/entrypoint.sh && \
    echo 'echo "" >> newrelic.ini' >> /app/entrypoint.sh && \
    echo 'echo "[newrelic:development]" >> newrelic.ini' >> /app/entrypoint.sh && \
    echo 'echo "monitor_mode = false" >> newrelic.ini' >> /app/entrypoint.sh && \
    echo 'echo "developer_mode = true" >> newrelic.ini' >> /app/entrypoint.sh && \
    echo 'echo "" >> newrelic.ini' >> /app/entrypoint.sh && \
    echo 'echo "[newrelic:production]" >> newrelic.ini' >> /app/entrypoint.sh && \
    echo 'echo "monitor_mode = true" >> newrelic.ini' >> /app/entrypoint.sh && \
    echo 'echo "high_security = false" >> newrelic.ini' >> /app/entrypoint.sh && \
    echo 'exec newrelic-admin run-program python app.py' >> /app/entrypoint.sh && \
    chmod +x /app/entrypoint.sh

# Add healthcheck
HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=3 \
  CMD curl -f http://localhost:4000/ || exit 1

# Use the entrypoint script to run the application
ENTRYPOINT ["/app/entrypoint.sh"]