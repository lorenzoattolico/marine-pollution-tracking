FROM python:3.10-slim

WORKDIR /app

# Installa dipendenze di sistema
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    build-essential \
    curl \
    netcat-traditional \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Copia requirements e installa dipendenze
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copia codice sorgente
COPY main.py .
COPY locations.yml .

# Imposta variabili d'ambiente
# Modificato per essere compatibile con docker-compose
ENV KAFKA_BOOTSTRAP_SERVERS=kafka:9092
ENV POLL_INTERVAL=300
ENV PYTHONUNBUFFERED=1

# Crea script di avvio che attende Kafka
COPY wait-for-kafka.sh .
RUN chmod +x wait-for-kafka.sh

# Aggiornato il percorso Kafka per l'attesa
CMD ["./wait-for-kafka.sh", "kafka:9092", "python", "main.py"]