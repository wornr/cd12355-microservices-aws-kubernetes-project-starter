FROM python:3.8-slim

RUN apt update -y

# Install a couple of packages to successfully install postgresql server locally
RUN apt install -y build-essential libpq-dev

# Update python modules to successfully build the required modules
RUN pip install --upgrade pip setuptools wheel

WORKDIR /app

COPY requirements.txt .
COPY *.py .

RUN pip install -r requirements.txt

ENV DB_USERNAME=admin
# probably not a good idea to pass password in here
ENV DB_PASSWORD=Passw0rd
ENV DB_HOST=127.0.0.1
ENV DB_PORT=5433
ENV DB_NAME=udacity

CMD ["python", "app.py"]