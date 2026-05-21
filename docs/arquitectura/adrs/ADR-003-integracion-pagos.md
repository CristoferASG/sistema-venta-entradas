# ADR-003: Delegación del Procesamiento de Pagos a una Pasarela Externa

## Estado
Aceptado

## Contexto
El flujo principal de negocio del sistema depende de la compra de entradas mediante transacciones financieras en línea con tarjetas de crédito o débito. Almacenar, procesar o transmitir datos bancarios e información sensible de tarjetas dentro de nuestros propios servidores propios exige el cumplimiento de auditorías legales de seguridad extremadamente costosas, complejas y estrictas a nivel internacional.

## Decisión
Se ha decidido integrar una **Pasarela de Pago Externa** (como Stripe o PayPal, tal como se definió en el diagrama de contexto de Structurizr), delegando por completo el flujo transaccional monetario, la captura de datos sensibles y la validación bancaria a este sistema externo.

## Alternativas Evaluadas
* **Procesamiento de Pagos Interno (In-House):** Se analizó la posibilidad de construir un módulo propio de cobros integrado de forma nativa en nuestra base de datos y backend. Esta alternativa fue **descartada** de inmediato por el altísimo riesgo de seguridad legal y técnico que representa para el proyecto, sumado a la imposibilidad de certificar la infraestructura bajo la normativa internacional PCI-DSS en el lapso del ciclo de desarrollo.

## Consecuencias (Trade-offs)
* **Lo que ganamos (Seguridad y Cumplimiento Normativo):** Se mitiga casi en su totalidad el riesgo de filtración de datos financieros de los clientes, delegando la responsabilidad legal al proveedor. Esto eleva drásticamente el atributo de calidad de **Seguridad (Security)**.
* **Lo que sacrificamos (Control de la Experiencia de Usuario y Disponibilidad):** Se pierde el control absoluto sobre la interfaz y el flujo exacto del pago, ya que el usuario debe interactuar con formularios embebidos o ser redirigido temporalmente al sitio web de un tercero. Además, el núcleo del negocio pasa a depender directamente de la **Disponibilidad (Availability)** y estabilidad operativa del proveedor externo.
