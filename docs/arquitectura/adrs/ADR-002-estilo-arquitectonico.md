# ADR-002: Adopción de una Arquitectura Monolítica Modular para el Backend

## Estado
Aceptado

## Contexto
El sistema cuenta con seis módulos lógicos bien definidos (Usuarios, Eventos, Pagos, Validación de QR, Reportes y Notificaciones). Al tratarse de un proyecto académico que debe ser diseñado, implementado y desplegado en un periodo semestral por un equipo de desarrollo pequeño, es crucial balancear la capacidad de crecimiento futuro de la plataforma con la simplicidad operativa, los costos de infraestructura y la velocidad inicial de entrega.

## Decisión
Se ha decidido diseñar el backend utilizando un estilo de **Arquitectura Monolítica Modular**. Toda la lógica de negocio residirá en una única base de código estructurada internamente por componentes o módulos claramente delimitados, mapeados bajo el contenedor unificado de la API Backend en Structurizr.

## Alternativas Evaluadas
* **Arquitectura de Microservicios:** Se evaluó separar cada uno de los módulos en servicios de despliegue independientes (un microservicio para pagos, otro para validación, etc.), cada uno con su propia base de datos. Esta alternativa fue **descartada** debido a que introduce una altísima complejidad en la red (orquestación, latencia, manejo de fallos distribuidos) y exige una gestión de consistencia eventual que ralentizaría drásticamente el desarrollo y la entrega del proyecto en el plazo establecido.

## Consecuencias (Trade-offs)
* **Lo que ganamos (Simplicidad y Agilidad):** Centralizar el código facilita las pruebas globales, el despliegue en un único servidor y simplifica las transacciones directas en la base de datos. Esto maximiza directamente los atributos de calidad de **Mantenibilidad (Maintainability)** y la **Velocidad de Desarrollo (Time-to-market)**.
* **Lo que sacrificamos (Escalabilidad Independiente):** No es posible escalar de forma aislada un módulo crítico con picos de carga específicos (como el módulo de Validación el día del evento). Si la demanda aumenta en un punto, se debe replicar e instanciar el monolito completo en la infraestructura, consumiendo más recursos globales.
