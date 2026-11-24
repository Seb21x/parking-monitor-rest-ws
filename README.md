# Parking Monitor System (WebSockets + REST API)

Arquitecturas para Aplicaciones en Red (2026-2027) - Práctica 2

## Arquitectura del Proyecto

El sistema está compuesto por dos aplicaciones Java independientes desplegadas en instancias separadas de Apache Tomcat 10.x:

1. **`parking-rest`**: Servicio RESTful (Jersey JAX-RS / Jakarta EE 10)
   - Catálogo de aparcamientos y disponibilidad en memoria.
   - Hilo en segundo plano que actualiza la disponibilidad de cada aparcamiento de forma aleatoria ($[-5, +5]$ plazas) cada 5 segundos.
   - Seguridad mediante credencial preconfigurada en cabecera HTTP (`X-API-Key`).
   - Despliegue en Tomcat 10 (puerto `8080`).

2. **`parking-ws`**: Aplicación Web basada en WebSockets
   - Panel de seguimiento de aparcamientos compartido por los usuarios de una misma zona.
   - Consulta periódica al servicio REST para mantener los datos sincronizados.
   - Actualización en tiempo real vía canal WebSocket a todos los clientes conectados a una zona.
   - Despliegue en Tomcat 10 (puerto `8081`).

## Requisitos
- Java 21 (JDK)
- Apache Maven 3.9+
- 2x Apache Tomcat 10.1.x

## Compilación
```bash
# Compilar servicio REST
cd parking-rest
mvn clean package

# Compilar aplicación WebSocket
cd ../parking-ws
mvn clean package
```
Los ficheros `.war` generados en las carpetas `target/` se despliegan en las carpetas `webapps/` de cada Tomcat correspondiente.
