FROM python:3.8.1-slim

ENV PYTHONUNBUFFERED 1

EXPOSE 8000
WORKDIR /usr/app


RUN apt-get update && \
    apt-get install -y --no-install-recommends netcat && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

COPY poetry.lock pyproject.toml ./

RUN printf '%s\n' \
       'POSTGRES_DB=rwdb' \
       'POSTGRES_PORT=5432' \
       'POSTGRES_USER=postgres' \
       'POSTGRES_PASSWORD=postgres' \
       > .env

RUN source .env

RUN docker run -d --name pgdb --rm \
       -e POSTGRES_USER="$POSTGRES_USER" \
       -e POSTGRES_PASSWORD="$POSTGRES_PASSWORD" \
       -e POSTGRES_DB="$POSTGRES_DB" \
       postgres

RUN echo "POSTGRES_HOST=$(docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' pgdb)" >> .env

RUN pip install poetry==1.1
RUN poetry config virtualenvs.in-project false
RUN poetry install --no-dev

RUN source .env

RUN echo DB_CONNECTION=postgresql://$POSTGRES_USER:$POSTGRES_PASSWORD@$POSTGRES_HOST:$POSTGRES_PORT/$POSTGRES_DB >> .env

RUN echo SECRET_KEY=$(openssl rand -hex 32) >> .env


COPY . ./

ENTRYPOINT ["bash", "entrypoint.sh"]
