FROM python:3.11.14-alpine3.23

WORKDIR /app

COPY . .

RUN pip install --upgrade pip &&  pip install . && pip install ".[test]"

CMD [ "uvicorn", "src.main:app", "--port", "8000"]
