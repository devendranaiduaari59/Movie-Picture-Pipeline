
FROM public.ecr.aws/docker/library/python:3.10-alpine

ENV FLASK_RUN_HOST=0.0.0.0

RUN adduser -u 1000 -D app

RUN apk update && apk add --no-cache \
    pcre \
    pcre-dev \
    build-base \
    gcc \
    linux-headers \
    openssl \
    libffi-dev \
    && pip install pipenv

WORKDIR /app

COPY Pipfile Pipfile.lock ./

RUN ls -la /app && pipenv install --system --deploy

COPY . .

RUN mkdir -p /var/www/app && chown -R app:app /app /var/www/app

EXPOSE 5000

CMD ["uwsgi", "--http", ":5000", "--master", "--enable-threads", "-s", "/var/www/app/app.sock", "--manage-script-name", "--mount", "/=app:app"]
