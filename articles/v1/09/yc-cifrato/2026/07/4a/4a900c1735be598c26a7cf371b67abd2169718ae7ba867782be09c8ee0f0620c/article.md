---
schema_version: "1.0.0"
document_id: "4a900c1735be598c26a7cf371b67abd2169718ae7ba867782be09c8ee0f0620c"
company_key: "yc-cifrato"
company: "Cifrato"
source_id: "yc-cifrato-news-import-c3bc34360ee9"
canonical_url: "https://cifrato.ai/blog/como-anular-una-factura-en-la-dian"
published_at: "2026-07-20T01:02:00+00:00"
first_seen_at: "2026-07-24T01:30:37.138560+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:d628bca2411a22f83c73f62e0aa441fd0830ed5b9b6d0cf00b9b279104c97bcf"
---

# Cómo anular una factura en la DIAN

Una factura electrónica que ya fue **validada por la DIAN no se puede eliminar ni "anular" en el sentido literal de la palabra** : queda registrada para siempre con su Código Único de Factura Electrónica (CUFE), y su número no puede reutilizarse. Lo que la norma llama "anulación" en realidad se hace emitiendo una **nota crédito electrónica** que referencia esa factura y la deja en ceros. Solo si tu factura aún no ha sido validada por la DIAN (está en borrador o fue rechazada) puedes eliminarla directamente desde tu software de facturación, sin necesidad de ningún documento adicional.


## Antes vs. después de la validación: dos procesos completamente distintos


Estado de la factura ¿Se puede anular directamente? Qué debes hacer


Guardada como borrador, sin enviar a la DIAN Sí Elimínala o anúlala desde tu software; no queda ningún rastro fiscal


Enviada y rechazada por la DIAN (por errores técnicos) Sí Puedes anularla en tu sistema; al no haber sido validada, no existe legalmente como factura


Validada (aprobada) por la DIAN No, nunca se elimina Debes emitir una nota crédito electrónica que la anule; el número de la factura queda inhabilitado para siempre


Aceptada expresa o tácitamente por el comprador (ya es título valor) No Igual debes usar nota crédito, pero el proceso técnico cambia (ver sección más abajo)


Este es el punto que más confunde a las empresas: **la nota crédito no "borra" la factura, la reemplaza contablemente dejándola en cero** , pero la factura original sigue existiendo en los sistemas de la DIAN con su CUFE y su historial.


## Paso a paso para anular una factura electrónica validada


1. **Ubica la factura original** en tu software de facturación, usando su número de comprobante o su CUFE.
2. **Verifica el estado de aceptación** de la factura frente al comprador (ver la sección siguiente, porque esto cambia el proceso).
3. **Genera una nota crédito electrónica** desde tu sistema, referenciando obligatoriamente el CUFE de la factura que quieres anular.
4. **Selecciona el concepto correcto** : para una anulación total, el concepto es "anulación de factura electrónica" (no "devolución" ni "descuento", que son conceptos distintos).
5. **Verifica que el valor de la nota crédito sea igual al valor total de la factura** , si tu intención es anularla por completo.
6. **Transmite la nota crédito a la DIAN** para su validación; recibirás un Código Único de Documento Electrónico (CUDE) propio de la nota crédito.
7. **Envía la nota crédito a tu cliente** , para que quede reflejada en su contabilidad y en su información exógena.
8. **Guarda el soporte** de la nota crédito junto con la factura original; ambos documentos deben conservarse, ya que la factura anulada no desaparece de tu historial.


**Importante:** el número de la factura anulada queda inhabilitado permanentemente. No puedes volver a usarlo para otra venta, ni siquiera si la operación anulada nunca llegó a concretarse.


## Qué pasa si la factura ya fue aceptada por el cliente


Una factura electrónica se considera aceptada, y por lo tanto adquiere la calidad de título valor, cuando el comprador la acepta expresamente o cuando pasan tres días hábiles sin que la rechace (aceptación tácita). Esto complica el proceso de anulación:


- Si intentas generar la nota crédito de forma normal y la DIAN la rechaza por figurar la factura como "aceptada", debes emitir la nota crédito **sin referenciarla directamente a la factura** , usando el concepto que aplica para estos casos.
- En la práctica, esto significa acordar con tu cliente que acepte la nota crédito por fuera del flujo automático de referencia, ya que la factura como título valor tiene efectos legales (puede respaldar un cobro judicial, por ejemplo) que no desaparecen solo porque tú quieras anularla.
- Si el error se detecta antes de que el cliente acepte la factura (dentro de esos tres días hábiles), el proceso de nota crédito es mucho más simple porque la referencia directa al CUFE sí funciona sin inconvenientes.


## Los conceptos de una nota crédito (y por qué elegir el correcto importa)


Cuando generas una nota crédito, la DIAN te exige indicar el motivo del ajuste. Los conceptos más usados son:


- **Anulación de la factura electrónica** : cuando quieres dejar la operación completamente sin efecto.
- **Devolución parcial o total de los bienes** : cuando el cliente devuelve productos, pero la venta como tal no se anula del todo.
- **Rebaja o descuento parcial o total** : cuando se otorga un descuento posterior que no estaba en la factura original.
- **Ajuste de precio** : cuando se corrige un valor unitario o total hacia abajo, sin que medie una devolución.
- **Otros** : concepto residual para casos no cubiertos por los anteriores.


Elegir el concepto equivocado no es un simple detalle administrativo: define cómo queda registrado el ajuste ante la DIAN y puede generar inconsistencias en tus reportes de información exógena o en tu declaración de IVA.


## Errores comunes al anular facturas (y por qué te pueden costar caro)


- **Emitir una factura nueva en lugar de una nota crédito.** Esto genera duplicidad de operaciones ante la DIAN y puede inflar artificialmente tus ingresos reportados.
- **No referenciar el CUFE de la factura original en la nota crédito.** Sin esa referencia, la DIAN no puede vincular ambos documentos, y tu contabilidad queda descuadrada.
- **Reutilizar el número de una factura ya anulada.** La norma lo prohíbe expresamente; cada número anulado queda inhabilitado.
- **Usar la anulación para hacer "desaparecer" ingresos** o moverlos de un período fiscal a otro. La DIAN identifica estos patrones y puede iniciar un proceso de fiscalización.
- **Ajustar una nota crédito con otra nota crédito o una nota débito de forma cruzada.** La normativa no permite este tipo de compensaciones entre notas; cada documento debe corregirse por el mecanismo que le corresponde.
- **Olvidar que el IVA sigue siendo exigible** mientras la factura no haya sido efectivamente anulada con la nota crédito correspondiente. Si nunca generas la nota crédito, el IVA discriminado en la factura original sigue debiendo declararse y pagarse, aunque la venta no se haya concretado.


## Implicaciones tributarias de no anular correctamente


Mientras una factura electrónica no sea anulada mediante nota crédito, la DIAN la considera vigente para todos los efectos, incluyendo:


- El **IVA discriminado** en la factura sigue siendo exigible y debe declararse en el período correspondiente, sin importar si la venta finalmente no se realizó.
- Los **ingresos facturados** pueden seguir apareciendo en tu información exógena y afectar tu declaración de renta si no corriges la operación a tiempo.
- Tu cliente puede tener problemas para soportar sus propios costos o deducciones si la factura que recibió no corresponde a una operación real, y tú no emitiste el documento que la corrige.


## Preguntas frecuentes


**¿Puedo anular una factura del año pasado en el año actual?** Sí, puedes emitir una nota crédito en un período posterior al de la factura original, referenciando su CUFE. Lo importante es que la nota quede correctamente vinculada a la factura que corrige.


**¿Una nota crédito electrónica se puede anular?** Si aún no ha sido enviada o aprobada por la DIAN, sí puedes anularla directamente en tu software. Si ya fue aprobada, no se puede eliminar; para revertir su efecto necesitas emitir una nota débito por el valor correspondiente.


**¿Qué pasa si mi cliente rechaza la factura electrónica?** Si el rechazo ocurre antes de la aceptación (dentro del plazo de tres días hábiles), en general procede la nota crédito referenciando la factura para anularla. Si el rechazo ocurre por otros motivos después de la aceptación, el tratamiento requiere revisar el caso puntual con tu contador o proveedor tecnológico.


**¿Puedo simplemente borrar una factura electrónica de mi sistema contable?** Solo si nunca fue validada por la DIAN. Una vez validada, borrarla de tu sistema interno no elimina su existencia legal ante la DIAN; siempre debes anularla con nota crédito para que ambos registros (el tuyo y el de la DIAN) queden consistentes.


**¿La anulación de una factura afecta mi declaración de IVA del período?** Sí. Si la nota crédito se emite dentro del mismo período gravable, el ajuste se refleja directamente. Si se emite en un período posterior, el efecto se reconoce en el período en que se emite la nota crédito, no de forma retroactiva.
