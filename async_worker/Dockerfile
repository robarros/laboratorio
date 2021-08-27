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

# IMAGE BUILD
FROM python-base as builder-base
RUN apt-get update && apt-get install --no-install-recommends curl build-essential -y
RUN curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python
#RUN pip install -U faust
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
#USER appuser
EXPOSE 6066
ENTRYPOINT [ "faust" ]
CMD ["-A", "app", "worker", "-l", "info"] 
