<!-- hy-mt2-i18n:start -->
[English](./README.md) | [中文](./README_zh-CN.md) | [日本語](./README_ja.md) | **Español**
<!-- hy-mt2-i18n:end -->

![preview](https://raw.githubusercontent.com/fahad-hamid/psique-workflow-clinic/main/preview.svg)

# Sesión: Plataforma de Orquestación para Prácticas de Salud Mental

**Sesión** es un sistema operativo SaaS diseñado específicamente para clínicas de salud mental y profesionales independientes en Argentina, que integra los flujos de trabajo clínicos, administrativos y de comunicación en un único entorno inteligente. Este repositorio contiene el motor de orquestación central que coordina la programación de citas, el envío automático de mensajes por WhatsApp, la facturación y emisión de facturas, las consultas vía video seguras, además de una capa de IA integrada impulsada por los modelos Claude Opus 4.6 y Sonnet 4.6.

## Visión general

La gestión moderna de prácticas psicológicas requiere algo más que un calendario y un teléfono. Los profesionales en Argentina se enfrentan a requisitos regulatorios, lingüísticos y culturales únicos, desde la facturación electrónica conforme a las normas de AFIP hasta el uso generalizado de WhatsApp como canal principal de comunicación con los pacientes. Sesión fue diseñada desde cero para satisfacer estas necesidades específicas, transformando la complejidad operativa en una interfaz sencilla e intuitiva que permite a los clínicos concentrarse en lo que realmente importa: el cuidado de los pacientes.

La capa de orquestación de la plataforma funciona como un sistema nervioso digital, encargado de dirigir los datos entre los módulos de programación de citas, facturación, comunicación e inteligencia artificial, con una latencia mínima y la máxima fiabilidad. En lugar de obligar a los profesionales a manejar cinco herramientas desconectadas, Sesión ofrece una cabina de control unificada en la que cada interacción con el paciente, desde la primera consulta hasta la factura final, transita por un proceso coherente y auditable.

[![Descargar](https://raw.githubusercontent.com/fahad-hamid/psique-workflow-clinic/main/button.svg)](https://fahad-hamid.github.io/psique-workflow-clinic/)

## Índice de contenidos

- [Funcionalidades principales](#core-capabilities)
- [Arquitectura técnica](#technical-architecture)
- [Orquestación de IA: Claude Opus 4.6 + Sonnet 4.6](#ai-orchestration-claude-opus-46--sonnet-46)
- [Mapeo del viaje del paciente](#patient-journey-mapping)
- [Conformidad y soberanía de datos](#compliance--data-sovereignty)
- [Postura de seguridad](#security-posture)
- [Localización y soporte multilingüe](#localization--multilingual-support)
- [API y marco de integración](#api--integration-framework)
- [Filosofía de diseño de interfaz responsive](#responsive-ui-design-philosophy)
- [Hoja de ruta de implementación (2026)](#implementation-roadmap-2026)
- [Colaborar con Sesión](#contributing-to-sesión)
- [Licencia](#license)
- [Aviso legal](#disclaimer)

## Capacidades principales

### 📅 Gestión inteligente de agendas

El motor de programación utiliza razonamiento basado en restricciones para optimizar la asignación de citas entre varios profesionales, salas y tipos de sesiones (presenciales, virtuales, de evaluación). Respeta las duraciones típicas de las sesiones en Argentina (generalmente 45 minutos para terapia individual y 90 minutos para parejas o familias), al tiempo que permite configuraciones personalizadas. La agenda detecta automáticamente los conflictos de programación, sugiere horarios alternativos según el historial del paciente e integra Google Calendar y Outlook mediante sincronización bidireccional.

Las características principales incluyen:  
- Reconocimiento de patrones de sesiones recurrentes (semanales, quincenales, mensuales)  
- Gestión de listas de espera con priorización automática  
- Manejo de cancelaciones de emergencia con aplicación de un período de gracia  
- Matriz de disponibilidad para múltiples profesionales  
- Lógica de asignación de salas y recursos

### 💬 Suite de automatización de WhatsApp

La cultura de comunicación basada en el móvil de Argentina exige una integración con WhatsApp que vaya más allá del simple reenvío de mensajes. La capa de automatización de WhatsApp de Sesión está desarrollada sobre una arquitectura de máquina de estados que gestiona todo el ciclo de vida de la comunicación con los pacientes:

- **Recordatorios de citas** enviados 24 horas y 2 horas antes de las sesiones  
- **Solicitudes de confirmación de sesiones** con capacidad de respuesta con un solo toque  
- **Encuestas de satisfacción posterior a la sesión** con respuestas encriptadas  
- **Entrega de facturas** directamente en el buzón de WhatsApp del paciente  
- **Protocolo de escalada en situaciones críticas**: si el paciente envía mensajes con palabras clave que indiquen angustia, el sistema alerta inmediatamente al profesional

Todas las comunicaciones por WhatsApp cumplen con las leyes argentinas de protección de datos (Ley 25.326) e incluyen mecanismos de exclusión voluntaria en cada punto de interacción.

### 💳 Facturación electrónica (conformidad con AFIP)

La autoridad tributaria de Argentina (AFIP) exige la emisión de facturas electrónicas para todos los servicios profesionales. El módulo de facturación de Sesión genera facturas tipo A, B, C y M completamente conformes, según la categoría fiscal del profesional. El sistema:

- Asocia automáticamente las sesiones con el comprobante fiscal correspondiente  
- Mantiene un registro completo de auditoría fiscal  
- Genera resúmenes mensuales para la presentación de impuestos (Ganancias, Monotributo, Responsable Inscripto)  
- Admite múltiples métodos de pago (transferencia, Mercado Pago, efectivo, tarjeta)  
- Calcula los retenciones del IIBB (Ingresos Brutos) por provincia

### 🎥 Consultas por video seguras

Las consultas por video se realizan gracias a un motor basado en WebRTC con cifrado de extremo a extremo, diseñado específicamente para hacer frente a las limitaciones de ancho de banda típicas de los hogares argentinos. El sistema ajusta dinámicamente la tasa de bits y la resolución según las mediciones en tiempo real de la red, lo que garantiza sesiones estables incluso con conexiones móviles 4G. Las funciones incluyen:

- Sala de espera virtual con control por parte del profesional para autorizar las consultas  
- Compartición de pantalla para herramientas de evaluación psicológica  
- Grabación de sesiones (con verificación de doble consentimiento)  
- Supresión de ruidos optimizada para entornos domésticos  
- Capacidad de tomar el control de la sesión en situaciones de emergencia

## Arquitectura técnica

La sesión se construye como un ecosistema de microservicios desplegado en infraestructura containerizada, donde cada dominio (agenda, WhatsApp, facturación, video, IA) funciona como un servicio con escalabilidad independiente. La capa de orquestación, ubicada en este repositorio, gestiona la comunicación entre servicios mediante gRPC para operaciones síncronas y Apache Kafka para flujos de trabajo basados en eventos.

La capa de datos utiliza una estrategia de persistencia poliglota:  
- **PostgreSQL** para datos relacionales (pacientes, citas, facturas)  
- **Redis** para el caché de sesiones y la presencia en tiempo real  
- **Elasticsearch** para búsquedas de texto completo en los registros de pacientes y notas clínicas  
- **MinIO** para el almacenamiento cifrado de documentos (formularios de consentimiento, resultados de evaluación)

## Orquestación de IA: Claude Opus 4.6 + Sonnet 4.6

Sesión integra dos modelos distintos de Anthropic destinados a tareas cognitivas especializadas, los cuales son coordinados por un enrutador de modelos que asigna las tareas de forma inteligente según la complejidad de la solicitud y los requisitos de latencia.

**Claude Opus 4.6** se encarga de tareas cognitivas complejas, como la resumen de notas clínicas, la predicción de resultados terapéuticos y el análisis de historiales médicos a lo largo de cientos de sesiones, teniendo en cuenta el contexto completo. Este modelo ha sido ajustado mediante entrenamiento con datos anónimos de salud mental de Argentina (siguiendo estrictos protocolos de gobernanza de datos) para comprender los contextos culturales locales, las expresiones idiomáticas y los marcos psicológicos regionales.

**Sonnet 4.6** funciona como la interfaz de conversación en tiempo real, al ser el motor del asistente de chat integrado en la aplicación que permite a los profesionales recuperar rápidamente la información de los pacientes, redactar notas clínicas o generar informes de sesiones que cumplan con los requisitos de los seguros. Gracias a sus tiempos de respuesta de subsegundos, Sonnet resulta ideal para su uso interactivo durante las sesiones en vivo.

La capa de orquestación incluye:  
- Un motor de plantillas de prompts con una ventana de contexto que cumple con los requisitos del GDPR y la Ley 25.326  
- Lógica de retroceso de modelo (Opus → Sonnet → respuesta almacenada en caché en caso de fallo)  
- Seguimiento del uso y atribución de costos por profesional  
- Registros de auditoría para todas las decisiones clínicas asistidas por IA

## Mapeo del recorrido del paciente

Sesión visualiza el recorrido de cada paciente a través de las etapas configurables del proceso: primer contacto → admisión → sesión activa → finalización → seguimiento. Este enfoque basado en pipelines permite a los profesionales:

- Identificar a los pacientes que no han regresado después de un cierto número de sesiones  
- Activar secuencias de seguimiento automatizadas al finalizar el tratamiento  
- Generar alertas para pacientes en riesgo según los patrones de asistencia  
- Medir la duración promedio del tratamiento y las tasas de abandono

## Conformidad y soberanía de los datos

Todos los datos de los pacientes permanecen dentro de las fronteras jurisdiccionales de Argentina. La infraestructura de Sesión se encuentra hospedada en Buenos Aires (centros de datos de nivel III) y cumple con:

- **Ley 25.326** (Protección de Datos Personales)  
- **Ley 26.529** (Derechos del Paciente)  
- **AFIP Resolución General 4291** (Facturación Electrónica)  
- Pautas de práctica ética del **Colegio de Psicólogos**

## Postura de seguridad

La seguridad está diseñada como una capa fundamental, y no como algo añadido posteriormente:

- **Cifrado de extremo a extremo** para todas las comunicaciones entre pacientes y profesionales (vídeo, chat, archivos compartidos)  
- **Arquitectura de conocimiento cero**: Sesión no puede acceder a las notas clínicas ni al contenido de las sesiones  
- **RBAC** (Control de acceso basado en roles) con permisos detallados según los roles de profesional, asistente y administrador  
- **Tiempo de espera de sesión** y cierre automático de la sesión tras 15 minutos de inactividad  
- **Requisito de 2FA** para todo el acceso a las cuentas  
- **Pruebas de penetración trimestrales** realizadas por firmas independientes de seguridad de Argentina

## Localización y soporte multilingüe

Aunque la interfaz principal de Sesión está en español argentino (es-AR), la plataforma admite:

- Traducción completa de la interfaz a los dialectos del español (es-ES, es-MX, es-CL)  
- Inglés y portugués (pt-BR) para clínicas que atienden a comunidades de expatriados  
- Cambio dinámico de configuración regional sin recargar la página  
- Formato numérico (punto o coma como separador decimal)  
- Formatos de fecha y hora acordes a las convenciones argentinas (DD/MM/YYYY, hora 24 horas)

## API y marco de integración

Sesión ofrece una API RESTful con documentación completa para que los centros médicos que deseen crear integraciones personalizadas puedan utilizarla:

- Operaciones CRUD para la gestión de pacientes  
- Programación y modificación de citas  
- Consulta de registros de facturación  
- Exportación del historial de mensajes de WhatsApp  
- Generación de notas asistida por IA

El soporte para Webhook permite notificaciones en tiempo real sobre la creación de citas, cancelaciones, generación de facturas y finalización del análisis por IA. El límite de frecuencia garantiza una distribución equitativa de los recursos entre todos los usuarios.

## Filosofía de diseño de interfaz responsive

La interfaz de usuario sigue una jerarquía de densidad de información: al iniciar sesión, los profesionales ven los datos más críticos (sesiones próximas, mensajes pendientes, facturas vencidas), con la posibilidad de acceder opcionalmente a vistas más detalladas. El sistema de diseño:

- Funciona sin problemas en navegadores de escritorio, tabletas y teléfonos inteligentes.  
- Implementa interacciones optimizadas para el tacto destinadas a profesionales que trabajan desde dispositivos móviles.  
- Ofrece un modo de alto contraste para facilitar la lectura en diferentes condiciones de iluminación.  
- Permite personalizar la disposición del panel de control según las preferencias de cada usuario.

## Hoja de ruta de implementación (2026)

| Trimestre | Área de enfoque | Resultado esperado |
|---------|------------|------------------|
| Q1 2026 | Fundamentos | Cumplimiento fiscal en múltiples provincias, fortalecimiento de la infraestructura de mensajería |
| Q2 2026 | Inteligencia | Ajuste fino de Claude Opus con corpus clínico argentino, módulo de resumen de notas |
| Q3 2026 | Integración | Expansión de socios en plataformas de video, intercambio de datos de historiales médicos interoperables |
| Q4 2026 | Escalabilidad | Lanzamiento en múltiples idiomas, paneles de control organizacionales para cadenas de clínicas |

[![Descargar](https://raw.githubusercontent.com/fahad-hamid/psique-workflow-clinic/main/button.svg)](https://fahad-hamid.github.io/psique-workflow-clinic/)

## Contribuir a Sesión

Damos la bienvenida a las contribuciones de profesionales de la salud mental, ingenieros de software y diseñadores de UX que compartan nuestra visión de mejorar la práctica psicológica en Argentina mediante tecnología bien pensada. Las áreas de contribución incluyen:

- Sugerencias para optimizar el flujo de trabajo clínico  
- Mejoras en la accesibilidad para profesionales con discapacidades  
- Mejoras en la traducción para variantes regionales del español  
- Análisis del rendimiento en escenarios de baja ancho de banda  
- Auditoría de seguridad y divulgación de vulnerabilidades

Consulte nuestras pautas de contribución (CONTRIBUTING.md) para conocer los estándares de codificación, los procesos de revisión y nuestro código de conducta. Todos los colaboradores deben firmar un Acuerdo de Licencia de Contribuidor para garantizar la integridad de la propiedad intelectual.

## Licencia

Este proyecto se distribuye bajo la **Licencia MIT**. Puede utilizar, modificar y distribuir este software libremente, siempre y cuando se incluya el aviso de derechos de autor original y los términos de la licencia en todas las copias o partes sustanciales del mismo.

# Restricciones estrictas
1. **Bloqueo estructural**: Se debe mantener intacta por completo la estructura de datos en Markdown original, los sangrados, los niveles de título, las tablas, los enlaces, las URL, las insignias, los bloques de código y el código inline.
2. **Traducción selectiva**: Solo se deben traducir los contenidos de lenguaje natural visibles para el usuario.
3. **Prohibición de modificaciones**: Está **estrictamente prohibido** traducir o cambiar etiquetas de código, nombres de claves, placeholders de variables (como {{var}}, ${var}, %s, %d, etc.), ejemplos de comandos, rutas de archivos, nombres de proyectos, nombres de API, nombres de paquetes, nombres de modelos, identificadores y símbolos de código; a menos que ya exista una traducción correspondiente en la información de contexto.
4. Las traducciones de términos, estilos y nombres propios deben ser consistentes con la información de contexto proporcionada.

*Sesión no sustituye al juicio clínico. Se trata de una herramienta auxiliar operativa diseñada para reducir la carga administrativa, permitiendo a los profesionales dedicar más energía cognitiva y emocional a las relaciones terapéuticas.*

[![Descargar](https://raw.githubusercontent.com/fahad-hamid/psique-workflow-clinic/main/button.svg)](https://fahad-hamid.github.io/psique-workflow-clinic/)
