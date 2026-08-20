---
schema_version: "1.0.0"
document_id: "726a3215892d17d21d373448905dc7fe7e7598fca701e56bbfc6f92a62f5ebc7"
company_key: "yc-cifrato"
company: "Cifrato"
source_id: "yc-cifrato-news-import-c3bc34360ee9"
canonical_url: "https://cifrato.ai/blog/como-causar-factura-venta"
published_at: "2026-08-17T15:19:00+00:00"
first_seen_at: "2026-08-17T14:59:19.702533+00:00"
fetched_at: "2026-08-17T15:19:04.101579+00:00"
content_hash: "sha256:f4269cb84f8b8adf2544720f163c6c0abbc7944d6b8fd896004dbf4a29b128d7"
---

# Cómo causar una factura de venta correctamente

[Causar una factura](https://cifrato.ai/blog/como-causar-facturas-mas-rapido) de venta correctamente significa registrarla en la contabilidad en el momento en que ocurre el hecho económico —es decir, cuando se entrega el bien o se presta el servicio—, sin importar si el cliente ya pagó. El asiento debe reconocer el ingreso, la cuenta por cobrar y los impuestos generados (IVA, retenciones) usando las cuentas correctas del PUC, con la factura electrónica validada por la DIAN como soporte. A continuación se explica el proceso completo, con ejemplos y errores comunes.


## Qué significa causar una factura de venta


La causación es la aplicación del principio de devengo: los hechos económicos se reconocen cuando suceden, no cuando se recibe o se paga el dinero. Este principio está incorporado en el marco técnico normativo de información financiera que rige en Colombia (NIIF para grandes empresas, NIIF para pymes o el régimen simplificado según el grupo al que pertenezca la empresa) y tiene respaldo directo en el Estatuto Tributario.


Cuando una empresa vende un producto o presta un servicio, debe causar la factura tan pronto se configura la venta —así el cliente pague de contado, a crédito o en cuotas—. Esto permite que los estados financieros reflejen la realidad económica del negocio en el periodo correspondiente y que la información sea consistente con lo que se declara ante la DIAN.


## Facturar, causar y pagar: por qué no son lo mismo


Uno de los errores más frecuentes es confundir estos tres momentos, que en la práctica casi nunca coinciden:


Concepto Qué ocurre Momento típico


Facturar (expedir) Se genera el documento electrónico, se firma digitalmente y se envía a validación de la DIAN (obtiene CUFE) Al momento de la venta o entrega


Causar Se registra el asiento contable de partida doble: débito a cuentas por cobrar, crédito a ingreso e IVA generado El mismo día del hecho económico, aunque no se haya cobrado


Pagar / cobrar Ingresa o sale el efectivo, se afecta caja o bancos y se cancela la cuenta por cobrar Puede ser inmediato o varios días/meses después


Causar la factura en la fecha de pago en lugar de la fecha del hecho económico es, de hecho, uno de los errores contables más comunes y puede generar diferencias entre el libro contable y las declaraciones tributarias.


## Marco normativo que respalda la causación en Colombia


Antes de causar una factura de venta conviene tener claro qué exige la norma, porque de eso depende que el soporte sea válido y que el asiento sea auditable:


- El principio de devengo está establecido en el marco técnico normativo de información financiera (Decreto 2420 de 2015 y sus modificatorios) y reconocido por el Estatuto Tributario en materia de realización del ingreso para obligados a llevar contabilidad.
- La estructura de cuentas para registrar el asiento es la del Plan Único de Cuentas (PUC), regulado por el Decreto 2650 de 1993, todavía usado operativamente por la mayoría de empresas y firmas contables, aunque el reporte financiero se presente bajo NIIF.
- La factura electrónica de venta, que debe existir y estar validada por la DIAN antes de poder causarse como soporte fiscal y contable, se rige por la Resolución DIAN 000165 de 2023, modificada por la Resolución 000202 de 2025 y consolidada por la Resolución 000227 de 2025.
- La obligación de facturar y las consecuencias de no hacerlo están en los artículos 615, 616-1 y 652 del Estatuto Tributario.
- El deber de llevar contabilidad conforme a las normas de información financiera está en la Ley 1314 de 2009.


Estas normas pueden actualizarse; conviene verificar siempre la versión vigente en el sitio oficial de la DIAN antes de aplicar cambios en el software contable.


## Requisitos previos antes de causar una factura de venta


Un asiento de causación solo es válido si la factura que lo soporta cumple ciertas condiciones. Antes de registrar el asiento, verifica que la factura electrónica:


- Esté validada por la DIAN y tenga asignado el CUFE (Código Único de Facturación Electrónica).
- Incluya el NIT o documento y el nombre o razón social del comprador.
- Detalle correctamente los ítems, cantidades, valores unitarios, descuentos y la base gravable.
- Discrimine el IVA generado según la tarifa aplicable (0 %, 5 % o 19 %, según el bien o servicio).
- Indique si el cliente practicó alguna retención (retención en la fuente por renta, IVA o ICA), cuando aplique.
- Corresponda a la resolución de numeración vigente asignada por la DIAN a la empresa.


Si la factura no cumple estos requisitos, no debe causarse hasta corregirla, porque el soporte documental es la base para sustentar el ingreso, el IVA generado y las retenciones ante una eventual revisión de la DIAN.


## Paso a paso para causar una factura de venta


**Verifica la factura emitida.** Confirma que fue validada por la DIAN, tiene CUFE y los datos del cliente son correctos.


**Identifica la cuenta de ingreso.** Debe corresponder a la actividad económica registrada (venta de mercancías, prestación de servicios, ingresos por arrendamiento, etc.), según el código PUC que use la empresa.


**Calcula los impuestos asociados.** Determina el IVA generado sobre la base gravable y, si el cliente es agente retenedor, identifica el valor de la retención en la fuente, reteIVA o reteICA que te practicará.


**Registra el asiento de partida doble.** Débito a la cuenta por cobrar (cliente) por el valor total de la factura; crédito a la cuenta de ingreso por el valor neto y crédito al IVA generado por pagar. Si hay retenciones a favor del cliente, se debitan como un anticipo de impuestos.


**Concilia con el auxiliar del cliente.** Verifica que el saldo de la cuenta por cobrar quede correctamente identificado por tercero, para hacer seguimiento a la cartera.


**Archiva el soporte.** Conserva la factura electrónica (XML y representación gráfica) vinculada al asiento contable, cumpliendo el plazo de conservación documental exigido por la norma mercantil y tributaria.


## Cuentas PUC más usadas para causar una venta


Código PUC Nombre de la cuenta Naturaleza en el asiento


1305 Clientes nacionales Débito (cuenta por cobrar)


1355 Anticipos y avances recibidos Crédito, si el cliente pagó anticipo


1355 / 135515 Retención en la fuente por cobrar Débito, si el cliente retiene renta


1355 / 135520 Retención de IVA por cobrar (reteIVA) Débito, si el cliente es agente de retención de IVA


4135 Comercio al por mayor y al por menor Crédito (ingreso por venta de mercancía)


4155 / 4175 Servicios / actividades profesionales Crédito (ingreso por servicios)


2408 Impuesto sobre las ventas por pagar (IVA generado) Crédito


2367 ICA retenido a favor de terceros Referencia si el cliente practica reteICA


Los códigos exactos pueden variar según el catálogo particular de cada empresa, pero la lógica de débitos y créditos se mantiene igual bajo el PUC del Decreto 2650 de 1993.


## Ejemplos de asientos contables al causar una factura de venta


**Venta de contado, sin retenciones.** Una empresa vende mercancía por $1.000.000 más IVA del 19 %, y el cliente paga de inmediato.


Cuenta Débito Crédito


1105 Bancos 1.190.000


4135 Comercio al por mayor y al por menor 1.000.000


2408 IVA generado 190.000


**Venta a crédito 30 días, sin retenciones.** Mismo caso, pero el cliente paga después.


Cuenta Débito Crédito


1305 Clientes nacionales 1.190.000


4135 Comercio al por mayor y al por menor 1.000.000


2408 IVA generado 190.000


**Venta a crédito con retención en la fuente practicada por el cliente.** El cliente es agente retenedor y aplica una retención del 2,5 % sobre la base gravable de $1.000.000.


Cuenta Débito Crédito


1305 Clientes nacionales 1.165.000


135515 Retención en la fuente por cobrar 25.000


4135 Comercio al por mayor y al por menor 1.000.000


2408 IVA generado 190.000


En este caso, la cuenta por cobrar se reduce porque el cliente descuenta el valor retenido, pero ese valor no se pierde: queda registrado como un anticipo de impuesto que la empresa podrá descontar en su declaración de renta.


## Casos especiales al causar facturas de venta


- **Notas crédito y devoluciones.** Si el cliente devuelve mercancía o se anula parcialmente la venta, se causa una nota crédito que revierte el ingreso, el IVA generado y, proporcionalmente, la cuenta por cobrar.
- **Anticipos recibidos antes de facturar.** Cuando el cliente paga por adelantado, el dinero se registra primero como un pasivo (anticipo de clientes) y solo se traslada a ingreso cuando se causa la factura definitiva por la entrega del bien o servicio.
- **Ventas a plazos o por cuotas.** El ingreso se causa por el valor total en el momento de la entrega, aunque el cobro se difiera; no se causa por cada cuota cobrada.
- **Exportaciones y ventas exentas.** Se causan igual que una venta nacional, pero sin generar IVA o aplicando la tarifa del 0 % según el tratamiento tributario correspondiente al bien o servicio.
- **Facturas anuladas o rechazadas por el cliente.** No deben causarse; si ya se causaron, se debe reversar el asiento y anular el documento electrónico ante la DIAN según el procedimiento vigente.


## Errores más comunes al causar una factura de venta


- Causar la factura en la fecha del pago en lugar de la fecha real del hecho económico.
- Usar la cuenta de ingreso equivocada por no diferenciar entre venta de mercancía, servicios o arrendamientos.
- Olvidar registrar la retención que el cliente practicó, generando descuadres en el auxiliar del cliente.
- No conciliar el IVA generado en contabilidad con el IVA reportado en la factura electrónica validada.
- Causar facturas que aún no han sido validadas por la DIAN o que fueron rechazadas.
- No conservar el soporte documental (XML y representación gráfica) vinculado al asiento, dificultando auditorías posteriores.
- Duplicar asientos al recibir la misma factura por dos canales distintos (por ejemplo, integración automática y digitación manual).


## Buenas prácticas para mantener la causación bajo control


Cerrar los libros contables mensualmente y conciliar la causación contra los pagos efectivamente recibidos ayuda a detectar diferencias a tiempo. También es recomendable parametrizar en el software contable las cuentas PUC según el tipo de ingreso, para que cada factura de venta se clasifique de forma consistente sin depender de la digitación manual. Cuando el volumen de facturación es alto, conviene revisar periódicamente una muestra de asientos para verificar que la fecha de causación coincida con la fecha real de la operación y no con la fecha de registro en el sistema.


## Preguntas frecuentes sobre la causación de facturas de venta


**¿Se puede causar una factura de venta antes de que la DIAN la valide?** No. La factura electrónica solo tiene validez como soporte fiscal y contable una vez la DIAN la valida y le asigna el CUFE.


**¿Qué pasa si el cliente nunca paga la factura causada?** El ingreso y el IVA ya causados no se reversan por falta de pago; en su lugar, la cartera vencida se gestiona como cuenta por cobrar incobrable y, si corresponde, se provisiona conforme a la política contable de la empresa.


**¿La causación cambia si la empresa pertenece al régimen simple de tributación?** El registro contable sigue el mismo principio de devengo; lo que cambia es el tratamiento tributario del ingreso para efectos de la declaración anual consolidada, no el asiento de causación en sí.


## Referencias


- Decreto 2420 de 2015 y modificatorios — Marco Técnico Normativo de Información Financiera, Ministerio de Comercio, Industria y Turismo.
- Decreto 2650 de 1993 — Plan Único de Cuentas (PUC).
- Ley 1314 de 2009 — Regulación de los principios y normas de contabilidad e información financiera en Colombia.
- Estatuto Tributario, artículos 615, 616-1 y 652 — Obligación de facturar y sanciones.
- Resolución DIAN 000165 de 2023, modificada por la Resolución 000202 de 2025 y consolidada por la Resolución 000227 de 2025 — Sistema de Facturación Electrónica.
- Resolución DIAN 000238 de 2025 — Valor de la UVT para el año gravable 2026.
- Portal oficial de la DIAN: dian.gov.co
