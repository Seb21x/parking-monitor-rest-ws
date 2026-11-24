# Parking Monitor System (WebSockets + REST API)

Network Applications Architecture (2026-2027) - Assignment 2

## System Architecture

The system consists of two independent Java applications deployed on separate Apache Tomcat 10.x instances:

1. **`parking-rest`**: RESTful Service (Jersey JAX-RS / Jakarta EE 10)
   - In-memory parking lot catalog and availability management.
   - Background thread updating availability randomly ($-5$ to $+5$ spaces) every 5 seconds.
   - API protection using a preconfigured HTTP header credential (`X-API-Key`).
   - Deployed on Apache Tomcat 10 (port `8080`).

2. **`parking-ws`**: WebSockets Web Application
   - Shared tracking dashboard for users connected to the same zone.
   - Periodic background polling of the REST service to synchronize catalog and availability.
   - Real-time updates pushed over WebSockets to all connected clients in the zone.
   - Deployed on Apache Tomcat 10 (port `8081`).

## Requirements
- Java 21 (JDK)
- Apache Maven 3.9+
- 2x Apache Tomcat 10.1.x instances

## Building the Projects
```bash
# Build REST service
cd parking-rest
mvn clean package

# Build WebSocket application
cd ../parking-ws
mvn clean package
```
The generated `.war` files in the `target/` directories can then be deployed to the `webapps/` folder of each respective Tomcat instance.
