---
schema_version: "1.0.0"
document_id: "ec3f1f15fb7be4e2b5c8ecd12f13b81d9cef14b720214b4f63604f4651db1790"
company_key: "yc-coba"
company: "Coba"
source_id: "yc-coba-atom-20474b558f09"
canonical_url: "https://blog.coba.ai/post/horario-corte-transferencias-internacionales/"
published_at: "2026-06-02T00:00:00+00:00"
first_seen_at: "2026-07-24T22:45:10.768066+00:00"
fetched_at: "2026-07-28T20:49:52.299135+00:00"
content_hash: "sha256:607cfe60fd394c8cb7aee521712a67c0626b4e068e95662f2ebc2b2d24fae314"
---

# Horario de corte en transferencias internacionales

El horario de corte es la hora límite que usa un banco o proveedor para procesar una transferencia el mismo día. Si mandas la instrucción antes del corte, el pago puede salir hoy; si la mandas después, normalmente se procesa hasta el siguiente día hábil. En pagos internacionales, ese límite pesa más porque no solo depende de tu banco: también puede depender del banco corresponsal, la moneda, el país destino y el riel por donde viaja el dinero.


Por eso el clásico “lo mando tantito después de las 4” no siempre es un detalle. A veces significa que la nómina llega mañana, que el proveedor no libera el embarque o que el equipo empieza a perseguir comprobantes por WhatsApp.


## ¿Qué es un horario de corte bancario?


Un horario de corte bancario es la frontera operativa del día. No necesariamente coincide con el horario de atención de la sucursal ni con la hora en la que tú capturas el pago en el portal. Es la hora a partir de la cual el banco deja de enviar ciertas operaciones para liquidarlas ese mismo día.


En México, muchos equipos de finanzas están acostumbrados a que SPEI funcione rápido para pagos nacionales. Pero cuando el pago cruza moneda o país —por ejemplo USD a MXN, MXN a USD o un wire hacia Estados Unidos— entran más piezas:


- La hora límite del banco que envía.
- La validación interna del pago.
- La disponibilidad de la moneda.
- El riel usado: SPEI, ACH, Fedwire, SWIFT u otro.
- El banco receptor o corresponsal.
- Días hábiles y festivos en más de un país.


Ese es el punto: el pago internacional no vive en un solo reloj. Vive en varios.


## ¿Hasta qué hora puedo hacer una transferencia internacional?


No hay una hora universal. Cada banco y cada riel tienen su propio corte. Como regla práctica, mientras más internacional y más manual sea el flujo, más temprano suele cerrar.


Para una empresa mexicana que mueve dinero entre México y Estados Unidos, los escenarios comunes son:


### SPEI en México


SPEI puede operar 24/7 para muchos pagos nacionales, pero eso no significa que todo pago relacionado con dólares o cuentas empresariales se liquide igual. Si tu banco necesita convertir moneda, revisar documentos, aprobar una instrucción empresarial o fondear una cuenta antes de mandar SPEI, el horario interno del banco puede volverse el verdadero corte.


### ACH en Estados Unidos


ACH es barato y útil para pagos domésticos en EE. UU., pero no siempre es instantáneo. Según el tipo de ACH y el banco, puede tardar uno o dos días hábiles. También tiene ventanas de procesamiento. Si quieres que un cliente en EE. UU. te pague por ACH, necesitas datos bancarios locales —routing number y account number— y entender cuándo se origina el pago.


### Wire doméstico en EE. UU.


Fedwire suele ser el camino más rápido para pagos grandes en dólares dentro de EE. UU. Puede liquidar el mismo día, pero también tiene hora límite. Si el wire se captura tarde, la operación puede moverse al siguiente día hábil.


### SWIFT internacional


SWIFT es el camino clásico para transferencias internacionales entre bancos. También es el menos predecible para el equipo de finanzas: puede pasar por bancos intermediarios, tener revisiones adicionales y tardar de dos a cinco días hábiles. Aquí el horario de corte no es el único problema; la visibilidad también se vuelve limitada.


## ¿Por qué una transferencia USD a MXN puede tardar aunque la captures temprano?


Porque capturar no es lo mismo que liquidar.


Tu equipo puede meter la instrucción a las 10:00 a.m., pero todavía falta que el banco valide, convierta, fondee, envíe y reciba confirmación. Si una de esas piezas depende de otra mesa, otro banco o una hora de corte externa, el pago puede quedarse atorado aunque tú “lo hayas mandado a tiempo”.


Lo escuchamos mucho en operaciones que cobran en dólares y pagan en pesos. El dinero está en un lado, la obligación está en otro, y el desfase se vuelve operativo: alguien está esperando nómina, un proveedor necesita comprobante o una factura no puede cerrarse porque el pago no aparece.


Ahí es donde el horario de corte deja de ser un dato técnico. Se vuelve riesgo operativo.


## El problema real: el bomberazo de las 4 p.m.


“Que sean las 4:05 no debería significar que tu pago se va hasta mañana.”


Pero en muchas operaciones sí significa eso.


Una empresa puede tener el dinero, la factura y el proveedor listos. Lo que no tiene es margen. Si la cobranza cayó tarde, si el banco tardó en liberar los dólares o si alguien tuvo que llamar para cerrar el tipo de cambio, el pago llega al portal a una hora incómoda. Entonces aparece el bomberazo:


- “¿Todavía alcanza a salir hoy?”
- “¿Quién lo puede autorizar?”
- “¿Ya cerraron tipo de cambio?”
- “¿Hay comprobante?”
- “Si no sale hoy, ¿qué pasa con el proveedor?”


Ese relajo no se ve en la comisión bancaria, pero le cuesta horas al equipo. También le cuesta confianza con proveedores y clientes.


## ¿Cómo evitar que el horario de corte controle tu operación?


No siempre puedes cambiar el reloj del banco, pero sí puedes diseñar el flujo para depender menos de él.


### 1. Separa “capturé el pago” de “el pago salió”


La captura es una intención. La salida real es otra cosa. Tu equipo necesita saber cuál es el estado del pago: creado, autorizado, fondeado, enviado, recibido o conciliado. Si todo se reduce a “ya lo subí al portal”, estás operando a ciegas.


### 2. Define por riel, no por país


No preguntes solo “¿cuánto tarda a Estados Unidos?” Pregunta por el riel exacto: ACH, wire doméstico, SWIFT, SPEI, transferencia local. Dos pagos hacia el mismo país pueden tener tiempos muy distintos según el riel.


### 3. Cierra tipo de cambio antes de que sea emergencia


Si cada operación USD/MXN depende de una llamada para conseguir tipo de cambio, el horario de corte se vuelve más apretado. El equipo no solo está esperando autorización del pago; está esperando precio. Para entender este costo, también vale revisar[qué es el spread cambiario](https://blog.coba.ai/post/que-es-el-spread-cambiario/) y cómo medirlo en puntos base.


### 4. Usa cuentas y rieles que reduzcan vueltas


Si cobras en dólares y pagas en pesos, o si tienes clientes en EE. UU., una cuenta que permita recibir USD con datos locales puede reducir fricción antes de convertir. La pregunta no es solo dónde recibes, sino qué tan rápido puedes mover después. Si estás evaluando opciones, aquí explicamos[cómo recibir pagos en dólares siendo empresa mexicana](https://blog.coba.ai/post/recibir-pagos-en-dolares-empresa-mexicana/) .


### 5. Documenta el SLA interno


Tu proveedor debería poder decirte, por moneda y por riel: hasta qué hora entra same-day, qué pasa después del corte, cuándo se genera comprobante y qué información tendrás si algo se retrasa. Si no hay respuesta clara, tu equipo va a llenar el hueco con llamadas.


## ¿Qué debe ver un equipo de finanzas antes de mandar un pago internacional?


Antes de dar clic, el equipo debería tener claridad sobre cinco cosas:


1. **Monto final.** Cuánto sale y cuánto llega.
2. **Tipo de cambio.** Cuál se aplica y contra qué referencia.
3. **Hora límite.** Si todavía entra hoy o no.
4. **Estado del pago.** Qué pasa después de autorizarlo.
5. **Comprobante.** Qué evidencia queda para conciliar y responder.


Si falta una de esas, el pago puede salir, pero la operación queda incompleta. Y cuando alguien pregunte “¿ya cayó?”, nadie va a tener una respuesta limpia.


## Cómo lo pensamos en Coba


En Coba no vemos el horario de corte como un detalle escondido en términos y condiciones. Lo vemos como parte del flujo de pago.


Mover dinero rápido importa. Pero también importa saber qué pasa si el pago se manda a las 3:45, a las 4:05 o el viernes antes de nómina. Importa que el tipo de cambio esté claro antes de operar. Importa que el comprobante exista cuando el proveedor lo pide. Importa que el equipo no tenga que vivir pegado al portal para saber si el dinero salió.


El objetivo no es prometer que nunca habrá cortes. Todos los rieles tienen reglas. El objetivo es que esas reglas no te agarren por sorpresa.


Un buen flujo de pagos internacionales debería sentirse así: sé hasta qué hora entra, sé cuánto cuesta, sé cuándo sale, tengo comprobante y puedo darle a lo que sigue.
