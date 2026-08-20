---
schema_version: "1.0.0"
document_id: "76df209bfa7ad093a8ae6133e168af9f3ba661121093c079034b83375610f9ce"
company_key: "yc-cardda"
company: "Cardda"
source_id: "yc-cardda-rss-045ead9ae99f"
canonical_url: "https://blog.cardda.com/automatizar-pagos-webpay-codigo-verificacion/"
published_at: "2026-08-19T19:10:20+00:00"
first_seen_at: "2026-08-19T19:11:28.152481+00:00"
fetched_at: "2026-08-19T19:11:30.593651+00:00"
content_hash: "sha256:88d62417428e8581c108579ae79b7c220d8f7a3110c37950c76a2ecbe8789e3a"
---

# Cómo automatizar los pagos por Webpay que exigen código de verificación

Una empresa puede tener el 95% de sus pagos automatizados y aun así necesitar a una persona sentada al lado de un teléfono. No para decidir el pago, no para llenar formularios, no para poner la tarjeta. Solo para leer seis dígitos que llegan por SMS y digitarlos antes de que expiren en la plataforma de pago.


Ese es el último paso manual de los pagos que se cobran por Webpay en Chile, y es el que decide si tu operación puede pagar sola o no. En este artículo explicamos por qué existe ese cuello de botella, cómo Cardda lo resuelve, y cómo[empresas chilenas ya lo están resolviendo](https://blog.cardda.com/como-afp-uno-puede-hacer-todos-sus-pagos-en-menos-tiempo-y-sin-errores-gracias-a-cardda/) .


## Qué significa “pagar con agentes”


Cada vez se habla más de agentes: software que ejecuta tareas de punta a punta en vez de solo sugerirlas. En pagos, un agente (o un robot, o un flujo automatizado) entra a un portal, llena el formulario del trámite, ingresa los datos de la tarjeta y cierra la compra. Todo eso ya es automatizable con la tecnología que las empresas tienen hoy.


El problema aparece en el único paso que no depende del portal ni del agente: la autenticación. Cuando pagas con tarjeta en Chile, el comercio puede exigir un factor extra de seguridad o autenticación reforzada (el estándar 3-D Secure), que la mayoría de las veces llega como un código de verificación por SMS. Ese código no vive en tu sistema. Llega a un número de teléfono, expira en minutos, y hay que ingresarlo para que el pago ocurra.


Ahí se rompe la automatización. Pagar con la tarjeta no era la parte manual. El código sí.


## El cuello de botella: el código que obliga a que alguien esté presente


Conviene definir bien qué es “manual” en un pago, porque no es lo que uno cree. Un pago con tarjeta en un portal (el Registro Civil, la Tesorería, un proveedor) tiene seis pasos:


Anatomía de un pago por Webpay


Tu software


Exige una persona


Tu software


1


Identificar el pago


2


Llenar el formulario


3


Ingresar los datos de la tarjeta


4


Recibir el código por SMS


5


Digitar el código antes de que expire


6


Registrar el pago en tu sistema


Cinco de esos seis los puede hacer tu software. Los pasos 4 y 5 no.


La definición útil entonces no es “tiempo de trabajo”, es tiempo de presencia. Digitar el código toma 30 o 60 segundos, pero bloquea a los otros cuatro eslabones: puedes tener el 95% del proceso automatizado y seguir necesitando a alguien al lado del teléfono de nueve a siete. El costo real no es el minuto que se digita. Es que no se puede pagar cuando esa persona no está.


Por eso este no es un problema de eficiencia que se resuelve con “hazlo más rápido”. Es un cuello de botella estructural: mientras el código dependa de un humano registrando un SMS, la operación tiene horario de oficina.


## Cómo lo resuelve Cardda


Cardda hace dos cosas. Te entrega[la tarjeta virtual](https://blog.cardda.com/mejora-las-finanzas-de-tu-empresa-con-tarjetas-virtuales/) con la que tu agente paga, y entrega el código de verificación de forma automática para que ese eslabón 4-5 deje de necesitar a una persona.


La tarjeta se configura una sola vez, no en cada pago: número, vencimiento y CVV quedan guardados en tu sistema y tu agente los rellena. Lo que Cardda automatiza en cada pago es la autenticación, no la orden de pago. Y lo hace por push: tú no nos preguntas nada, nosotros te entregamos el código apenas llega.


### El flujo en detalle es:


El viaje del código de verificación


0,0 s


dentro de Cardda: del mensaje recibido a tu webhook


### Tu agente, conectado al pago


Visto desde tu agente, la ecuación queda simple: la tarjeta se configura una vez y el código llega firmado a tu webhook en el momento exacto en que el portal lo pide. Con esas dos piezas puede orquestar el pago completo (entrar al portal, llenar el formulario, poner la tarjeta, recibir el código y cerrar la compra) sin que nadie tenga que estar mirando un teléfono.


Y no tiene que ser un desarrollo a medida. Un agente construido sobre Claude o Codex, conectado por API a tus sistemas, puede tomar ese flujo de punta a punta: decidir qué corresponde pagar según tus reglas, ejecutar el pago y dejarlo registrado donde lo necesites. Cardda aporta las dos piezas que hasta ahora no existían: una tarjeta virtual que tu agente puede usar y un código de verificación que llega por webhook, no por SMS al celular de alguien.


### El segundo factor no desaparece, solo cambia de manos


El código nunca fue algo que una persona *sabe* : es algo que llega a una línea que le pertenece al titular de la tarjeta. Con Cardda esa línea es la que el titular tiene registrada ante su emisor, y el código viaja hasta tu sistema por un canal cifrado y firmado, en vez de quedar en la pantalla de un teléfono sobre un escritorio.


Vale la pena comparar contra lo que pasa hoy en el flujo manual: los datos de la tarjeta viven en una planilla compartida, el código se dicta en voz alta o se manda por chat, el teléfono lo levanta cualquiera y no queda registro de nada. Justamente automatizar hace auditable por primera vez este flujo, manteniendo el control.


**Sobre la regulación.** En Chile la autenticación reforzada la regula la NCG 538 de la CMF, que crea la Autenticación Reforzada de Clientes (ARC) y la exige en transferencias de fondos y en el acceso y cambio de datos. Las compras con tarjeta en comercio electrónico no están en esa lista: el código de Webpay es un control que el emisor aplica por su cuenta. Automatizar su entrega no elimina el factor, pero una vez ingresado el código la operación se presume autorizada por el titular, así que el control se traslada a donde sí lo puedes gobernar: qué tarjeta, con qué tope y para qué proveedor.


### ¿Y si me roban la tarjeta?


Es la pregunta que aparece apenas uno entiende el flujo, y la respuesta es corta: quien se roba la tarjeta no se roba el código y tampoco puede cambiar el número asociado como autenticador.


El código no viaja hacia quien tiene los datos de la tarjeta, viaja hacia el único destino que configuraste antes en Cardda. Alguien con número, vencimiento y CVV puede intentar la compra, y el portal le va a pedir el código igual que siempre. Ese código llega a tu sistema, no a él. Y un código que nadie estaba esperando pasa a ser una alerta dentro de tu sistema, algo que el flujo manual nunca te dio.


## Antes y después


Indicador Antes (flujo manual) Después (con Cardda)


Pasos que exigen una persona presente 2 de 6 (recibir y digitar el código) 0


Horario en que se puede pagar Solo cuando hay alguien mirando el teléfono 24/7


, incluida la madrugada


Tiempo entre que llega el código y queda en tu sistema Depende de la atención de la persona 2,5 s


de mediana


Pagos desatendidos posibles Ninguno Todos


Trazabilidad Un SMS en un teléfono Registro firmado y cifrado por pago


Radio de daño si algo sale mal La tarjeta física o de la empresa Una tarjeta virtual dedicada, con tope configurable y revocable


Desliza para ver la comparación completa


El punto que ordena la tabla: sin Cardda, el techo de tu automatización lo pone la disponibilidad de una persona. Con Cardda, ese techo desaparece.


## Dónde esto brilla


No todas las empresas viven el mismo dolor, y vale la pena identificar cuál es el de tu empresa.


**Esta herramienta es para ti si el pago es parte de tu producto** , no un gasto interno: si para completar cada venta que cierras necesitas pagar en un portal web que exige un código SMS de verificación. Si nadie digita ese código, no entregas tu servicio. Y como el pago escala con tus ventas, el cuello de botella crece contigo. Es el caso de quien le paga al Registro Civil, a municipalidades, a la Tesorería o a proveedores que cobran vía Webpay.


## Cómo funcionó en una empresa chilena


En una empresa chilena que, por cada venta que cierra, tiene que pagarle a un organismo público en su portal, el código empezó a llegar por webhook y el volumen pasó de un puñado de pagos al mes a más de 300. Entre el 24 de marzo y el 6 de agosto de 2026 fueron 1.090 pagos aprobados, CLP 3.890 millones movidos y una tasa de aprobación de 99,5%, con solo 5 rechazos. El dato que mejor muestra el punto de fondo es otro: 3 de cada 10 pagos ocurrieron fuera del horario hábil, y 39 cayeron entre medianoche y las cinco de la mañana. En esos cuatro meses la operación dejó de tener horario.


Un cliente · tarjeta automatizada · 24-mar a 6-ago 2026


1.090


pagos aprobados


$3.890M


CLP movidos


99,5%


tasa de aprobación · 5 rechazos


30,0%


de los pagos ocurrió fuera del horario hábil


Pagos por hora del día · hora de Chile


Pasa el cursor o haz clic en una hora


00


06


12


18


23


Horario hábil, de 09 a 19


Fuera de horario


El peak es a las **18:00** , justo cuando cierra la oficina. Entre las 05:00 y las 07:00 no hubo un solo pago. Y **39** cayeron entre medianoche y las cinco de la mañana: 39 veces en que alguien habría tenido que levantarse.


## Cómo empezar


Si reconociste tu operación acá, el camino es corto. Primero identifica el pago que hoy te obliga a tener a alguien disponible: normalmente es uno solo, y normalmente es el que más crece con tus ventas. Después emite una tarjeta virtual dedicada a ese flujo, con un tope acorde a lo que pagas ahí, y configura el webhook donde quieres recibir el código. De ahí en adelante el trabajo lo hace tu agente.


¿Quieres ver si tu operación tiene este cuello de botella? Conversemos y lo revisamos con tus propios números.


[Agendar Demo](https://www.cardda.com/agendar?utm_source=blog&utm_medium=organic&utm_campaign=blog-pagos-agentes)
