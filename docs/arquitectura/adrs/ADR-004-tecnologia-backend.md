# ADR-004: Elección de Node.js y Express para el Desarrollo de la API Backend

## Estado
Aceptado

## Contexto
La API Backend del sistema actuará como el núcleo conector de la plataforma. Deberá procesar simultáneamente múltiples peticiones concurrentes provenientes de clientes consultando eventos, pasarelas de pago devolviendo confirmaciones, y personal de validación escaneando códigos QR de manera constante en los accesos físicos. El entorno tecnológico requiere manejar eficientemente un volumen elevado de operaciones de Entrada/Salida (I/O) de datos con baja latencia.

## Decisión
Se ha decidido construir la API REST utilizando **Node.js** como entorno de ejecución junto con el framework minimalista **Express.js**, asegurando la alineación con los contenedores definidos en el diagrama C2.

## Alternativas Evaluadas
* **Java con Spring Boot:** Se evaluó por su robustez, tipado estático estricto y alto rendimiento en arquitecturas empresariales. Fue **descartada** debido a que su configuración inicial requiere una mayor curva de aprendizaje y más tiempo de desarrollo (boilerplate code), lo que pondría en riesgo la agilidad del proyecto semestral.
* **Python con Django:** Se consideró por su velocidad de codificación y herramientas incorporadas. Sin embargo, fue **descartada** debido a que su modelo tradicional de hilos síncronos por defecto maneja de forma menos eficiente los picos masivos de solicitudes concurrentes de Entrada/Salida (E/S) si se compara con el bucle de eventos asíncrono y no bloqueante de Node.js.

## Consecuencias (Trade-offs)
* **Lo que ganamos (Eficiencia y Concurrencia):** Su arquitectura orientada a eventos permite procesar miles de conexiones simultáneas con un consumo mínimo de memoria, ideal para consultas constantes y validaciones en tiempo real. Esto optimiza el atributo de calidad de **Eficiencia de Rendimiento (Performance Efficiency)**.
* **Lo que sacrificamos (Robustez del Tipado Nativo):** Al trabajar con JavaScript (tipado dinámico y débil), se incrementa la probabilidad de introducir errores silenciosos en tiempo de ejecución. Esto nos obliga a implementar middlewares estrictos de validación de esquemas (como Joi o Zod) en las capas de entrada de datos para mitigar el riesgo.
