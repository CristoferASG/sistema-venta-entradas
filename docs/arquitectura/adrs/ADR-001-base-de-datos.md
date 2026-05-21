# ADR-001: Elección de PostgreSQL para garantizar la Consistencia de Datos

## Estado
Aceptado

## Contexto
El Sistema de Venta de Entradas busca digitalizar y centralizar el proceso de compra. Una de las problemáticas más críticas identificadas es la "doble venta de entradas", lo que genera pérdida de confianza por parte de los usuarios y serios conflictos logísticos en el acceso físico a los eventos. Para solucionar esto de raíz, el sistema requiere un mecanismo robusto de bloqueo concurrente de asientos en el preciso instante en que múltiples compradores intenten adquirir la misma entrada en simultáneo. Necesitamos un motor de almacenamiento transaccional que ofrezca un alto nivel de integridad referencial y soporte absoluto para propiedades ACID (Atomicidad, Consistencia, Aislamiento, Durabilidad).

## Decisión
Se ha decidido utilizar **PostgreSQL** como el motor de base de datos relacional principal del sistema. Este centralizará el almacenamiento de usuarios, eventos, entradas, transacciones financieras y estados de validación de los accesos.

## Alternativas Evaluadas
* **MongoDB (Base de datos NoSQL):** Se evaluó utilizar MongoDB debido a su flexibilidad de esquemas y su alta velocidad nativa para operaciones de lectura/escritura (Rendimiento). Sin embargo, fue **descartada** debido a que, aunque soporta transacciones multi-documento, su naturaleza distribuida y orientada a documentos hace que el control estricto de concurrencia y los bloqueos pesimistas a nivel de registro sean significativamente más complejos de implementar y menos seguros frente a un motor relacional tradicional ante picos masivos de transacciones.

## Consecuencias (Trade-offs)
* **Lo que ganamos (Consistencia y Seguridad):** Garantizamos que un asiento/entrada nunca pueda ser asignado a dos personas en el mismo instante. El sistema mantiene la verdad absoluta sobre el aforo. Esto impacta de forma directa y positiva en el atributo de calidad de **Fiabilidad (Reliability)** y **Consistencia de Datos**.
* **Lo que sacrificamos (Rendimiento bajo alta demanda):** El uso de bloqueos a nivel de fila durante el proceso transaccional del pago puede generar colas de espera y cuellos de botella temporales ante una demanda extremadamente alta (como la apertura de venta de un concierto masivo), sacrificando parcialmente la velocidad pura de respuesta por transacciones seguras.
