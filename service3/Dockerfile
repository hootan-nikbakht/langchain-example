FROM python:3.9-slim-buster

WORKDIR /app

COPY . /app

RUN pip install --no-cache-dir fastapi uvicorn redis requests openai langchain python-dotenv postgres psycopg2-binary pgvector tiktoken debugpy

# install postgresql client
RUN apt-get update && apt-get install -y postgresql-client && rm -rf /var/lib/apt/lists/*

# Copy wait-for-postgres.sh and make it executable
COPY ./wait-for-postgres.sh /wait-for-postgres.sh
RUN chmod +x /wait-for-postgres.sh

EXPOSE 80
EXPOSE 5678

# CMD ["/wait-for-postgres.sh", "postgres", "uvicorn", "app:app", "--host", "0.0.0.0", "--port", "80"]
# CMD ["sh", "-c", "python -m debugpy --listen 0.0.0.0:5678 -m uvicorn app:app"]
CMD ["/bin/sh", "-c", "/wait-for-postgres.sh postgres && python -m debugpy --wait-for-client --listen 0.0.0.0:5678 -m uvicorn app:app --host 0.0.0.0 --port 80"]
