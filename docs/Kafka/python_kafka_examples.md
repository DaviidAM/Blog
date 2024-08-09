# Ejemplos con Python

## Requisitos previos

1. **Instalar Kafka:** Asegúrate de tener Kafka instalado y en funcionamiento en tu entorno.
2. **Instalar la librería confluent_kafka:** Puedes instalarla mediante pip:

```bash
pip install confluent_kafka
```

## Kafka producer

El productor envía mensajes a un topic específico en Kafka. A continuación, se muestra un ejemplo simple de cómo crear un productor en Python:

```py title="producer.py"
from confluent_kafka import Producer

# Configuración del productor
conf = {
    'bootstrap.servers': 'localhost:9092'  # Dirección del broker de Kafka
}

# Crear el productor
producer = Producer(conf)

# Función para confirmar la entrega de los mensajes
def delivery_report(err, msg):
    if err is not None:
        print(f'Error en la entrega: {err}')
    else:
        print(f'Mensaje entregado a {msg.topic()} [{msg.partition()}]')

# Enviar un mensaje
topic = 'mi_topic'
producer.produce(topic, key='mi_clave', value='Hola, Kafka!', callback=delivery_report)

# Esperar a que se envíen todos los mensajes
producer.flush()
```

Explicación:

- **bootstrap.servers**: Especifica la dirección del broker de Kafka.
- **producer.produce**: Envia un mensaje al topic especificado. La función delivery_report es una callback que maneja la confirmación de la entrega.
- **producer.flush**: Asegura que todos los mensajes se hayan enviado antes de cerrar el productor.

## Kafka consumer

El consumidor se suscribe a un topic y recibe mensajes. A continuación se muestra cómo crear un consumidor en Python:

```py title="consumer.py"
from confluent_kafka import Consumer, KafkaException

# Configuración del consumidor
conf = {
    'bootstrap.servers': 'localhost:9092',  # Dirección del broker de Kafka
    'group.id': 'mi_grupo',                 # ID del grupo de consumidores
    'auto.offset.reset': 'earliest'         # Leer mensajes desde el principio si no hay offset guardado
}

# Crear el consumidor
consumer = Consumer(conf)

# Suscribirse al topic
topic = 'mi_topic'
consumer.subscribe([topic])

# Leer mensajes
try:
    while True:
        msg = consumer.poll(timeout=1.0)
        if msg is None:
            continue
        if msg.error():
            raise KafkaException(msg.error())
        print(f'Recibido: {msg.value().decode("utf-8")} de {msg.topic()} [{msg.partition()}] en offset {msg.offset()}')

except KeyboardInterrupt:
    pass

finally:
    # Cerrar el consumidor
    consumer.close()
```

Explicación:

- **group.id**: Define el ID del grupo de consumidores, lo cual permite que múltiples consumidores compartan la carga.
- **auto.offset.reset**: Configura desde dónde comenzar a leer mensajes si no se ha guardado un offset previo.
- **consumer.poll**: Espera mensajes del topic y los procesa. Se usa un bucle para leer mensajes de manera continua.
