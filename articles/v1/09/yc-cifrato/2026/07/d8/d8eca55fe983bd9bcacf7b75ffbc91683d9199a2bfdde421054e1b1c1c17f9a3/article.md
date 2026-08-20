---
schema_version: "1.0.0"
document_id: "d8eca55fe983bd9bcacf7b75ffbc91683d9199a2bfdde421054e1b1c1c17f9a3"
company_key: "yc-cifrato"
company: "Cifrato"
source_id: "yc-cifrato-news-import-c3bc34360ee9"
canonical_url: "https://cifrato.ai/blog/nota-credito-en-facturacion-electronica-que-es-y-como-hacerla"
published_at: "2026-07-25T02:47:00+00:00"
first_seen_at: "2026-07-26T05:04:08.979462+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:28aceff8b4a3e00582d28c60424454416cb0da1379c3f0ad412de95e24f746fd"
---

# Nota crédito en facturación electrónica: qué es y cómo hacerla

Una **nota crédito electrónica** es el documento que disminuye o anula, total o parcialmente, el valor de una factura electrónica ya validada por la DIAN. Se usa cuando hay una devolución de producto, un descuento posterior, un error en cantidades o precios, o cuando es necesario anular por completo la factura. A diferencia de lo que muchos piensan, **una factura electrónica no se puede editar ni eliminar una vez validada** : la única forma legal de corregirla o dejarla sin efecto es emitiendo una nota crédito que la referencie.


Desde 2023 es obligatorio emitir la nota crédito de forma electrónica, con su propio código de validación ante la DIAN. En esta guía te explicamos qué es exactamente, en qué se diferencia de la nota débito, cuándo usar cada código de concepto, y el proceso paso a paso para emitirla correctamente y sin generar inconsistencias tributarias.


## ¿Qué es una nota crédito y para qué sirve?


La nota crédito es un documento electrónico que se deriva de una operación de venta previamente facturada. Su función es **ajustar hacia abajo** el valor de esa factura (o anularla del todo), dejando registro formal ante la DIAN y ante el cliente de por qué cambió el monto de la operación.


Se usa principalmente en estos casos:


- El cliente devuelve total o parcialmente los productos o rechaza el servicio.
- Se otorga un descuento comercial después de haber emitido la factura.
- Hay un error en cantidades, precios o ítems facturados.
- Es necesario anular por completo una factura ya validada (por ejemplo, si se facturó por error o se duplicó).
- Se aplica una rebaja total sobre el valor facturado.


Es importante tener claro qué **no** resuelve una nota crédito: si el error está en los datos del cliente (NIT, razón social), la corrección **no se hace con nota crédito** . Según criterio vigente de la DIAN, en ese caso se debe anular la factura original con nota crédito y emitir una factura completamente nueva con los datos correctos, ya que un error de identificación del adquirente no es un simple ajuste de valor.


## Nota crédito vs. nota débito: la diferencia clave


Ambos documentos corrigen facturas ya validadas, pero en direcciones opuestas:


Aspecto Nota crédito Nota débito


Efecto sobre el valor Disminuye o anula el valor de la factura Aumenta el valor de la factura


Casos típicos de uso Devoluciones, descuentos posteriores, anulaciones, errores que reducen el valor Intereses de mora, cobros adicionales no facturados, ajustes de precio al alza


Frecuencia de uso Alta (la mayoría de correcciones) Baja (casos puntuales de cobro adicional)


Identificador único CUDE propio, referenciando el CUFE de la factura original CUDE propio, referenciando el CUFE de la factura original


Validación ante la DIAN Obligatoria Obligatoria


## CUFE y CUDE: por qué la referencia correcta es obligatoria


Toda factura electrónica validada por la DIAN recibe un **CUFE** (Código Único de Factura Electrónica). Cuando emites una nota crédito, esta genera su **propio CUDE** (Código Único de Documento Electrónico), pero debe **referenciar obligatoriamente el CUFE de la factura original** que está corrigiendo. Sin esa referencia, la DIAN puede rechazar la nota o generar inconsistencias que luego son difíciles de rastrear en tu contabilidad.


## Códigos de concepto de la nota crédito ante la DIAN


Cada nota crédito debe indicar un **código de concepto de corrección** , que explica el motivo del ajuste. Estos son los códigos vigentes según la normativa colombiana de facturación electrónica:


Código Concepto


1 Devolución de parte de los bienes; no aceptación de parte del servicio


2 Anulación de la factura electrónica


3 Rebaja total aplicada


4 Descuento total aplicado


5 Rescisión: nulidad por falta de requisitos


6 Otros


Elegir el código correcto no es un detalle menor: si usas el código 2 (anulación) es porque la operación comercial completa se está dejando sin efecto, y en muchos casos deberás volver a facturar si la venta realmente sí ocurrió. Usarlo quiere decir que ese número de factura queda anulado y no puede reutilizarse.


## Cómo hacer una nota crédito electrónica: paso a paso


1. **Ubica la factura electrónica original** que necesitas corregir, usando su número de consecutivo y su CUFE. Verifica que efectivamente haya sido validada por la DIAN (si nunca fue validada, el tratamiento es distinto y no requiere nota crédito).
2. **Confirma que el ajuste requiere nota crédito y no nota débito.** Si el valor debe bajar, anularse o se trata de una devolución, es nota crédito. Si el valor debe subir, es nota débito.
3. **Selecciona el código de concepto correcto** (devolución, anulación, rebaja, descuento, rescisión u otros) según la causa real del ajuste.
4. **Referencia el CUFE de la factura original** dentro del documento; este campo es obligatorio.
5. **Detalla los ítems o el valor afectado** : unidades, descripción, IVA (cuando aplique), valor unitario y valor total del ajuste.
6. **Verifica los datos del adquirente** (NIT, razón social, correo electrónico) directamente desde la factura original; recuerda que estos datos no se corrigen con nota crédito.
7. **Genera y firma el documento** desde tu software de facturación electrónica autorizado por la DIAN.
8. **Transmite la nota crédito a la DIAN** para su validación; recibirás un CUDE que la identifica de forma única.
9. **Envía el documento al cliente** (representación gráfica en PDF y archivo XML) y conserva el acuse de recibo.
10. **Registra el ajuste contablemente** , reflejando la devolución, el descuento o la anulación en tus libros y, si corresponde, en tu declaración de IVA del período.


## ¿Cuánto tiempo tienes para emitir una nota crédito?


La DIAN no establece un plazo único y obligatorio para emitirla, pero se recomienda hacerlo lo antes posible y, si es viable, **dentro del mismo período de la factura original** , para evitar inconsistencias entre lo declarado y lo realmente ajustado. Si el ajuste ocurre después de cerrado el período de IVA, tanto tu empresa como el cliente deberán reflejarlo en el período siguiente, lo que exige más comunicación y control interno.


## Errores comunes al emitir notas crédito (y cómo evitarlos)


- **No referenciar el CUFE de la factura original** : es un campo obligatorio; sin él, la nota puede ser rechazada o generar inconsistencias.
- **Usar nota crédito para corregir datos del cliente** : si el error es en el NIT, cédula o razón social del adquirente, la solución es anular la factura con nota crédito y emitir una completamente nueva, no "editar" con un ajuste de valor.
- **Elegir el código de concepto equivocado** : usar "anulación" cuando en realidad es una "rebaja parcial" (o viceversa) genera inconsistencias frente a auditorías y cruces de información de la DIAN.
- **Dejar pasar demasiado tiempo antes de emitirla** : retrasar la nota crédito varios períodos tributarios complica el ajuste de IVA tanto para tu empresa como para tu cliente.
- **Emitir una factura nueva en lugar de la nota crédito correspondiente** : genera duplicidad de documentos ante la DIAN y puede derivar en observaciones en auditorías.
- **No informar al cliente del ajuste** : aunque la nota se valide ante la DIAN, el cliente debe recibirla y, si ya declaró la factura original, ajustar su propia declaración de IVA.


No corregir estos errores a tiempo tiene consecuencias reales: la normativa vigente contempla sanciones para quienes no corrigen adecuadamente sus facturas electrónicas, y la DIAN puede incluso bloquear temporalmente la habilitación de facturación electrónica si detecta errores recurrentes.


## Preguntas frecuentes


**¿Puedo anular una factura sin nota crédito?** No. Una factura electrónica ya validada por la DIAN no se puede eliminar ni dejar sin efecto de otra forma; la nota crédito con código de anulación es el único mecanismo válido, y el número de esa factura anulada no podrá reutilizarse.


**¿La nota crédito reemplaza el proceso de declarar IVA?** No. La nota crédito ajusta el valor facturado, pero tanto tu empresa como el cliente deben reflejar ese ajuste en sus respectivas declaraciones de IVA del período correspondiente.


**¿Qué pasa si el cliente rechaza mi nota crédito?** El sistema de la DIAN permite que el receptor rechace un documento electrónico; si esto ocurre, debes revisar el motivo del rechazo y, de ser necesario, corregir y reenviar el documento.


**¿Toda nota crédito debe referenciar una factura?** En la mayoría de los casos sí, mediante el CUFE de la factura original. Existen escenarios donde una nota crédito puede transmitirse sin referenciar una factura específica, pero son la excepción, no la regla, y conviene validarlo con tu proveedor tecnológico o tu contador.


La nota crédito electrónica no es un simple trámite administrativo: es el mecanismo legal que te permite corregir, devolver o anular una venta ya facturada sin generar inconsistencias fiscales ni exponer a tu empresa a sanciones. La clave está en tres cosas: referenciar correctamente el CUFE original, elegir el código de concepto que refleje la realidad de la operación, y emitirla lo antes posible para mantener alineadas tu contabilidad y tus declaraciones de impuestos.


Si tu empresa maneja un volumen alto de devoluciones, descuentos o ajustes, automatizar la conciliación entre facturas y sus notas crédito asociadas —en lugar de rastrearlas manualmente— reduce el riesgo de errores contables y te da visibilidad real de tus ingresos netos en tiempo real.
