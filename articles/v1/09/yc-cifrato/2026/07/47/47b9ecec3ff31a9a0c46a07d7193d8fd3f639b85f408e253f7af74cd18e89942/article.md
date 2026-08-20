---
schema_version: "1.0.0"
document_id: "47b9ecec3ff31a9a0c46a07d7193d8fd3f639b85f408e253f7af74cd18e89942"
company_key: "yc-cifrato"
company: "Cifrato"
source_id: "yc-cifrato-news-import-c3bc34360ee9"
canonical_url: "https://cifrato.ai/blog/conciliacion-bancaria"
published_at: "2026-07-11T20:24:00+00:00"
first_seen_at: "2026-07-24T01:30:37.138560+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:be7f55f2721bf50242c24eea473e0af746bf85ab5b3c842fc964651138173f9f"
---

# Conciliación bancaria: qué es y cómo hacerla

Cada mes llega el mismo momento de verdad: el saldo que muestra tu software contable no coincide con el saldo que reporta el banco. La conciliación bancaria es el proceso que existe justamente para resolver esa diferencia, entender su origen y dejar los libros de la empresa reflejando la realidad.


Esta guía va más allá de la definición básica que encuentras en la mayoría de blogs: incluye el formato exacto de conciliación con cifras en pesos colombianos, los asientos contables de ajuste, los códigos PUC involucrados, cómo afecta el GMF (4x1000) a tu conciliación, y los errores más costosos que cometen los auxiliares contables en Colombia.


## ¿Qué es la conciliación bancaria?


La conciliación bancaria es el procedimiento contable mediante el cual se comparan los movimientos registrados en el libro auxiliar de bancos de la empresa con los movimientos que reporta la entidad financiera en su extracto, con el fin de identificar y explicar cualquier diferencia entre ambos saldos, y dejar ambos registros cuadrados en $0 de diferencia.


No es un requisito exigido de forma explícita como trámite independiente ante la DIAN, pero en la práctica es indispensable para tres cosas muy concretas en Colombia:


- Sustentar el saldo de la cuenta de bancos que se reporta en el Balance General frente a revisoría fiscal o auditoría externa.
- Cumplir con el sistema de control interno que exige la **Ley 87 de 1993** para entidades obligadas, dentro del cual la conciliación de cuentas es un control clave.
- Detectar oportunamente gastos financieros (comisiones, GMF, intereses) que impactan la deducibilidad de costos ante el Estatuto Tributario.


## Tipos de conciliación bancaria


- **Conciliación aritmética o de dos saldos:** el método más usado; parte del saldo en libros y del saldo en extracto y ajusta ambos hasta que coincidan. Es la que se explica en detalle más abajo.
- **Conciliación diaria:** propia de empresas con alto volumen de transacciones (comercio electrónico, retail), donde esperar a fin de mes generaría demasiadas partidas acumuladas.
- **Conciliación periódica (mensual, trimestral):** la más común en pymes; se recomienda no superar el mes entre una conciliación y otra.
- **Conciliación automática o conectada:** el software contable (Siigo, Alegra, World Office, Helisa, entre otros) se conecta al banco o procesa el archivo plano del extracto y cruza automáticamente las partidas coincidentes por fecha, valor y/o documento, dejando solo las diferencias reales para revisión manual.


## Documentos que necesitas antes de empezar


1. Extracto bancario del periodo a conciliar (PDF, archivo plano o vía conexión API del banco).
2. Libro auxiliar de la cuenta contable de bancos (código PUC 1110 o 1105, según el caso).
3. Conciliación bancaria del periodo anterior, para verificar que el saldo inicial de este mes coincide con el saldo final conciliado del mes previo.
4. Soportes de las operaciones: comprobantes de egreso, recibos de caja, facturas, notas débito y crédito emitidas por el banco.


## Partidas de tiempo vs. partidas que exigen ajuste contable


Esta es una de las distinciones que más se pasa por alto en el contenido sobre el tema, y es la que más confunde a quien está aprendiendo: **no todas las diferencias se corrigen igual.** Algunas simplemente se explican por el desfase de tiempo entre el registro de la empresa y el del banco (no requieren asiento contable, solo quedan documentadas como partida conciliatoria), mientras que otras sí exigen un ajuste en el libro auxiliar porque la empresa aún no las ha registrado.


Tipo de partida Ejemplo ¿Requiere asiento contable? ¿Quién debe reflejarla eventualmente?


Partida en tránsito (timing) Consignación registrada en libros el día 30, pero abonada por el banco el día 2 del mes siguiente No, se resuelve sola en el siguiente extracto Ambos registros ya la tienen, solo difiere la fecha


Cheque girado no cobrado Cheque contabilizado como egreso, pero el beneficiario aún no lo ha cobrado en el banco No, queda como partida pendiente hasta que se cobre El banco lo reflejará cuando se haga efectivo


Nota débito bancaria no contabilizada Comisión de manejo, cuota de tarjeta débito, GMF (4x1000) **Sí** , la empresa debe registrar el gasto La empresa (falta en libros)


Nota crédito bancaria no contabilizada Intereses ganados por la cuenta de ahorros, reversión de un cobro **Sí** , la empresa debe registrar el ingreso La empresa (falta en libros)


Error de registro Se contabilizó $450.000 en vez de $540.000 **Sí** , corrección del valor Quien cometió el error (empresa o banco)


Error del banco Cargo duplicado o mal aplicado a la cuenta No en libros; se reclama al banco El banco corrige en su extracto


## Formato de conciliación bancaria: método de los dos saldos


El formato más usado parte de dos columnas —saldo según libros y saldo según extracto— y ajusta cada uno con sus partidas pendientes hasta que ambos lleguen al mismo saldo conciliado final.


Concepto Valor (COP)


Saldo según libros (auxiliar de bancos) $12.480.000


(+) Nota crédito por intereses no contabilizada $35.000


(–) Nota débito por comisión de manejo no contabilizada $18.500


(–) GMF (4x1000) no contabilizado $12.300


**Saldo ajustado en libros** **$12.484.200**


Saldo según extracto bancario $12.620.000


(+) Consignación en tránsito (registrada en libros, aún no reflejada por el banco) $150.000


(–) Cheque girado no cobrado $285.800


**Saldo ajustado según banco** **$12.484.200**


Cuando el saldo ajustado en libros coincide con el saldo ajustado según banco, la conciliación está completa. Si no coincide, el diferencial debe seguir investigándose antes de cerrar el periodo.


## Cómo hacer la conciliación bancaria: paso a paso


1. **Verifica el saldo inicial.** Confirma que el saldo con el que arranca el mes en tu libro auxiliar coincide con el saldo final conciliado del mes anterior.
2. **Cruza los movimientos uno a uno.** Compara cada consignación, cheque, transferencia y nota bancaria del extracto contra el libro auxiliar, por fecha, valor y/o número de documento.
3. **Separa las partidas en tránsito de los faltantes reales.** Identifica qué diferencias son solo de tiempo (se resolverán solas) y cuáles corresponden a movimientos que la empresa aún no ha registrado.
4. **Contabiliza los ajustes que le correspondan a la empresa.** Registra en el libro auxiliar las notas débito, notas crédito, comisiones, GMF e intereses que el banco ya aplicó pero que la empresa no tenía contabilizados.
5. **Documenta los errores y repórtalos.** Si el error es de la empresa, corrígelo en el sistema contable; si es del banco, solicita la corrección formalmente y deja evidencia de la comunicación.
6. **Calcula ambos saldos ajustados y verifica que coincidan.** Si el saldo ajustado en libros es igual al saldo ajustado según extracto, la conciliación queda cuadrada.
7. **Firma y archiva el formato de conciliación.** Guarda el documento con sus soportes; es la evidencia que pedirá revisoría fiscal o auditoría, y sirve como punto de partida para el mes siguiente.


## Ejemplo práctico con asientos contables


Retomando la tabla del formato anterior, así quedarían los asientos de ajuste que la empresa debe registrar en su libro auxiliar, usando el Plan Único de Cuentas (PUC):


**Ajuste 1 — Intereses ganados no contabilizados ($35.000)**


Cuenta Código PUC Débito Crédito


Bancos 1110 35.000


Ingresos financieros 4210 35.000


**Ajuste 2 — Comisión de manejo no contabilizada ($18.500)**


Cuenta Código PUC Débito Crédito


Gastos financieros 5305 18.500


Bancos 1110 18.500


**Ajuste 3 — GMF (4x1000) no contabilizado ($12.300)**


Cuenta Código PUC Débito Crédito


Impuestos asumidos (gravamen movimientos financieros) 511595 12.300


Bancos 1110 12.300


Después de registrar estos tres ajustes, el saldo del libro auxiliar de bancos pasa de $12.480.000 a $12.484.200, coincidiendo con el saldo ajustado del extracto bancario.


## El GMF (4x1000): la partida que más descuadres genera en Colombia


El Gravamen a los Movimientos Financieros es una nota débito recurrente que muchos auxiliares contables clasifican mal. Un error frecuente es registrarlo dentro de "Gastos bancarios" (código 530505) en lugar de la subcuenta correspondiente a impuestos asumidos (511595). La suma total del mes puede cuadrar en el agregado, pero la conciliación cuenta por cuenta contra el extracto queda descuadrada porque cada movimiento vive en un grupo distinto del PUC. Si notas una diferencia pequeña y recurrente mes a mes, revisa primero cómo estás clasificando el 4x1000 antes de asumir que es un error de otro tipo.


## Errores comunes al hacer la conciliación bancaria


- **Reclasificar mal el GMF (4x1000)** , como se explicó arriba.
- **Fechas cruzadas entre meses:** una transacción del día 31 puede aparecer reflejada en el banco el día 1 del mes siguiente, generando una diferencia que no es un error real.
- **Duplicar notas débito:** las comisiones bancarias a veces quedan registradas dos veces si se procesan tanto de forma manual como automática.
- **No conciliar cuentas con poco movimiento:** las cuentas bancarias inactivas o de uso ocasional también deben conciliarse; ahí suelen esconderse comisiones de mantenimiento que nadie está registrando.
- **Confundir partidas en tránsito con errores:** tratar de "corregir" en libros algo que en realidad solo necesita esperar al siguiente extracto genera descuadres innecesarios el mes siguiente.
- **No arrastrar el saldo conciliado anterior:** empezar la conciliación del mes sin verificar que coincide con el cierre del mes previo hace que los errores se acumulen sin detectarse.


## ¿Con qué frecuencia se debe hacer?


No existe una periodicidad única obligatoria, pero la práctica recomendada varía según el volumen de operación:


- **Empresas con alto volumen de transacciones** (comercio, e-commerce): conciliación semanal o incluso diaria.
- **Pymes con volumen moderado:** conciliación mensual, alineada con el cierre contable y el extracto que envía el banco.
- **Empresas de Grupo NIIF 1 y 2** con revisoría fiscal: conciliación mensual como mínimo, documentada y firmada, ya que suele ser objeto de revisión directa por parte del revisor fiscal.
- [Microempresas (Grupo NIIF 3)](https://cifrato.ai/blog/grupo-3-niif-microempresas) : conciliación mensual es suficiente en la mayoría de los casos, dado el menor volumen de operaciones.


## Excel vs. software contable: ¿cuándo automatizar?


Hacer la conciliación en Excel con fórmulas como` BUSCARV` (para cruzar por fecha) o` CONTAR.SI` (para cruzar por valor) funciona bien para empresas con pocas transacciones al mes, pero se vuelve un cuello de botella cuando el volumen crece, porque cada transacción hay que cruzarla manualmente y el riesgo de duplicar o pasar por alto un movimiento aumenta. Software contable como Siigo, Alegra, World Office o Helisa permiten importar el extracto (manual o vía conexión bancaria) y cruzan automáticamente las partidas coincidentes por documento, fecha y valor, dejando visibles solo las diferencias reales que requieren revisión humana.


## Checklist de conciliación bancaria mensual


- Verificar que el saldo inicial coincide con el cierre conciliado del mes anterior.
- Descargar o solicitar el extracto bancario completo del periodo.
- Exportar el libro auxiliar de bancos del mismo periodo.
- Cruzar cada movimiento por fecha, valor y documento.
- Separar partidas en tránsito de faltantes reales.
- Registrar los asientos de ajuste correspondientes (notas débito, notas crédito, GMF, comisiones).
- Confirmar que el GMF (4x1000) está en la subcuenta correcta (511595).
- Verificar que el saldo ajustado en libros coincide con el saldo ajustado del extracto.
- Firmar y archivar el formato de conciliación con sus soportes.


## Preguntas frecuentes


**¿La conciliación bancaria es obligatoria en Colombia?** No existe una norma que la exija como trámite independiente ante la DIAN, pero es un control esencial dentro del sistema de control interno (Ley 87 de 1993) y una práctica que revisoría fiscal y auditoría externa esperan encontrar documentada.


**¿Qué pasa si el saldo del extracto y el de los libros nunca coinciden?** Significa que hay al menos un error de registro sin corregir o una partida sin contabilizar. Se debe investigar movimiento por movimiento hasta identificar el origen de la diferencia; no se recomienda "forzar" el cuadre con un ajuste genérico sin sustento.


**¿En qué cuenta del PUC se registra el GMF (4x1000)?** En la subcuenta 511595 (Impuestos asumidos), no en la cuenta genérica de gastos bancarios (530505), para evitar descuadres en la conciliación por cuenta.
