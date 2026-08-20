---
schema_version: "1.0.0"
document_id: "7f261253f96796edf37a97c1dc7aa8742deeeec503a070038845385636fe1f08"
company_key: "yc-truora"
company: "Truora"
source_id: "yc-truora-rss-676fa6acb2da"
canonical_url: "https://blog.truora.com/es/el-nuevo-kyc-en-argentina-cuentas-activas-en-3-minutos"
published_at: "2026-06-24T13:00:08+00:00"
first_seen_at: "2026-07-25T01:08:05.780615+00:00"
fetched_at: "2026-07-28T21:10:03.278263+00:00"
content_hash: "sha256:056f0dbdb5f673f2f6e41e37292cacb7410f61c8b642cc2c2f1e6ee737610cc3"
---

# El Nuevo KYC en Argentina: Cuentas Activas en 3 Minutos

El ecosistema de Proveedores de Servicios de Pago (PSP) y las billeteras virtuales en Argentina enfrenta un punto de inflexión. Con las últimas normativas del


**Banco Central de la República Argentina (BCRA)** , los requisitos de capital, estructura y debida diligencia se han endurecido drásticamente. En este entorno hiperregulado, las fintech se debaten entre dos prioridades críticas: blindar la seguridad para mitigar el fraude y eliminar la fricción en el registro para evitar el abandono de los usuarios.


La respuesta técnica a este dilema es el


**Onboarding Conversacional** . Hoy en día, es técnicamente viable ejecutar un proceso completo de


*Know Your Customer* (KYC o Conozca a su Cliente) directamente dentro de una conversación de WhatsApp, validando la identidad de un usuario en menos de 180 segundos.


¿Cómo se logra esto manteniendo el cumplimiento estricto ante las autoridades de control? A continuación, desglosamos la infraestructura técnica y el marco legal que lo hace posible.


La respuesta técnica a este dilema es el


## **El Marco Regulatorio: BCRA, UIF y la Validación Digital**


Para que un flujo de KYC remoto sea válido en Argentina, debe alinearse con las directrices de los principales organismos reguladores del país:


**Organismo Oficial**


**Rol en la Verificación de Identidad**


**Requisito Clave para Canales Digitales**


**BCRA**


*(Banco Central)*


Regula a las entidades financieras y PSPs (billeteras electrónicas).


Exige mecanismos de identificación no presencial seguros y trazables para la apertura de cuentas virtuales (CVU) o cajas de ahorro.


**UIF**


*(Unidad de Información Financiera)*


Previene el Lavado de Activos y la Financiación del Terrorismo (Ley N. 25.246).


Obliga a realizar una Debida Diligencia (CDD) exhaustiva antes de habilitar transacciones, requiriendo el cruce de datos contra listas PEP (Personas Expuestas Políticamente).


**RENAPER**


*(Registro Nacional de las Personas)*


Resguarda la base de datos de identidad y biometría oficial de la población.


Provee el


**SID (Sistema de Identidad Digital)** , permitiendo la confrontación biométrica facial en tiempo real con validez legal.


Tradicionalmente, cumplir con estas tres entidades implicaba que el usuario completara formularios extensos, descargara aplicaciones nativas pesadas o experimentara fallas al subir imágenes de su Documento Nacional de Identidad (DNI) en navegadores web móviles. El canal conversacional de WhatsApp unifica estas tres capas de control en un solo hilo de chat.


## **Arquitectura Técnica: El Flujo de 3 Minutos Paso a Paso**


El éxito de este proceso radica en el uso de APIs integradas de forma invisible dentro de un bot automatizado de WhatsApp. El flujo técnico opera bajo la siguiente secuencia automatizada:


### **1.Inicio y Captura de Datos:**


0 a 30 segundos, e


l usuario inicia el chat de forma voluntaria. El bot solicita los datos esenciales de manera amigable: Nombre completo,correo.


### **2. Captura de Documento y Validación de Identidad (DNI + RENAPER)**


Para asegurar la legitimidad del usuario antes de la verificación biométrica, el proceso se divide en tres pasos consecutivos dentro del flujo:


- **Paso A: Captura de DNI (Frente y Dorso):** El bot le solicita al usuario que tome una fotografía nítida del frente y del dorso de su Documento Nacional de Identidad (DNI) utilizando la cámara de su celular. El sistema aplica tecnología


**OCR (Optical Character Recognition)** para extraer de forma automática los datos textuales (nombre, apellido, número de documento) y el número de trámite.


- **Paso B: Validación con RENAPER:** Con el número de documento y el número de trámite obtenidos, el sistema realiza una consulta en tiempo real a la base de datos del


**Registro Nacional de las Personas (RENAPER)** . Esto permite comprobar de inmediato que el documento está vigente, no ha sido denunciado por robo o extravío, y que los datos corresponden a una persona real.


- **Paso C: Prueba de Vida (Liveness Detection):** Una vez confirmado el documento, el bot solicita una


*selfie* o un video corto en tiempo real. Los algoritmos de profundidad analizan la imagen para certificar que se trata de una persona viva y presente, cruzando finalmente los rasgos faciales de esta captura con la fotografía oficial registrada en el RENAPER para asegurar que el portador del DNI es realmente el dueño del documento.


**Tiempo estimado del proceso completo:** 60 a 120 segundos.


### **3. Prueba de Vida y Verificación Biométrica Final: 30 a 60 segundos.**


Una vez que el sistema confirma que el DNI es válido y vigente ante el RENAPER, se procede a verificar que la persona que está realizando el trámite es efectivamente la dueña de ese documento:


- **Captura en tiempo real:** El bot le solicita al usuario que se tome una selfie o un video corto directamente con la cámara de su celular dentro del chat.


- **Detección de vida (Liveness Detection):** El sistema aplica algoritmos avanzados de profundidad y movimiento para asegurar que se trata de una persona viva y presente en tiempo real, bloqueando cualquier intento de fraude por suplantación con fotografías impresas, pantallas o deepfakes.


- **Match Biométrico:** Finalmente, la tecnología confronta los rasgos faciales de esta selfie en vivo contra la foto del DNI que ya fue procesada y validada en el paso anterior, asegurando una coincidencia exacta de identidad.


### **4.Filtro AML y Cumplimiento UIF:**


120 a 150 segundos.


En paralelo al cruce biométrico, el sistema corre el número de DNI y CUIT/CUIL del usuario contra listas de control de lavado de activos, sanciones internacionales y listados de Personas Expuestas Políticamente (PEP). Esto garantiza que la fintech cumple de forma automatizada con los reportes obligatorios de la UIF.


### **5.Activación del CVU:**


150 a 180 segundos.


Una vez que el RENAPER aprueba la identidad y los filtros de la UIF dan luz verde, el sistema se conecta con el Core de la fintech para generar y activar la cuenta virtual de forma inmediata, notificando al usuario en el mismo chat de WhatsApp.


## **Impacto en el Negocio: ¿Por qué WhatsApp multiplica las conversiones?**


Cada segundo extra y cada pantalla adicional en un flujo de registro tradicional representa una caída potencial en tu tasa de conversión. Al migrar la verificación de identidad a un entorno familiar como WhatsApp, las fintech experimentan métricas drásticamente superiores:


- **Cero fricción de descarga:** El usuario no necesita liberar espacio en su dispositivo para descargar una aplicación nueva solo para registrarse.


- **Optimización del OCR:** La cámara integrada de WhatsApp facilita la captura de imágenes, disminuyendo drásticamente los rechazos por fotos borrosas, mala iluminación o reflejos que suelen trabar los motores de reconocimiento de caracteres tradicionales.


- **Estrategia Omnicanal:** Si un usuario interrumpe el proceso por falta de señal, el bot puede enviar un recordatorio automatizado horas más tarde en el mismo chat para reanudar el proceso exactamente donde quedó, rescatando leads que de otro modo se habrían perdido.


**Nota de Seguridad de Datos:** Todas las transmisiones de datos biométricos y biográficos a través de canales conversacionales se realizan bajo protocolos de cifrado de extremo a extremo, cumpliendo rigurosamente con la Ley de Protección de Datos Personales de Argentina (Ley N. 25.326).


Optimizar su onboarding digital puede incrementar la tasa de conversión en más de un 30%. Si está listo para habilitar canales conversacionales seguros y libres de fraude en Argentina,


[Agende una sesión con nuestros especialistas en cumplimiento B2B](https://www.truora.com/es/contacto-ventas)
