---
schema_version: "1.0.0"
document_id: "1b4f01e0736c884e01fe457fd4a54e1cb2d36e8021bd4d5a07fe08dd47ec0622"
company_key: "yc-cifrato"
company: "Cifrato"
source_id: "yc-cifrato-news-import-c3bc34360ee9"
canonical_url: "https://cifrato.ai/blog/que-es-el-nit-en-colombia"
published_at: "2026-07-12T20:47:00+00:00"
first_seen_at: "2026-07-21T13:28:50.703940+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:72083e43fe47348bfc569804bad15f5c10eea82a6ff779c0793472e9514d97ce"
---

# ¿Qué es el NIT en Colombia? Para qué sirve

Si alguna vez has facturado, abierto una cuenta bancaria empresarial o participado en una licitación en Colombia, seguramente te han pedido el NIT. Es uno de esos números que todo el mundo usa, pero pocos saben explicar con precisión cómo se construye, quién lo asigna y por qué el último dígito, separado por un guion, es tan importante como los nueve anteriores.


Esta guía cubre la definición oficial, el marco legal que respalda al NIT, cómo se calcula realmente el dígito de verificación (con un ejemplo numérico completo, paso a paso), las diferencias según el tipo de contribuyente, y los errores que hacen que una factura electrónica sea rechazada por la DIAN.


## ¿Qué es el NIT?


El NIT (Número de Identificación Tributaria) es el número único que la **Dirección de Impuestos y Aduanas Nacionales (DIAN)** asigna a cada persona natural o jurídica cuando se inscribe en el[RUT (Registro Único Tributario)](https://cifrato.ai/consultar-rut) . Su función es permitir la identificación inequívoca del contribuyente para efectos tributarios, aduaneros y de control cambiario en Colombia.


Es importante entender que **el NIT y el RUT no son lo mismo, pero tampoco existen el uno sin el otro** :


- El **RUT** es el documento completo: contiene el nombre o razón social, la dirección, el código de actividad económica (CIIU), las responsabilidades tributarias (si es responsable de IVA, si declara renta, si pertenece al régimen SIMPLE, etc.).
- El **NIT** es el número que identifica a ese contribuyente dentro del RUT. Es personal e intransferible.


## Marco legal del NIT


El NIT no es un simple número administrativo: está respaldado por normativa específica que pocos artículos mencionan de forma completa:


- **Decreto 2788 de 2004:** reglamentó la organización del RUT tal como se conoce hoy y estableció que la inscripción tiene vigencia indefinida, sin necesidad de renovación periódica.
- **Orden Administrativa 4 de 1989 (DIAN):** define el algoritmo matemático oficial para calcular el dígito de verificación del NIT.
- **Decreto Único Reglamentario 1625 de 2016:** compila la reglamentación tributaria vigente, incluyendo los procedimientos de inscripción, actualización y cancelación del RUT.
- **Estatuto Tributario:** establece quiénes están obligados a inscribirse en el RUT y, por tanto, a tener NIT, así como las sanciones por no hacerlo o por reportar información incorrecta.


## Estructura del NIT: formato y dígito de verificación


El NIT colombiano sigue el formato:


```text
XXX.XXX.XXX - Y


```


Donde los primeros nueve dígitos son el número base y la **Y** , después del guion, es el **dígito de verificación (DV)** : un número calculado matemáticamente a partir de los nueve dígitos anteriores, cuya función es detectar errores de digitación. Si alguien transcribe mal un dígito del NIT en una factura o un contrato, el dígito de verificación no cuadra y el sistema —incluida la validación en tiempo real de la factura electrónica— lo rechaza.


### Cómo se calcula el dígito de verificación (ejemplo completo)


La mayoría de artículos sobre el NIT mencionan que existe un algoritmo, pero pocos muestran el cálculo completo. Así funciona, usando como ejemplo el NIT base` 900123456` :


**Paso 1:** Multiplica cada uno de los nueve dígitos por su factor correspondiente, en este orden fijo:


Dígito del NIT 9 0 0 1 2 3 4 5 6


Factor de multiplicación 41 37 29 23 19 17 13 7 3


Producto 369 0 0 23 38 51 52 35 18


**Paso 2:** Suma todos los productos.` 369 + 0 + 0 + 23 + 38 + 51 + 52 + 35 + 18 = 586`


**Paso 3:** Calcula el residuo de dividir esa suma entre 11.` 586 ÷ 11 = 53 con residuo 3`


**Paso 4:** Aplica la regla final:


- Si el residuo es 0 o 1, el dígito de verificación es igual al residuo.
- Si el residuo es 2 o mayor, el dígito de verificación es` 11 − residuo` .


Como el residuo fue 3:` 11 − 3 = 8`


**Resultado:** el NIT completo, con su dígito de verificación, queda como **900.123.456-8** .


Esta es la razón por la que nunca debes omitir el dígito de verificación al reportar un NIT en una factura, un contrato o un trámite: sin él, cualquier sistema de validación —empezando por el de facturación electrónica de la DIAN— puede rechazar el documento de forma automática.


## Cómo se asigna el NIT según el tipo de contribuyente


Otro punto que muchos artículos simplifican demasiado es que el NIT no se construye igual para todos. La siguiente tabla resume las variaciones según el tipo de persona:


Tipo de contribuyente Cómo se construye el NIT Dígitos base


Persona natural colombiana (mayor de edad) Número de cédula de ciudadanía + dígito de verificación calculado por la DIAN Variable (hasta 10 dígitos)


Persona jurídica (empresa) Asignado directamente por la DIAN al momento de la inscripción en el RUT 9 dígitos


Menores de edad colombianos (con obligaciones tributarias) Fecha de nacimiento + número de registro civil de nacimiento + dígito de verificación 11 dígitos


Extranjero con cédula de extranjería o pasaporte Número del documento de identificación extranjero registrado ante la DIAN + dígito de verificación Variable


Tercero del exterior sin NIT (para efectos de retención) La empresa compradora debe asumir un NIT genérico compuesto por seis "4" y tres dígitos consecutivos 9 dígitos


## ¿Quién está obligado a tener NIT?


Deben inscribirse en el RUT —y, por tanto, obtener NIT— entre otros:


- Personas naturales y jurídicas que realicen cualquier actividad económica gravada o no gravada en Colombia.
- Trabajadores independientes que facturen sus servicios de forma recurrente, aunque no tengan una empresa formalmente constituida.
- Importadores, exportadores y usuarios aduaneros.
- Responsables y no responsables del IVA que, por disposiciones especiales, estén obligados a expedir factura.
- Quienes decidan acogerse voluntariamente al Impuesto Unificado SIMPLE.
- Sujetos sin residencia en Colombia que presten servicios digitales gravados con IVA o con presencia económica significativa (PES) desde el exterior.
- Cualquier persona que quiera abrir una cuenta bancaria empresarial o solicitar un préstamo comercial en el país.


## Para qué sirve el NIT en la práctica


Más allá de la definición tributaria, el NIT cumple funciones muy concretas en el día a día de una empresa o profesional independiente:


- **Facturación electrónica:** toda factura electrónica en Colombia valida el NIT del emisor y el receptor en tiempo real contra la base de datos de la DIAN; si no coincide, el sistema la rechaza.
- **Apertura de cuentas bancarias y créditos:** los bancos exigen el NIT como parte de la identificación tributaria antes de vincular a una empresa o independiente.
- **Contratación pública y licitaciones:** sin NIT no es posible competir ni presentar propuestas en procesos de contratación estatal.
- **Cálculo de retenciones e impuestos:** el NIT es la base sobre la que se determinan retenciones en la fuente, IVA, impuesto de renta y aportes parafiscales.
- **Control fiscal y trazabilidad:** al aparecer en facturas, contratos y recibos, el NIT permite a la DIAN cruzar información y detectar inconsistencias entre lo declarado y lo facturado.
- **Validación comercial (debida diligencia B2B):** antes de contratar un proveedor, otorgar crédito a un cliente o firmar una alianza, verificar el NIT permite confirmar que la empresa existe legalmente y está activa.


## Cómo obtener el NIT: paso a paso


1. **Reúne los documentos base:** cédula de ciudadanía (persona natural) o documentos de constitución de la empresa (persona jurídica), junto con el código de actividad económica (CIIU) que corresponda.
2. **Inscríbete en el RUT** a través del portal de la DIAN o, si es tu primera inscripción como persona jurídica, con la intermediación de la Cámara de Comercio correspondiente.
3. **Diligencia la información de ubicación, actividad económica y responsabilidades tributarias** (si eres responsable de IVA, si perteneces al régimen SIMPLE, etc.).
4. **Espera la validación y asignación del NIT.** El sistema genera automáticamente el número y calcula el dígito de verificación.
5. **Descarga el certificado del RUT** , donde el NIT aparece en la parte superior del documento, generalmente en la casilla destinada al número de identificación.


Tanto la inscripción en el RUT como la asignación del NIT son trámites completamente gratuitos, y la vigencia de la inscripción es indefinida: no se exige renovarla periódicamente, aunque sí debe actualizarse cuando cambian los datos del contribuyente.


## Cómo y cuándo actualizar tu NIT o RUT


A diferencia de lo que muchos asumen, el NIT en sí mismo no cambia una vez asignado, pero el RUT sí debe actualizarse ante la DIAN en varios escenarios:


- Cambio de dirección o domicilio fiscal.
- Cambio del representante legal de la empresa.
- Cambio o ampliación del código de actividad económica (CIIU).
- Cambio en las responsabilidades tributarias (por ejemplo, pasar a ser responsable de IVA o inscribirse en el régimen SIMPLE).
- Disolución o liquidación de la empresa, que implica la cancelación del RUT y, con ella, la inactivación del NIT asociado.
- Fallecimiento del titular, en el caso de personas naturales.


No actualizar el RUT a tiempo puede generar sanciones y, en la práctica, inconsistencias que hacen que terceros rechacen facturas o contratos por datos desactualizados.


## Cómo consultar y verificar el NIT de un tercero


Antes de hacer negocios con una empresa, verificar su NIT es una forma sencilla de reducir riesgo comercial. Existen dos rutas gratuitas, según la información que ya tengas:


Si tienes... Usa este canal Para qué sirve


El nombre o razón social de la empresa RUES (Registro Único Empresarial y Social) Encontrar el NIT asociado a esa razón social y confirmar su registro mercantil


El número de NIT Consulta de NIT en el portal de la DIAN Confirmar a qué empresa corresponde y si está activa frente a la DIAN


Si el NIT que te entrega un proveedor o cliente no coincide con la razón social o el representante legal que dice tener, esa discrepancia es una señal de alerta que vale la pena investigar antes de continuar la relación comercial.


## Errores comunes relacionados con el NIT


- **Omitir el dígito de verificación** al reportar el NIT en una factura, cotización o contrato, lo que puede provocar el rechazo automático del documento.
- **Confundir NIT con RUT** , asumiendo que son el mismo documento cuando en realidad uno es el número y el otro es el registro completo.
- **No actualizar el RUT tras un cambio de representante legal o dirección** , generando inconsistencias frente a auditorías o procesos de contratación.
- **Aceptar un NIT sin verificarlo** antes de una transacción comercial relevante, especialmente en operaciones de crédito o alianzas de mayor exposición.
- **Asumir que el NIT tiene costo o vencimiento** , cuando en realidad tanto el trámite como la vigencia son gratuitos e indefinidos.


## Preguntas frecuentes


**¿El NIT y el RUT cuestan algo?** No. Tanto la inscripción en el RUT como la asignación del NIT son completamente gratuitas ante la DIAN.


**¿Una persona natural sin empresa necesita NIT?** Sí, si realiza actividades económicas de forma recurrente —vende productos, presta servicios, genera ingresos gravados—, aunque no tenga una empresa formalmente constituida.


**¿El NIT es información pública?** El número del NIT sí puede consultarse públicamente para efectos tributarios y comerciales; la información detallada del RUT (como direcciones o datos de contacto) no es de acceso público general.


**¿Qué pasa si doy un NIT incorrecto en una factura?** La factura puede ser rechazada, tanto por el receptor como por el sistema de facturación electrónica de la DIAN, que valida el NIT en tiempo real contra su base de datos.


**¿El NIT cambia si me equivoco al escribirlo?** No; el NIT es fijo una vez asignado. Lo que puede fallar es la transcripción: por eso el dígito de verificación existe, precisamente para detectar ese tipo de errores antes de que generen un problema mayor.
