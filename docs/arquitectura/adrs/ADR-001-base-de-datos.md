# ADR-001: Elección de PostgreSQL para garantizar la Consistencia de Datos

## Estado
Aceptado

## Contexto
[cite_start]El Sistema de Venta de Entradas busca digitalizar y centralizar el proceso de compra[cite: 9]. [cite_start]Una de las problemáticas más críticas identificadas es la "doble venta de entradas", lo que genera pérdida de confianza y conflictos en el acceso[cite: 16]. 
[cite_start]Para solucionar esto, el sistema requiere un bloqueo concurrente de los asientos al momento de que múltiples usuarios (compradores) intenten adquirir la misma entrada al mismo tiempo[cite: 33]. Por lo tanto, necesitamos un motor de almacenamiento que ofrezca un alto nivel de integridad referencial y soporte fuerte para transacciones ACID (Atomicidad, Consistencia, Aislamiento, Durabilidad).

## Decisión
Se ha decidido utilizar **PostgreSQL** como la base de datos principal del sistema para almacenar usuarios, eventos, entradas, transacciones y estados de validación.

## Alternativas Evaluadas
* **MongoDB (Base de datos NoSQL):** Se evaluó utilizar MongoDB por su flexibilidad de esquemas y alta velocidad de lectura/escritura (Rendimiento). Sin embargo, fue **descartada** debido a que, aunque soporta transacciones, su naturaleza distribuida y orientada a documentos hace que el bloqueo pesimista a nivel de registro (necesario para evitar la doble venta simultánea) sea más complejo de implementar y menos robusto de forma nativa frente a un motor relacional estricto.

## Consecuencias (Trade-offs)
* **Lo que ganamos (Consistencia y Seguridad):** Garantizamos que dos usuarios no puedan comprar la misma entrada al mismo tiempo. [cite_start]El sistema siempre reflejará el estado real del aforo[cite: 33]. Esto impacta directamente y de manera positiva en el atributo de calidad de **Fiabilidad (Reliability)** y **Consistencia de Datos**.
* [cite_start]**Lo que sacrificamos (Rendimiento):** El bloqueo de registros durante el procesamiento del pago puede generar cuellos de botella bajo una demanda extremadamente alta (ej. venta de entradas para un concierto muy esperado), haciendo el sistema ligeramente más lento[cite: 33]. La arquitectura de la base de datos es más rígida (esquemas relacionales).