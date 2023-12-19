FROM python:3.12.0-slim@sha256:19a6235339a74eca01227b03629f63b6f5020abc21142436eced6ec3a9839a76 as base

FROM base as builder

# Install dependencies.
COPY requirements.txt .
RUN pip install --prefix="/install" -r requirements.txt

FROM base

# Set our working directory
WORKDIR /app

COPY --from=builder /install /usr/local

# Add application code.
COPY locustfile.py .

# show python logs as they occur
ENV PYTHONUNBUFFERED=0

# enable gevent support in debugger
ENV GEVENT_SUPPORT=True

# explicitly set a fallback log level in case no log level is defined by Kubernetes
ENV LOG_LEVEL info

# start loadgenerator
ENTRYPOINT locust --host="http://${FRONTEND_ADDR}" --loglevel $LOG_LEVEL --headless --users="${USERS:-10}" 2>&1
