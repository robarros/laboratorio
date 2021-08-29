# syntax = docker/dockerfile:1.3
#
# IMAGE BASE
FROM python:3.9-slim as python-base
ENV PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1 \
    PIP_NO_CACHE_DIR=off \
    PIP_DISABLE_PIP_VERSION_CHECK=on \
    PIP_DEFAULT_TIMEOUT=100 \
    POETRY_VERSION=1.1.8 \
    POETRY_HOME="/opt/poetry" \
    POETRY_VIRTUALENVS_IN_PROJECT=true \
    POETRY_NO_INTERACTION=1 \
    PYSETUP_PATH="/opt/pysetup" \
    VENV_PATH="/opt/pysetup/.venv" 

ENV PATH="$POETRY_HOME/bin:$VENV_PATH/bin:$PATH"
COPY ./ssl/certificate.crt ./ssl/private.key /etc/ssl/

# IMAGE BUILD
FROM python-base as builder-base
RUN apt-get update && apt-get install --no-install-recommends curl build-essential -y && rm -rf /var/lib/apt/lists/*
COPY get-poetry.py ./
RUN python get-poetry.py
WORKDIR $PYSETUP_PATH
COPY poetry.lock pyproject.toml ./
RUN poetry install --no-dev

# IMAGE PRODUCTION
FROM python-base as production
ENV FASTAPI_ENV=production 
RUN groupadd -r appuser && useradd -r -u 1001 appuser -g appuser
COPY --from=builder-base $POETRY_HOME $POETRY_HOME
COPY --from=builder-base $PYSETUP_PATH $PYSETUP_PATH
WORKDIR /app
COPY app.py ./
USER appuser
EXPOSE 8000
ENTRYPOINT [ "uvicorn" ]
CMD ["app:app", "--host", "0.0.0.0", "--port", "8000", "--ssl-keyfile", "/etc/ssl/private.key", "--ssl-certfile", "/etc/ssl/certificate.crt"]

