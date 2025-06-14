## Reporte Individual — Tarea 3: Diseño e Implementación de Arquitecturas de Microservicios

**Nombre: Angel Janvier Gonzalez Delgado**
**Materia:** Arquitectura de Software
**Profesor:** Julieta G. Rodríguez Ruiz
**Fecha de entrega:** Viernes 13 de Junio, 2025
**Repositorio del equipo: Tarea de Microservicios\docker-microservicios**

---

### 1) ¿Qué es un microservicio?

Un microservicio es una pieza de software pequeña y autónoma que tiene una responsabilidad bien definida dentro de un sistema más grande. Se comunica con otros microservicios a través de interfaces (APIs) y puede ser desarrollado, probado, desplegado y escalado de manera independiente. Esta forma de arquitectura facilita la evolución continua del sistema, ya que cada microservicio puede mejorarse o sustituirse sin afectar a los demás, siempre que se mantenga su contrato de comunicación. Esto lo hace ideal para sistemas que deben ser altamente escalables, mantenibles y adaptables a nuevas necesidades del negocio.

---

### 2) Las 9 características según Martin Fowler y James Lewis

| Característica                                     | Descripción                                                                                                                                                                              |
| --------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Componentization via Services**          | Se divide el sistema en múltiples servicios independientes y desplegables de forma separada, lo que mejora la mantenibilidad y la reutilización.                                        |
| **2. Organised around Business Capabilities** | Los equipos se estructuran en torno a funcionalidades de negocio completas (por ejemplo: pagos, notificación, gestión de clientes) en lugar de por capas técnicas (backend, frontend). |
| **3. Products not Projects**                  | El equipo no se limita a entregar un proyecto y olvidarse; mantiene y evoluciona el servicio como un producto a lo largo de su ciclo de vida.                                             |
| **4. Smart Endpoints and Dumb Pipes**         | Los microservicios realizan la lógica de negocio internamente (endpoints inteligentes) mientras que la comunicación entre ellos es ligera y sencilla (pipes simples, como HTTP/REST).   |
| **5. Decentralized Governance**               | Cada equipo tiene libertad de elegir las tecnologías y herramientas más adecuadas para su servicio, fomentando la innovación y la responsabilidad técnica.                            |
| **6. Decentralized Data Management**          | Cada servicio tiene su propia base de datos o almacenamiento, lo que evita cuellos de botella y dependencias excesivas entre servicios.                                                   |
| **7. Infrastructure Automation**              | Se automatizan tareas repetitivas como pruebas, despliegues y monitoreo usando herramientas de CI/CD y contenedores como Docker, facilitando la entrega continua.                         |
| **8. Design for Failure**                     | Los microservicios se diseñan para que si uno falla, el resto del sistema pueda seguir operando correctamente, usando patrones de tolerancia a fallos como circuit breakers.             |
| **9. Evolutionary Design**                    | La arquitectura permite que los servicios evolucionen de forma gradual y controlada, adaptándose a nuevas exigencias sin tener que reestructurar todo el sistema de golpe.               |

---

### 3) Diferencia entre microservicio y API

Una API es una interfaz de programación de aplicaciones: define  **cómo se comunican los sistemas entre sí** . Un microservicio, en cambio, es una aplicación con lógica de negocio, procesamiento de datos y persistencia, **que expone sus funcionalidades mediante una API** para que otros servicios o clientes puedan usarla. Es decir, toda API no es necesariamente un microservicio, pero todo microservicio expone una o varias APIs para interactuar con el resto del ecosistema.

---

### 4) ¿Para qué se usa el API Gateway y cuál es su ventaja?

El **API Gateway** es un componente esencial en arquitecturas de microservicios, ya que centraliza la entrada de todas las solicitudes externas al sistema. Proporciona múltiples funciones clave: autenticación, autorización, balanceo de carga, traducción de protocolos y monitoreo. Al actuar como puerta de enlace única, reduce la complejidad para el cliente, mejora la seguridad al exponer solo los endpoints necesarios y permite aplicar políticas de control de tráfico de forma uniforme. Esto facilita la escalabilidad y el mantenimiento de toda la arquitectura.

---

### 5) Tabla de responsabilidades de los elementos del sistema

| Elemento                       | Responsabilidad                                                                                                                                                                                                                      |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **API Gateway (Tyk)**    | Recibe todas las peticiones de los usuarios y las distribuye de forma segura y controlada a cada microservicio correspondiente.                                                                                                      |
| **Cliente (Flutter)**    | Interfaz amigable que permite a los usuarios finales interactuar con el sistema desde dispositivos móviles.                                                                                                                         |
| **Gestor de Clientes**   | Administra el registro y actualización de la información de los clientes asegurados, garantizando que los datos estén siempre correctos y disponibles.                                                                            |
| **Notificador**          | Envía mensajes de confirmación y actualización a los clientes, utilizando la API de Telegram como canal de mensajería confiable.                                                                                                 |
| **Pagos**                | Microservicio responsable de consultar, mostrar y simular los pagos realizados por los asegurados.                                                                                                                                   |
| **Reporteador**          | Genera documentos PDF de las pólizas de seguro, personalizando los datos para cada cliente y facilitando la distribución digital.                                                                                                  |
| **Simulador**            | Crea registros de pagos falsos para pruebas funcionales, sin necesidad de pagos reales.Simula los pagos realizados para pruebas.                                                                                                     |
| **payment-validator-ms** | Microservicio en Go que lee los registros de pagos, valida su estado y envía notificaciones y documentos por Telegram para completar el flujo de actualización de pólizas.Lee pagos, valida estado y envía póliza por Telegram. |

---

### 6) Análisis del sistema: requerimientos y soporte

| Requerimiento de arquitectura     | ¿Es soportado? | ¿Cómo es soportado?                                                         | ¿Contribuye al objetivo de negocio? |
| --------------------------------- | --------------- | ----------------------------------------------------------------------------- | ------------------------------------ |
| Gestión de clientes asegurados   | Sí             | Mediante el microservicio Gestor de Clientes y su base de datos dedicada.     | Sí                                  |
| Notificación de pago validado    | Sí             | Con payment-validator-ms, que revisa `payment_records.json` y usa Telegram. | Sí                                  |
| Envío de póliza por mensajería | Sí             | Con el Reporteador, que genera el PDF, y el Notificador, que lo envía.       | Sí                                  |
| Usar contenedores                 | Sí             | Con Docker y docker-compose para orquestar todos los servicios.               | Sí                                  |
| Soportar nuevas tecnologías      | Sí             | Se integra Go para explorar nuevos lenguajes junto a Python y Flutter.        | Sí                                  |

---

### 7) Atributos de calidad adicionales

| Atributo      | Definición                                                                    | ¿Es soportado? | ¿Cómo se soporta?                                                                                 |
| ------------- | ------------------------------------------------------------------------------ | --------------- | --------------------------------------------------------------------------------------------------- |
| Escalabilidad | Capacidad del sistema de manejar un aumento de carga sin degradar el servicio. | Sí             | Se pueden levantar múltiples instancias de microservicios y balancear tráfico con el API Gateway. |
| Seguridad     | Protección de la información y control de acceso.                            | Sí             | Se usa Tyk Gateway para autenticar peticiones y se configuran tokens para Telegram.                 |
| Auditabilidad | Capacidad de rastrear y registrar todas las acciones importantes.              | Sí             | Los logs de contenedores y registros de pagos permiten seguir la pista de todas las operaciones.    |
| Desempeño    | Tiempo de respuesta óptimo y uso eficiente de recursos.                       | Sí             | Cada servicio hace solo lo necesario, sin afectar a otros, y se aprovecha la concurrencia de Go.    |

---

### 8) Justificación de la decisión de diseño

El enfoque de microservicios se eligió porque resuelve los problemas de escalabilidad, flexibilidad y mantenibilidad del sistema original. Al dividir la lógica en componentes independientes, cada equipo puede trabajar en un microservicio sin interferir con otros. Además, si la base de clientes sigue creciendo, podemos escalar solo los servicios más demandados (por ejemplo, el validador de pagos) sin necesidad de redimensionar todo el sistema. Esto optimiza costos y recursos, alineándose con la meta de ofrecer un servicio rápido, confiable y automatizado.

---

### 9) Diagrama actualizado

https://www.canva.com/design/DAGqMWAgTE4/cMMkNGN0QuWLwIu1BcYwCw/edit?utm_content=DAGqMWAgTE4&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton

![[diagrama1.png]]

![[diagrama2.png]]

### 10) Repositorio

**URL del repositorio:** Tarea de Microservicios\docker-microservicios

---
