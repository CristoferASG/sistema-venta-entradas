# ADR-005: Implementación de la Generación y Validación de QR como Módulo Interno Síncrono

## Estado
Aceptado

## Contexto
Cada entrada vendida debe contar con un identificador único representado en un código QR para su validación. El proceso clave ocurre durante el acceso físico al evento, donde el personal escanea el código y el sistema debe responder de inmediato para no ralentizar las filas de ingreso, asegurando al mismo tiempo que el QR sea auténtico y no haya sido utilizado previamente.

## Decisión
Se ha decidido desarrollar el `Servicio de QR` como un **Módulo Interno Síncrono** integrado dentro del mismo proceso del backend (Node.js) mediante librerías nativas del ecosistema, comunicándose directamente a través de llamadas de funciones internas en lugar de una API remota.

## Alternativas Evaluadas
* **Servicio Serverless Externo o API de Terceros (ej. AWS Lambda):** Se evaluó aislar la lógica de generación y renderizado de imágenes de los códigos QR en funciones independientes en la nube para descargar al servidor principal de tareas pesadas. Fue **descartada** debido a que añade un paso intermedio de red (latencia) en un proceso crítico de negocio y genera un costo económico variable y adicional por cada consulta o renderizado de imagen.

## Consecuencias (Trade-offs)
* **Lo que ganamos (Baja Latencia e Independencia):** La verificación de los códigos en las puertas del evento se realiza con la menor latencia de red posible al ocurrir de manera interna. Esto beneficia el atributo de calidad de **Tiempo de Respuesta (Response Time)** y **Disponibilidad (Availability)** local del acceso físico.
* **Lo que sacrificamos (Consumo de CPU en el Servidor Principal):** La generación matemática y renderizado de imágenes QR es una tarea intensiva en CPU. Si miles de usuarios deciden descargar su entrada en formato PDF/QR exactamente al mismo tiempo horas antes del evento, el módulo consumirá recursos del backend común, pudiendo ralentizar otros endpoints del sistema si no se añade una estrategia de caché en el almacenamiento.
