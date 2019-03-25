FROM python:3.7-alpine

RUN pip install simple-http-server

ADD . /src

RUN chmod 777 /src/RunWebApp

CMD ["/src/RunWebApp"]
