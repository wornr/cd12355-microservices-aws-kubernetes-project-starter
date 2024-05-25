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

CMD ["python", "app.py"]