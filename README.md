# Laboratorio: Microservicios con Spring Boot, RabbitMQ, Docker y Play With Docker

Este repositorio contiene el desarrollo del laboratorio propuesto para crear un sistema basado en eventos usando dos microservicios hechos en Spring Boot (productor y consumidor), que se comunican a través de RabbitMQ. Toda la arquitectura fue orquestada con Docker Compose y desplegada exitosamente en [Play With Docker](https://labs.play-with-docker.com/).

## Objetivo

Implementar un flujo de mensajes entre dos aplicaciones independientes usando RabbitMQ como broker de mensajería, demostrando comunicación asincrónica entre servicios y despliegue de microservicios con contenedores.

## Estructura del proyecto

```
event-driven-lab/
├── producer-service/         # Servicio Productor (Spring Boot)
├── consumer-service/         # Servicio Consumidor (Spring Boot)
├── .devcontainer/            # Configuración para Codespaces
├── docker-compose.yml        # Orquestación de los servicios
└── README.md
```

## Tecnologías usadas

- Java 17
- Spring Boot
- RabbitMQ
- Docker / Docker Compose
- GitHub Codespaces
- Play With Docker

## Descripción de los servicios

### Producer Service

Servicio con un endpoint REST (`/api/messages/send`) que recibe un mensaje por parámetro y lo envía a un exchange en RabbitMQ. Utiliza una `DirectExchange` y una `RoutingKey` configuradas vía `application.properties`.

La lógica de envío se implementó usando `RabbitTemplate`, y se puede probar vía `curl` o Postman.

### Consumer Service

Servicio que escucha en la cola definida y procesa cada mensaje recibido. Usa la anotación `@RabbitListener` para mantenerse a la espera de nuevos eventos.

En este laboratorio, el procesamiento consiste en registrar el mensaje en consola, simulando una acción básica de consumo.

## Dockerización y orquestación

Ambos servicios fueron dockerizados utilizando imágenes base ligeras de Java 17, y el archivo `docker-compose.yml` define la red de servicios:

- `rabbitmq`: Broker de mensajería con interfaz web habilitada en el puerto `15672`.
- `producer-service`: Expone el puerto `8080`.
- `consumer-service`: Se ejecuta en segundo plano, sin necesidad de exponer puertos.

RabbitMQ se configuró para ser accesible desde ambos microservicios a través del hostname `rabbitmq`.

## Pruebas realizadas

Después de desplegar el sistema en [Play With Docker](https://labs.play-with-docker.com/), se realizaron pruebas de envío y consumo de mensajes:

1. Se utilizó `curl` para enviar mensajes al productor:

   ```bash
   curl -X POST "http://localhost:8080/api/messages/send?message=HolaDesdePlayWithDocker"
   ```

2. Se verificó que el consumidor recibió y procesó correctamente el mensaje revisando los logs:

   ```bash
   docker-compose logs consumer-service
   ```

3. También se accedió a la interfaz de RabbitMQ para observar la actividad de la cola (`messages.queue`), validando visualmente el flujo de mensajes.

## Evidencia

Se tomaron capturas de:

- La instalación del contenedor con el proyecto.
![image](https://github.com/user-attachments/assets/23f6069e-846e-4dde-a214-db5b27e6eed5)

- El envío exitoso del mensaje vía `curl` desde el productor.
![image](https://github.com/user-attachments/assets/e4b523be-d82b-42d6-acd4-6675d65eddf7)
![image](https://github.com/user-attachments/assets/72cc07b5-3846-44c6-a9da-1ac4101c13cf)


- El log del consumidor procesando el mensaje.
![image](https://github.com/user-attachments/assets/6966d243-63e1-486d-9c04-106ed7dc7bbd)
![image](https://github.com/user-attachments/assets/69442a4d-0139-447b-954b-12ed50a28177)



- El estado de la cola en la UI de RabbitMQ.
![image](https://github.com/user-attachments/assets/1ea5589d-74d4-490c-af88-93bbf40864cb)





## Conclusiones

Este laboratorio permitió experimentar de forma práctica con conceptos clave de arquitectura basada en eventos, incluyendo:

- Comunicación asincrónica con RabbitMQ
- Separación de responsabilidades entre servicios
- Uso de Docker Compose para orquestación
- Despliegue rápido y pruebas en entornos en la nube como Play With Docker

El enfoque paso a paso facilitó entender cada componente del flujo y cómo se integran entre sí.


