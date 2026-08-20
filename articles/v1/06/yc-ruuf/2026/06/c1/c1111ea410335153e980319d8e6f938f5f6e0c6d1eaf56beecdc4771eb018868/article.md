---
schema_version: "1.0.0"
document_id: "c1111ea410335153e980319d8e6f938f5f6e0c6d1eaf56beecdc4771eb018868"
company_key: "yc-ruuf"
company: "RUUF"
source_id: "yc-ruuf-news-import-7ad24376001b"
canonical_url: "https://ruuf.cl/blog/disyuntor-termomagnetico"
published_at: "2026-06-08T18:55:10.688+00:00"
first_seen_at: "2026-07-24T11:43:06.125313+00:00"
fetched_at: "2026-07-28T21:42:42.932365+00:00"
content_hash: "sha256:9363c858d14c80a825fd7230bbbded727796719fea28cb10d145c3531c6aa14e"
---

# Disyuntor termomagnético

Si alguien te muestra un sistema solar fotovoltaico y te señala los paneles, el inversor y el[medidor bidireccional](https://ruuf.cl/blog/medidor-bidireccional) , probablemente estás viendo todo lo que la gente considera "importante".


Pero hay un componente pequeño, silencioso y montado dentro del tablero eléctrico que puede ser la diferencia entre un incidente menor y un incendio: el disyuntor termomagnético.


No es glamoroso. No tiene app. Nadie lo sube a Instagram. Pero sin él, tu sistema solar no es seguro y no es legal.


Este artículo explica qué es un disyuntor termomagnético, cómo funciona, por qué es obligatorio en toda instalación solar en Chile, qué tipos necesita tu sistema y cómo saber si el tuyo está bien protegido.


## **Qué es un disyuntor termomagnético**


Un disyuntor termomagnético (también llamado interruptor automático, breaker o automático) es un dispositivo de protección eléctrica que corta el paso de corriente cuando detecta una condición anormal: una sobrecarga o un cortocircuito.


Es como un fusible inteligente que se puede rearmar. Cuando un fusible se funde, lo tiras y pones uno nuevo.


Cuando un disyuntor se dispara, lo reseteas levantando la palanca y vuelve a funcionar (siempre que la causa del disparo se haya resuelto).


El nombre "termomagnético" viene de sus dos mecanismos de protección internos, que trabajan en conjunto para cubrir dos tipos de falla distintos.


### **El mecanismo térmico: protección contra sobrecargas**


Dentro del disyuntor hay una lámina bimetálica: dos metales diferentes unidos que se expanden a velocidades distintas cuando se calientan.


Cuando la corriente que pasa por el disyuntor supera su capacidad nominal de forma sostenida (una sobrecarga), la lámina se calienta, se curva y acciona el mecanismo de desconexión. La respuesta es lenta y proporcional. Una sobrecarga leve tarda más en disparar el disyuntor.


Una sobrecarga fuerte lo dispara más rápido. Esto tiene sentido porque las sobrecargas leves y breves son normales (el arranque de un motor, el encendido de un compresor de refrigerador) y no deberían cortar la energía cada vez que ocurren.


Este mecanismo protege contra situaciones como tener demasiados aparatos enchufados en un circuito, un electrodoméstico con un motor que está trabado y consume más de lo normal, o un cable que está calentándose porque lleva más corriente de la que debería.


### **El mecanismo magnético: protección contra cortocircuitos**


Cuando ocurre un cortocircuito (la corriente positiva hace contacto directo con la negativa, sin pasar por una carga), la corriente se dispara a niveles enormes en milisegundos.


El mecanismo térmico es demasiado lento para reaccionar a tiempo.


Aquí entra el mecanismo magnético: una bobina electromagnética que genera un campo magnético proporcional a la corriente que la atraviesa.


Cuando la corriente sube brutalmente (un cortocircuito), el campo magnético se intensifica lo suficiente para accionar instantáneamente el mecanismo de desconexión.


La respuesta es rápida. Milisegundos. Lo necesario para cortar la corriente antes de que los cables se derritan, salte una chispa o se inicie un incendio.


### **Dos mecanismos, una sola protección**


La genialidad del disyuntor termomagnético es que combina ambas protecciones en un solo dispositivo compacto:


-


**Sobrecarga sostenida** → la lámina bimetálica (térmico) actúa en segundos a minutos.


-


**Cortocircuito** → la bobina magnética actúa en milisegundos.


Sin la parte térmica, una sobrecarga prolongada podría calentar cables y causar un incendio sin que nada lo detecte. Sin la parte magnética, un cortocircuito podría generar un arco eléctrico destructivo antes de que el mecanismo lento reaccione.


## **Por qué tu sistema solar necesita disyuntores termomagnéticos**


Un sistema solar fotovoltaico tiene dos circuitos eléctricos que necesitan protección: el circuito de corriente continua (CC) del lado de los paneles y el circuito de corriente alterna (CA) del lado de la casa. Cada circuito tiene riesgos distintos y necesita protecciones específicas.


### **Protección en el lado de corriente continua (CC)**


El circuito CC va desde los paneles solares en el techo hasta la entrada del inversor.


En un sistema residencial típico, este circuito lleva voltajes de 150V a 600V CC, dependiendo de cuántos paneles estén conectados en serie. El disyuntor termomagnético de CC (también llamado seccionador CC o interruptor automático DC) se instala entre los paneles y el inversor. Cumple dos funciones:


-


**Protección:** corta la corriente si hay una sobrecarga o cortocircuito en el cableado CC del techo o en la entrada del inversor.


-


**Seccionamiento:** permite desconectar manualmente los paneles del inversor para mantenimiento, reparaciones o emergencias. Sin este disyuntor, la única forma de "apagar" los paneles sería desconectar cada conector MC4 en el techo, lo que es impráctico y peligroso.


Es importante que el disyuntor CC sea específico para corriente continua. No puedes usar un disyuntor de corriente alterna en un circuito CC.


La corriente continua no tiene cruces por cero (momentos donde la corriente pasa por 0V), lo que hace que los arcos eléctricos al abrir el circuito sean más difíciles de extinguir.


Un disyuntor de CA no está diseñado para manejar eso y puede fallar peligrosamente. Los disyuntores CC para sistemas solares se especifican según:


-


**Voltaje nominal DC:** debe ser igual o superior al voltaje máximo del string (Voc multiplicado por el número de paneles en serie, corregido por temperatura mínima). Valores comunes: 500V CC, 800V CC, 1000V CC.


-


**Corriente nominal:** debe ser igual o superior a la corriente de cortocircuito del string (Isc) multiplicada por un factor de seguridad de 1,25.


-


**Número de polos:** generalmente 2 polos para desconectar tanto el positivo como el negativo del string.


### **Protección en el lado de corriente alterna (CA)**


El circuito CA va desde la salida del inversor hasta el tablero eléctrico de tu casa, y desde ahí se conecta a la red. Este circuito opera a 220V CA monofásico (el estándar residencial en Chile).


El disyuntor termomagnético de CA se instala en el tablero eléctrico, en un circuito dedicado exclusivamente para el sistema solar.


Protege contra sobrecargas y cortocircuitos en el cableado entre el inversor y el tablero.


Este disyuntor sí es un automático estándar de corriente alterna, similar a los que ya tienes en tu tablero para los circuitos de enchufes y luces.


La diferencia es que es exclusivo para el circuito solar y su capacidad debe coincidir con la corriente de salida del inversor. Especificaciones típicas para residencial:


-


**Voltaje nominal:** 220V CA o 400V CA (según si es monofásico o trifásico).


-


**Corriente nominal:** 16A, 20A, 25A o 32A, según la potencia del inversor. Un inversor de 5 kW a 220V entrega aproximadamente 23A, por lo que un automático de 25A o 32A es lo apropiado.


-


**Número de polos:** bipolar (2 polos) para instalaciones monofásicas en Chile.


## **El disyuntor no trabaja solo: las otras protecciones de tu sistema**


El disyuntor termomagnético es una pieza clave, pero no es la única protección que necesita tu sistema solar. Funciona en conjunto con otros dispositivos.


### **Disyuntor diferencial (interruptor diferencial)**


Mientras el disyuntor termomagnético protege contra sobrecargas y cortocircuitos, el disyuntor diferencial protege contra fugas de corriente a tierra.


Si una persona toca un cable energizado, o si el aislamiento de un cable se daña y la corriente empieza a "fugarse" hacia la estructura metálica del techo, el diferencial detecta esa fuga y corta la energía en milisegundos.


En sistemas solares residenciales en Chile, se instala un diferencial dedicado en el lado CA, generalmente de 30 mA de sensibilidad (la corriente mínima a la que reacciona).


Algunos inversores requieren un diferencial de tipo A o tipo B, dependiendo de si pueden generar corrientes de fuga con componente continua.


En la práctica, muchos instaladores usan un dispositivo combinado (disyuntor magnetotérmico + diferencial en un solo equipo), lo que simplifica la instalación y ahorra espacio en el tablero.


### **Fusibles CC**


Además del disyuntor termomagnético CC, en sistemas con múltiples strings conectados en paralelo se agregan fusibles en cada string. Si un string falla, el fusible de ese string se funde y aísla la falla sin afectar a los demás strings.


Los fusibles solares CC son del tipo gPV (específicos para fotovoltaica), formato 10x38 mm, y se montan en portafusibles sobre riel DIN dentro de una caja de protecciones CC.


### **Protector de sobretensión (descargador de sobretensiones)**


Protege contra picos de voltaje causados por descargas atmosféricas (rayos) o fluctuaciones de la red. Se instala tanto en el lado CC (entre los paneles y el inversor) como en el lado CA (en el tablero). Es especialmente importante en zonas con tormentas eléctricas frecuentes.


### **Puesta a tierra**


Todas las partes metálicas del sistema (estructura de montaje, marcos de los paneles, carcasa del inversor) deben estar conectadas a la malla de tierra de la vivienda. La puesta a tierra desvía corrientes de falla hacia la tierra en vez de hacia las personas o los equipos.


## **Qué dice la normativa chilena**


En Chile, la instalación de sistemas solares fotovoltaicos conectados a la red está regulada por la SEC (Superintendencia de Electricidad y Combustibles) y debe cumplir con la Instrucción Técnica RGR N°02/2024 de la SEC y las normas eléctricas vigentes. La normativa exige:


-


**Protecciones termomagnéticas obligatorias** tanto en el lado CC como en el CA para desconectar o aislar la instalación fotovoltaica de la red. En instalaciones monofásicas, el disyuntor debe ser bipolar.


-


**Disyuntor diferencial** en el circuito CA del sistema solar.


-


**Seccionamiento visible:** un medio de desconexión accesible que permita aislar completamente el sistema fotovoltaico de la red. En la práctica, el disyuntor termomagnético del tablero cumple esta función.


-


**Declaración TE4:** la instalación debe ser declarada ante la SEC por un instalador eléctrico autorizado, quien certifica que todas las protecciones están correctamente instaladas.


Un sistema solar sin las protecciones termomagnéticas requeridas no pasa la inspección de la SEC. Sin la aprobación de la SEC, no puedes inscribirte en[Net Billing](https://ruuf.cl/blog/net-billing-en-chileno) con tu distribuidora.


Y sin Net Billing, no puedes inyectar excedentes ni recibir descuento en la boleta. Las protecciones no son un "extra". Son un requisito legal y técnico sin el cual tu sistema no es seguro ni está legalizado.


## **Cómo saber si tu sistema está bien protegido**


Si ya tienes paneles solares instalados, o si estás evaluando una propuesta de instalación, hay cosas concretas que puedes verificar.


### **En tu tablero eléctrico**


Abre la tapa de tu tablero (con cuidado, sin tocar nada adentro). Deberías ver:


-


Un automático (disyuntor termomagnético) identificado como el circuito solar o fotovoltaico. Puede estar etiquetado como "FV", "Solar" o "Inversor".


-


Un diferencial dedicado al circuito solar (puede ser un dispositivo separado o combinado con el automático).


-


Estos elementos deberían estar separados de los automáticos de los circuitos normales de tu casa (enchufes, luces, cocina).


Si no ves ningún automático dedicado al sistema solar, o si el inversor está conectado directamente a un circuito existente sin protección propia, hay un problema.


### **En la caja de protecciones CC (cerca del inversor o en el techo)**


Debería haber una caja o gabinete con:


-


Un disyuntor termomagnético CC (o seccionador CC) que permita desconectar los paneles del inversor.


-


En sistemas con múltiples strings: fusibles por string.


Si el cableado de los paneles llega directamente al inversor sin ninguna protección intermedia, hay un problema.


### **En la propuesta de instalación**


Si estás evaluando una cotización, busca que mencione explícitamente:


-


Disyuntor termomagnético CC (con voltaje y corriente nominal especificados)


-


Disyuntor termomagnético CA (con amperaje especificado)


-


Disyuntor diferencial (con sensibilidad en mA)


-


Protección de sobretensión (deseable, especialmente si estás en zona de tormentas)


-


Puesta a tierra


Si la propuesta solo dice "paneles + inversor + instalación" sin detallar protecciones, pregunta. Y si la respuesta es vaga, busca otra empresa.


## **Errores comunes con las protecciones termomagnéticas**


### **Usar un disyuntor de CA en un circuito CC**


Es el error más peligroso. Los disyuntores de corriente alterna no pueden extinguir los arcos de corriente continua de forma segura.


Si se produce un cortocircuito en el lado CC y el disyuntor no está diseñado para ese tipo de corriente, puede no cortar la falla, fundirse internamente o incluso provocar un incendio.


Los disyuntores CC tienen cámaras de extinción de arco más grandes y un mecanismo diseñado específicamente para la corriente continua. No son intercambiables.


### **Subdimensionar el disyuntor**


Un disyuntor con una corriente nominal muy baja para el sistema se va a disparar constantemente durante la operación normal.


Un disyuntor con una corriente nominal muy alta no va a proteger adecuadamente contra sobrecargas. El dimensionamiento correcto se hace con la corriente de cortocircuito de los paneles (Isc), multiplicada por 1,25 como factor de seguridad.


### **No instalar seccionamiento CC**


Algunos instaladores omiten el disyuntor CC y conectan los paneles directamente al inversor. Esto es un problema porque no hay forma segura de desconectar los paneles para mantenimiento.


Mientras haya luz solar, los paneles generan voltaje, y sin un seccionador no puedes "apagarlos" desde abajo.


### **No verificar la capacidad de ruptura**


La capacidad de ruptura (kA) es la corriente máxima de cortocircuito que el disyuntor puede interrumpir de forma segura. Si la corriente de falla supera la capacidad de ruptura del disyuntor, este no logra cortar la corriente y puede destruirse.


En sistemas solares residenciales, una capacidad de ruptura de 6 kA es generalmente suficiente, pero debe verificarse caso a caso.


## **¿Cuánto cuestan las protecciones termomagnéticas?**


Las protecciones eléctricas son una fracción mínima del costo total del sistema, pero su impacto en la seguridad es enorme. Como referencia para el mercado chileno:


-


**Disyuntor termomagnético CC 2P (500V, 16-32A):** $15.000 a $25.000 CLP


-


**Disyuntor termomagnético CA 2P (220V, 20-32A):** $8.000 a $18.000 CLP


-


**Disyuntor diferencial 2P 30mA:** $25.000 a $50.000 CLP


-


**Fusibles gPV 10x38 + portafusibles:** $5.000 a $12.000 CLP por string


-


**Protector de sobretensión CC:** $30.000 a $60.000 CLP


-


**Protector de sobretensión CA:** $20.000 a $40.000 CLP


El conjunto completo de protecciones para un sistema residencial típico cuesta entre $80.000 y $150.000 CLP.


En un sistema de $5.000.000, eso es menos del 3% del costo total. Omitirlo para "ahorrar" es una decisión que no tiene ningún sentido económico ni técnico.


## **Lo que hacemos en RUUF con las protecciones de tu sistema**


En RUUF, las protecciones eléctricas no son un ítem negociable ni un "extra" que se agrega si sobra presupuesto. Son parte integral de cada instalación, sin excepción.


### **Protecciones estándar en toda instalación RUUF**


-


Disyuntor termomagnético CC dimensionado para el voltaje y corriente de tus strings


-


Disyuntor termomagnético CA bipolar dedicado en el tablero eléctrico


-


Disyuntor diferencial en el circuito solar


-


Puesta a tierra completa de la estructura y equipos


-


Protección de sobretensión cuando las condiciones lo requieren


### **Instalación por eléctricos certificados SEC**


Nuestros instaladores son eléctricos autorizados por la SEC que verifican cada protección, cada conexión y cada calibre de cable antes de poner el sistema en marcha.


La declaración TE4 que presentamos ante la SEC certifica que la instalación cumple con todas las normas de seguridad vigentes.


### **Los números de RUUF**


-


Más de 1.800 instalaciones en más de 170 comunas de Chile


-


Rating de 4.8 basado en evaluaciones reales


-


Ahorro promedio de $120.000 mensuales por cliente (a julio de 2026)


-


100% enfocados en energía solar residencial


## **Conclusión: el disyuntor termomagnético es la protección que no ves pero que siempre está trabajando**


El disyuntor termomagnético no es el componente más emocionante de un sistema solar.


Pero es el que te protege cuando algo sale mal. Y en un sistema eléctrico que opera durante 25 años en tu techo, bajo sol, lluvia, viento y variaciones de temperatura, las cosas eventualmente pueden salir mal.


Un disyuntor CC correctamente dimensionado protege tus paneles y tu inversor. Un disyuntor CA correctamente instalado protege tu tablero y tu casa. Un diferencial protege a tu familia.


Y todo esto junto es lo que la SEC exige para aprobar tu instalación y permitir que inyectes excedentes a la red.


No es un gasto. Es la base mínima de una instalación segura. Y si tu instalador no le da importancia a las protecciones, probablemente no le da importancia a otras cosas que tampoco deberías pasar por alto.


¿Quieres un sistema solar instalado con todas las protecciones que corresponden, por gente que sabe de electricidad de verdad?[Cotiza con RUUF en menos de 5 minutos](https://get.ruuf.cl/onboarding) y te mostramos un diseño personalizado basado en tu consumo real y las condiciones de tu casa.
