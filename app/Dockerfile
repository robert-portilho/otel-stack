FROM quay.io/alexmartbrad25/teste-node-v2

USER root

# Setup a separate directory for the instrumentation code
RUN mkdir -p /otel-utils

# Copy the instrumentation script
COPY instrumentation.js /otel-utils/

# Install OpenTelemetry dependencies
# We use 'cd' to avoid changing the global WORKDIR, preventing issues with 'npm start'
RUN cd /otel-utils && \
    npm init -y && \
    npm install @opentelemetry/api \
    @opentelemetry/sdk-node \
    @opentelemetry/auto-instrumentations-node \
    @opentelemetry/exporter-trace-otlp-grpc \
    @opentelemetry/exporter-metrics-otlp-grpc \
    @opentelemetry/exporter-logs-otlp-grpc \
    @opentelemetry/resources \
    @opentelemetry/semantic-conventions \
    @opentelemetry/sdk-logs \
    @opentelemetry/sdk-metrics

# Ensure permissions
RUN chmod -R 755 /otel-utils

# Enable auto-instrumentation via environment variable
# We add NODE_PATH so Node.js can find the modules installed in /otel-utils/node_modules
ENV NODE_PATH="/otel-utils/node_modules"
ENV NODE_OPTIONS="--require /otel-utils/instrumentation.js"

# Execute the app directly
CMD ["node", "dist/src/main.js"]
