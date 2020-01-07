FROM alpine:latest
MAINTAINER sunlnx@gmail.com
RUN apk add python && apk add py-pip
COPY ./requirements.txt /app/requirements.txt
WORKDIR /app
RUN pip install -r requirements.txt
COPY . /app
ENTRYPOINT [ "python" ]
CMD [ "app.py" ]

