FROM python:3.9.6

WORKDIR /app

COPY requirements.txt ./

RUN pip install -r requirements.txt

COPY . .

EXPOSE 5000

ENTRYPOINT FLASK_APP=main.py flask run --host=0.0.0.0
