FROM python:3.11-slim AS base
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PIP_NO_CACHE_DIR=1
WORKDIR /app

FROM base AS builder
RUN python -m pip install --no-cache-dir wheel

COPY pyproject.toml ./
RUN python -m pip wheel --wheel-dir /wheels .
RUN python -m pip wheel --wheel-dir /wheels-test ".[test]"

FROM base AS prod
RUN adduser --disabled-password --gecos "" appuser

COPY --from=builder /wheels /wheels
RUN python -m pip install --no-index --find-links=/wheels KubSU && rm -rf /wheels

COPY src ./src

ENV PYTHONPATH=/app \
    ROOT_PATH="" \
    PORT=8000
EXPOSE 8000

USER appuser
CMD ["sh", "-c", "uvicorn src.main:app --host 0.0.0.0 --port ${PORT}"]
