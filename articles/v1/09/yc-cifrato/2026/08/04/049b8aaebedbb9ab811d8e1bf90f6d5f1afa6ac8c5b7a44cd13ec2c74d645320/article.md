---
schema_version: "1.0.0"
document_id: "049b8aaebedbb9ab811d8e1bf90f6d5f1afa6ac8c5b7a44cd13ec2c74d645320"
company_key: "yc-cifrato"
company: "Cifrato"
source_id: "yc-cifrato-news-import-c3bc34360ee9"
canonical_url: "https://cifrato.ai/blog/como-causar-factura-iva-retenciones-aplicadas"
published_at: "2026-08-19T15:30:00+00:00"
first_seen_at: "2026-08-20T02:27:55.573462+00:00"
fetched_at: "2026-08-20T02:27:56.824511+00:00"
content_hash: "sha256:da6f6dc971ec95f9a4d1d80bae424e50a7c45aca50865b1524573fdf5d4c2d46"
---

# Cómo causar una factura con IVA y retenciones aplicadas

Causar una factura con IVA y retenciones aplicadas significa registrar en un solo asiento contable tres movimientos distintos: el ingreso o el costo/gasto por el valor neto de la operación, el IVA generado o descontable, y las retenciones (renta, IVA o ICA) que el comprador practica al vendedor o que la empresa debe practicar como agente retenedor. Cada retención tiene una base de cálculo distinta y afecta cuentas diferentes según si la empresa está vendiendo o comprando. A continuación se explica el proceso completo, con tarifas y bases vigentes para 2026.


## Qué es una factura con IVA y retenciones aplicadas


Cuando una empresa vende un bien o presta un servicio gravado con IVA a un cliente que es agente de retención, la factura resultante puede combinar hasta tres impuestos distintos sobre una misma operación: el IVA generado por la venta, la retención en la fuente a título de renta que el cliente le practica al vendedor, y —en algunos casos— la retención en la fuente a título de IVA (reteIVA) y la retención de industria y comercio (reteICA). Cada uno de estos conceptos tiene una base de cálculo diferente y un tratamiento contable propio, por lo que causar correctamente la factura exige separar cada componente en la cuenta que le corresponde.


## Los tres tipos de retención que pueden aparecer en una factura


- **Retención en la fuente a título de renta (retefuente):** es un anticipo del impuesto de renta que el comprador descuenta al vendedor y consigna a la DIAN a nombre de este. Se calcula sobre el valor de la operación antes de IVA.
- **Retención en la fuente a título de IVA (reteIVA):** es un anticipo del IVA que el comprador descuenta al vendedor cuando actúa como agente de retención de IVA. Se calcula sobre el valor del IVA generado en la factura, no sobre el valor total de la operación.
- **Retención de industria y comercio (reteICA):** es un anticipo del impuesto municipal de industria y comercio, cuya tarifa y base la fija cada municipio o distrito dentro de los rangos que autoriza la ley, por lo que puede variar según dónde se realice la operación.


## Cómo se calculan el IVA y las retenciones sobre una misma factura


El orden de cálculo es clave para no cometer errores: primero se determina la base gravable de la operación (el valor antes de impuestos), luego se calcula el IVA sobre esa base, y finalmente se calculan las retenciones. La retefuente y la reteICA se calculan sobre la base gravable sin IVA; la reteIVA se calcula sobre el valor del IVA ya liquidado, no sobre el total de la factura ni sobre la base gravable.


Un error frecuente es aplicar la retención en la fuente sobre el valor total de la factura (base más IVA), lo que genera una retención más alta de la que corresponde legalmente.


## Bases y tarifas de retención en la fuente vigentes en 2026


Desde el 1 de julio de 2026, tras la reactivación de las bases mínimas del Decreto 572 de 2025 por decisión del Consejo de Estado, estas son las cifras que deben usarse:


Concepto Base mínima 2026 (UVT / pesos) Tarifa declarante Tarifa no declarante


Compras generales 10 UVT ($523.740) 2,5 % 3,5 %


Servicios generales 2 UVT ($104.748) 4 % 6 %


Honorarios y comisiones (persona jurídica) Sin cuantía mínima 11 % —


Honorarios y comisiones (persona natural) Sin cuantía mínima 11 % o 10 %, según el caso 10 %


Arrendamiento de bienes inmuebles 10 UVT ($523.740) 3,5 % 3,5 %


Arrendamiento de bienes muebles Sin cuantía mínima 4 % 4 %


Compras agrícolas o pecuarias sin procesar 92 UVT 1,5 % 1,5 %


La UVT vigente para el año gravable 2026 es de $52.374, fijada por la Resolución DIAN 000238 de 2025. Los porcentajes de honorarios varían según si la persona natural suscribió o no más de un contrato en el año, o si sus ingresos superan cierto monto; conviene validar el caso concreto antes de aplicar la tarifa.


Para la reteIVA, la tarifa general es del 15 % del valor del IVA generado en la factura, y sube al 100 % en operaciones especiales como pagos a personas sin residencia en el país por servicios digitales o electrónicos, o compra de chatarra y tabaco a proveedores específicos. La reteICA depende del acuerdo municipal aplicable en cada jurisdicción, por lo que su tarifa debe consultarse directamente en la administración tributaria local.


## Causación desde la perspectiva del vendedor: las retenciones que te aplican


Cuando la empresa emite la factura de venta y el cliente le practica retenciones, esos valores no se pierden: se registran como un activo (anticipo de impuestos) que la empresa podrá descontar en sus declaraciones de renta e IVA. Es decir, en la venta las retenciones son un derecho a favor de quien factura, no un gasto.


## Causación desde la perspectiva del comprador: las retenciones que tú practicas


Cuando la empresa recibe la factura de un proveedor y actúa como agente de retención, debe descontar el valor correspondiente del pago al proveedor y registrar un pasivo a favor de la DIAN por cada tipo de retención practicada, que se cancelará al presentar la declaración mensual de retención en la fuente (formulario 350).


## Cuentas PUC más usadas para causar facturas con IVA y retenciones


Código PUC Nombre de la cuenta Uso


1305 Clientes nacionales Cuenta por cobrar en la venta


135515 Anticipo de retención en la fuente por cobrar Cuando el cliente te practica retefuente en una venta


135517 Anticipo de reteIVA por cobrar Cuando el cliente te practica reteIVA en una venta


135518 Anticipo de reteICA por cobrar Cuando el cliente te practica reteICA en una venta


2408 IVA generado (venta) o IVA descontable (compra) Según el rol en la operación


236505 Retención en la fuente por pagar Cuando tú practicas retefuente en una compra


236801 Retención de IVA por pagar (reteIVA) Cuando tú practicas reteIVA en una compra


236715 Retención de ICA por pagar Cuando tú practicas reteICA en una compra


2205 Proveedores nacionales Cuenta por pagar en la compra


## Ejemplo de causación: factura de venta de servicios con retefuente, reteIVA y reteICA


Una empresa presta un servicio profesional por $5.000.000 más IVA del 19 %. El cliente es agente de retención y aplica retención en la fuente por honorarios del 11 %, reteIVA del 15 % sobre el IVA generado y reteICA del 0,7 % sobre la base gravable.


Cuenta Débito Crédito


1305 Clientes nacionales 5.257.500


135515 Anticipo de retención en la fuente por cobrar (11 % de $5.000.000) 550.000


135517 Anticipo de reteIVA por cobrar (15 % de $950.000) 142.500


135518 Anticipo de reteICA por cobrar (0,7 % de $5.000.000) 35.000


4115 Ingresos por servicios 5.000.000


2408 IVA generado (19 % de $5.000.000) 950.000


Total débitos: $5.985.000; total créditos: $5.950.000; la diferencia corresponde a que la reteICA se descuenta del pago pero no reduce el valor de la factura ante la DIAN, sino que se resta directamente del efectivo que recibirá la empresa. Al conciliar el pago, el valor neto que efectivamente ingresará a bancos será $5.950.000 menos la suma de las tres retenciones ($727.500), es decir, $5.222.500.


## Ejemplo de causación: factura de compra de servicios con retenciones practicadas


La misma operación, vista desde el comprador que recibe el servicio y actúa como agente de retención.


Cuenta Débito Crédito


5135 / 6135 Gastos o costos por servicios 5.000.000


2408 IVA descontable (19 % de $5.000.000) 950.000


236505 Retención en la fuente por pagar 550.000


236801 Retención de IVA por pagar 142.500


236715 Retención de ICA por pagar 35.000


2205 Proveedores nacionales 5.222.500


Aquí el pasivo con el proveedor (2205) ya refleja el valor neto que efectivamente se le pagará, una vez descontadas las tres retenciones practicadas.


## Casos especiales al causar facturas con IVA y retenciones


- **Operaciones con no responsables de IVA:** si el proveedor no es responsable de IVA, la factura no genera IVA descontable ni reteIVA, pero sí puede generar retefuente y reteICA según el concepto.
- **Servicios prestados por personas no residentes:** la reteIVA sube al 100 % del IVA generado, ya que el prestador extranjero no puede ser fiscalizado directamente por la DIAN.
- **Autorretención:** algunas empresas, en lugar de que les retengan, están obligadas o autorizadas a autorretenerse renta e ICA sobre sus propias ventas; en ese caso, el asiento de causación de la venta debe incluir la autorretención practicada por la misma empresa vendedora.
- **Contribuyentes del régimen simple de tributación:** en general no son sujetos de retención en la fuente a título de renta por parte de sus clientes (con algunas excepciones), aunque sí pueden generar IVA si son responsables de este impuesto.
- **Operaciones inferiores a la base mínima:** si el valor de la compra o el servicio no supera la base mínima en UVT, no debe practicarse retefuente ni reteICA, aunque sí puede generarse IVA si la operación está gravada.


## Errores comunes al causar facturas con IVA y retenciones


- Calcular la retención en la fuente sobre el valor total de la factura (base más IVA) en lugar de sobre la base gravable.
- Calcular la reteIVA sobre el valor de la operación en lugar de sobre el valor del IVA generado.
- Aplicar tarifas de no declarante a un proveedor que sí es declarante de renta, o viceversa, sin haber verificado su situación tributaria.
- No verificar si la operación supera la base mínima antes de aplicar una retención.
- Registrar las retenciones que el cliente te practica en una venta como un gasto, cuando en realidad son un activo (anticipo de impuestos).
- Confundir la retención en la fuente con la reteICA municipal, aplicando una sola tarifa genérica sin consultar el acuerdo del municipio correspondiente.
- No emitir o solicitar el certificado de retención correspondiente para poder soportar el anticipo de impuestos ante la DIAN.


## Preguntas frecuentes sobre la causación de facturas con IVA y retenciones


**¿La reteIVA se suma o se resta del valor de la factura?** Se resta, junto con la retefuente y la reteICA cuando apliquen, del valor que finalmente recibirá el vendedor; ninguna de las retenciones reduce el ingreso ni el IVA generado que se declaran ante la DIAN, solo afecta el flujo de caja.


**¿Todas las empresas deben practicar reteIVA?** No. Solo quienes califican como agentes de retención de IVA según el artículo 437-2 del Estatuto Tributario están obligados a practicarla; el resto de compradores, aunque sean responsables de IVA, no necesariamente son agentes de retención de este impuesto.


**¿Qué pasa si no se registra correctamente la retención que me practicaron?** Si el certificado de retención no coincide con lo registrado en la contabilidad, la empresa puede tener diferencias al momento de descontar esos anticipos en su declaración de renta o de IVA, lo que puede generar glosas o requerimientos de la DIAN.


## Referencias


- Estatuto Tributario, artículos 365 a 419 — Retención en la fuente a título de renta.
- Estatuto Tributario, artículo 437-2 — Agentes de retención del IVA.
- Decreto 1625 de 2016 (Único Reglamentario en Materia Tributaria), modificado por el Decreto 572 de 2025 — Bases mínimas y tarifas de retención en la fuente.
- Decreto 2650 de 1993 — Plan Único de Cuentas (PUC).
- Resolución DIAN 000238 de 2025 — Valor de la UVT para el año gravable 2026.
- Resolución Unificada DIAN 000227 de 2025 — Sistema de facturación electrónica.
- Portal oficial de la DIAN: dian.gov.co
