---
schema_version: "1.0.0"
document_id: "2ae0cfeb28c1029bfb09a4e0e4411807f0b27d6722fb897e55a40959f623bee2"
company_key: "yc-cifrato"
company: "Cifrato"
source_id: "yc-cifrato-news-import-c3bc34360ee9"
canonical_url: "https://cifrato.ai/blog/ejemplo-de-factura-de-ventas-estructura-y-campos-obligatorios"
published_at: "2026-07-22T01:50:00+00:00"
first_seen_at: "2026-07-26T05:04:08.979462+00:00"
fetched_at: "2026-07-28T21:20:14.720808+00:00"
content_hash: "sha256:001d3457f6291fb06c3dcbced9b8023c290129c8348e805b360b5ba1ef1f77ba"
---

# Ejemplo de factura de ventas: estructura y campos obligatorios

Una factura de venta en Colombia debe incluir, como mínimo, la denominación expresa de "factura de venta", los datos del vendedor y del comprador (nombre o razón social y NIT), un número consecutivo autorizado, la fecha de expedición, la descripción de lo vendido, el valor total de la operación y la discriminación del IVA, según el artículo 617 del Estatuto Tributario. Si es electrónica, además debe llevar el CUFE, el código QR y la firma digital del emisor. Más abajo tienes un ejemplo completo con cada campo explicado.


## Campos obligatorios según el artículo 617 del Estatuto Tributario


Estos requisitos aplican a **toda factura de venta** , sea electrónica, de talonario o por computador:


# Campo obligatorio Qué debe contener


1 Denominación expresa Debe decir literalmente "Factura de Venta"


2 Identificación del vendedor Nombre completo o razón social y NIT de quien vende o presta el servicio


3 Identificación del comprador Nombre completo o razón social y NIT del adquirente, junto con el IVA discriminado


4 Numeración consecutiva Un número que corresponda a un sistema de numeración consecutiva autorizado


5 Fecha de expedición Día en que se genera y entrega la factura


6 Descripción de bienes o servicios Específica o genérica de lo vendido o prestado


7 Valor total de la operación Suma final que debe pagar el comprador


8 Calidad de retenedor de IVA Si el vendedor es agente retenedor, debe indicarlo expresamente


9 Datos del proveedor tecnológico o impresor Nombre y NIT de quien imprimió o generó técnicamente el documento


## Campos adicionales exclusivos de la factura electrónica


Si tu empresa está obligada a facturar electrónicamente (la gran mayoría de responsables de IVA y muchas personas jurídicas), la Resolución 000165 de 2023 exige, además de lo anterior:


Campo adicional Para qué sirve


**CUFE** (Código Único de Factura Electrónica) Cadena de 40 caracteres, generada con algoritmo SHA-384, que identifica de forma única e irrepetible cada factura ante la DIAN


**Código QR** Enlace directo al catálogo de validación de la DIAN, donde cualquier persona puede verificar la autenticidad de la factura


**Firma digital del emisor** Certifica que el documento fue generado por el facturador autorizado y no ha sido alterado


**Representación gráfica (PDF)** Versión legible en PDF del archivo XML original que recibe el comprador


**Fecha y hora exacta de generación** No puede ser retroactiva; debe coincidir con el momento real de expedición


**Forma de pago** Contado o crédito, y en algunos casos el medio de pago utilizado


**Dato clave:** el documento legal que realmente existe ante la DIAN es el archivo XML bajo el estándar UBL 2.1; el PDF que recibe tu cliente es solo su "representación gráfica" para que sea legible por humanos.


## Ejemplo completo de una factura de venta


A continuación, un ejemplo con datos ficticios que ilustra cómo se ve una factura de venta electrónica con todos sus campos:


**Encabezado**


Campo Ejemplo


Denominación FACTURA ELECTRÓNICA DE VENTA


Prefijo y número FEV-1045


Fecha y hora de expedición 2026-07-15 14:32:10


Vendedor (razón social y NIT) Comercializadora Andina S.A.S. — NIT 900.123.456-7


Régimen del vendedor Responsable de IVA


Comprador (razón social y NIT) Juan Pérez Gómez — C.C. 1.020.345.678


Resolución de facturación DIAN Resolución 18764021234567 – rango autorizado 1 a 5.000


**Detalle de productos o servicios**


Descripción Cantidad Valor unitario Descuento IVA (19%) Valor total


Silla ergonómica de oficina 2 $450.000 $0 $171.000 $1.071.000


Escritorio en madera 120cm 1 $620.000 $20.000 $114.000 $714.000


**Totales y pie de página**


Campo Valor


Subtotal (antes de IVA) $1.050.000


IVA total (19%) $285.000


Valor total a pagar $1.785.000


Forma de pago Crédito a 30 días


CUFE e5bac48e354bc907bccff0ea7d45fbf784f0a8e7243b58337361e1fbd430489d


Código QR Enlace de verificación al catálogo de la DIAN


Proveedor tecnológico Nombre y NIT del proveedor autorizado


## Factura de talonario vs. factura electrónica vs. documento equivalente POS


No todos los negocios facturan de la misma forma. Estas son las tres modalidades vigentes en Colombia:


- **Factura electrónica de venta:** obligatoria para la mayoría de responsables de IVA y personas jurídicas. Se genera en XML, se valida por la DIAN en tiempo real y se entrega con CUFE, QR y firma digital.
- **Factura de talonario o por computador:** en desuso, reservada para casos muy puntuales donde no aplica la obligación electrónica; requiere numeración previamente autorizada por la DIAN.
- **Documento equivalente POS electrónico:** usado en ventas de bajo valor al consumidor final (por ejemplo, en tiendas o restaurantes); tiene requisitos más simples, pero sigue debiendo transmitirse a la DIAN.


## Qué pasa si a tu factura le falta un campo obligatorio


Omitir alguno de estos requisitos no es un simple detalle estético:


- **El comprador puede rechazarla legalmente** , lo que significa que no le sirve para soportar sus costos, gastos o IVA descontable.
- **No es válida para efectos tributarios** , así el bien o servicio sí se haya entregado realmente.
- **La DIAN puede sancionar al vendedor** , con multas que van desde la invalidez del documento hasta la suspensión de su autorización para facturar, si detecta incumplimientos reiterados.
- Si falta el CUFE, la numeración autorizada o la firma digital en una factura electrónica, el documento **ni siquiera llega a considerarse válido** ante el sistema de la DIAN.


## Preguntas frecuentes


**¿Una factura sin IVA discriminado es válida?** Solo si la operación está exenta o excluida de IVA. Si el bien o servicio es gravado, no discriminar el IVA hace inválida la factura para efectos tributarios.


**¿El PDF que recibo de mi proveedor es "la factura" oficialmente?** No. El documento legal es el archivo XML validado por la DIAN; el PDF es solo su representación gráfica para que las personas puedan leerlo fácilmente.


**¿Puedo diseñar el formato visual de mi factura como yo quiera?** Sí, siempre que incluya todos los campos obligatorios. El diseño, colores y logo son libres; lo que no es negociable es la información mínima exigida por la norma.


**¿Qué diferencia hay entre el CUFE y el código QR?** El CUFE es el identificador único de la factura ante la DIAN. El código QR es una representación visual que, al escanearla, lleva a esa misma información y permite verificarla en el catálogo oficial.


**¿Una cuenta de cobro es lo mismo que una factura de venta?** No. La cuenta de cobro la usan personas no obligadas a facturar (por ejemplo, algunos no responsables de IVA) y tiene requisitos mínimos distintos; no reemplaza a la factura de venta para quienes sí están obligados a expedirla.
