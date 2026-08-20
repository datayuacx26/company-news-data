---
schema_version: "1.0.0"
document_id: "690d6e538b922304eb93e9fd236aa130c1681229164dc07cd0a9267eb86c8d21"
company_key: "yc-palenca"
company: "Palenca"
source_id: "yc-palenca-rss-c9f8371bccc6"
canonical_url: "https://blog.palenca.com/como-verificar-ingresos-de-trabajadores-gig-para-originacion-de-credito-en-mexico/"
published_at: "2026-04-21T16:17:08+00:00"
first_seen_at: "2026-07-25T18:13:53.501952+00:00"
fetched_at: "2026-07-28T20:52:23.948620+00:00"
content_hash: "sha256:14a0e066019ee54938ee809617d9a5cba5812b8bd08b3bbce0d96c815290621c"
---

# Cómo verificar ingresos de trabajadores gig para originación de crédito en México

*5 minutos de lectura*


---


Más del 56% de la fuerza laboral en México trabaja en la economía informal, según datos del INEGI. Una proporción creciente de esos trabajadores genera ingresos de forma consistente y verificable a través de plataformas gig como Uber, Rappi, DiDi o inDrive, pero no tienen recibo de nómina, no cotizan al IMSS y no aparecen en el historial de buró de crédito tradicional.


Para un prestamista, esto plantea un problema concreto: **¿cómo verificas los ingresos de alguien que trabaja en Uber si no existe ningún documento oficial que los respalde?**


La respuesta está en ir directamente a la fuente. Las plataformas gig tienen registros exactos de cuánto ganó cada conductor o repartidor, cuántos viajes completó y durante cuánto tiempo ha estado activo. En este artículo te mostramos cómo acceder a esos datos mediante la API de Palenca, calcular el ingreso mensual promedio y usarlo como parte de tu proceso de originación de crédito.


---


## El problema con la verificación manual de ingresos gig


El proceso tradicional de verificación de ingresos para trabajadores formales asume que existe un empleador que emite comprobantes. Para un trabajador gig esto no aplica:


- **No hay recibo de nómina** — Las plataformas pagan directamente a la cuenta bancaria sin emitir comprobante fiscal
- **Los estados de cuenta no distinguen fuente** — Un depósito de Uber aparece como transferencia de tercero
- **Las declaraciones de impuestos son inconsistentes** — Muchos conductores no declaran o lo hacen de forma irregular
- **Las capturas de pantalla son manipulables** — El historial de ganancias de la app del conductor puede falsificarse fácilmente


El resultado: los prestamistas o rechazan estos clientes (perdiendo mercado) o asumen el riesgo de aprobar sin datos verificados (perdiendo en cartera vencida).


---


## La solución: verificación directa vía API


Palenca se conecta directamente a las plataformas gig mediante los mismos mecanismos que usa el trabajador para acceder a su cuenta. En lugar de pedirle al conductor que traiga un documento, le pides que autorice el acceso una sola vez y obtienes datos estructurados, verificables y actualizados en tiempo real.


**El flujo completo toma menos de 2 minutos:**


1. El trabajador proporciona su número de teléfono (o email) y código de verificación de la plataforma gig
2. Palenca autentica y extrae los datos directamente de la plataforma
3. Tu sistema recibe un webhook con el resultado
4. Consultas los endpoints de ingresos para obtener el historial estructurado


No hay documentos, no hay revisión manual, no hay fraude.


---


## Implementación paso a paso


### Paso previo: el trabajador conecta su cuenta vía Widget


Antes de poder consultar datos, el trabajador debe autorizar el acceso a través del **Widget de Palenca** , que se embebe directamente en tu flujo de solicitud de crédito. El Widget maneja toda la autenticación: el trabajador ingresa su teléfono/email y contraseña de la plataforma gig, y Palenca lo conecta de forma segura.


Una vez completado, tu sistema recibe un webhook` login.success` con el` account_id` que usarás en todos los pasos siguientes. Consulta la integración del Widget en[developers.palenca.com](https://developers.palenca.com/?ref=blog.palenca.com) .


### Paso 1: Obtener el historial de ingresos


Una vez que la cuenta está conectada y activa (recibirás un webhook` login.success` ), puedes consultar el historial de ingresos:


```text
import requests
from datetime import datetime, timedelta


def obtener_ingresos(account_id: str, meses: int = 6) -> dict:
"""Obtiene el historial completo de ingresos de los últimos N meses (todas las páginas)."""
fecha_fin = datetime.today().strftime("%Y-%m-%d")
fecha_inicio = (datetime.today() - timedelta(days=30 * meses)).strftime("%Y-%m-%d")


todos_los_ingresos = []
page = 1


while True:
response = requests.post(
f"{BASE_URL}/accounts/{account_id}/earnings/search",
headers=headers,
json={
"start_date": fecha_inicio,
"end_date": fecha_fin,
"options": {"page": page, "items_per_page": 100}
}
)
response.raise_for_status()
data = response.json()
todos_los_ingresos.extend(data["data"]["earnings"])


pagination = data.get("pagination", {})
if page >= pagination.get("total_pages", 1):
break
page += 1


return {"data": {"earnings": todos_los_ingresos}}


ingresos = obtener_ingresos(account_id, meses=6)


```


**Respuesta JSON de muestra:**


```text
{
"success": true,
"error": null,
"data": {
"account_id": "acc_01HX3K8MNPQ2R4S5T6U7V8W9X0",
"earnings": [
{
"earning_date": "2024-01-15",
"amount": 2340.50,
"currency": "mxn",
"count_trips": 18,
"cash_amount": null
},
{
"earning_date": "2024-01-08",
"amount": 1890.00,
"currency": "mxn",
"count_trips": 14,
"cash_amount": null
},
{
"earning_date": "2023-12-23",
"amount": 3120.75,
"currency": "mxn",
"count_trips": 24,
"cash_amount": null
}
]
},
"pagination": {
"page": 1,
"items_per_page": 100,
"total_items": 48,
"total_pages": 1
}
}


```


---


## Paso 2: Calcular el ingreso mensual promedio


Con los datos de ingresos ya disponibles, calculas el ingreso mensual promedio para la decisión crediticia:


```text
from collections import defaultdict


def calcular_ingreso_mensual_promedio(ingresos_data: dict) -> dict:
"""
Calcula el ingreso mensual promedio y estadísticas de consistencia.


Retorna:
- promedio_mensual: Promedio de ingresos por mes
- meses_activos: Número de meses con actividad registrada
- desviacion: Variabilidad de ingresos mes a mes
- ingreso_minimo: El mes de menor ingreso (para worst-case)
"""
totales_mensuales = defaultdict(float)


for ingreso in ingresos_data.get("data", {}).get("earnings", []):
mes = ingreso["earning_date"][:7]  # "2024-01"
totales_mensuales[mes] += ingreso["amount"]


if not totales_mensuales:
return {"promedio_mensual": 0, "meses_activos": 0}


valores = list(totales_mensuales.values())
promedio = sum(valores) / len(valores)
ingreso_minimo = min(valores)


return {
"promedio_mensual": round(promedio, 2),
"meses_activos": len(totales_mensuales),
"ingreso_minimo": round(ingreso_minimo, 2),
"consistencia_pct": round((ingreso_minimo / promedio) * 100, 1)
}


metricas = calcular_ingreso_mensual_promedio(ingresos)
print(f"Ingreso promedio mensual: ${metricas['promedio_mensual']:,.2f} MXN")
print(f"Meses activos: {metricas['meses_activos']}")
print(f"Ingreso mínimo: ${metricas['ingreso_minimo']:,.2f} MXN")
print(f"Consistencia: {metricas['consistencia_pct']}%")
# Output:
# Ingreso promedio mensual: $9,420.00 MXN
# Meses activos: 6
# Ingreso mínimo: $7,800.00 MXN
# Consistencia: 82.8%


```


---


## Qué hacer con estos datos en tu proceso de crédito


Una vez que tienes el ingreso mensual promedio verificado, puedes usarlo como parte de tu modelo de decisión:


Señal Descripción Uso sugerido


` promedio_mensual` Ingreso promedio de los últimos 6 meses Base para cálculo de capacidad de pago


` meses_activos` Meses con ingresos registrados Indicador de estabilidad laboral


` ingreso_minimo` Mes de menor ingreso Estimación conservadora para cuota máxima


` consistencia_pct` Ratio mínimo/promedio Indicador de variabilidad de ingresos


` count_trips` Viajes por periodo Proxy de nivel de actividad


Una regla de decisión simple:
-` meses_activos >= 3` y` consistencia_pct >= 60%` → Ingreso verificable
-` promedio_mensual * 0.3` → Cuota máxima mensual sugerida (30% de ingresos)


---


## Plataformas soportadas en México


Palenca soporta las principales plataformas gig en México, lo que significa que puedes verificar ingresos sin importar en cuál opera tu solicitante:


Plataforma Identificador Datos disponibles


Uber` uber` Ingresos, viajes, perfil, antigüedad


Rappi` rappi` Ingresos, entregas, perfil


DiDi` didi` Ingresos, viajes, perfil


DiDi Food` didi_food` Ingresos, entregas, perfil


inDriver` indriver` Ingresos, viajes, perfil


---


## Tiempo de integración y ambiente sandbox


La API de Palenca tiene un ambiente sandbox disponible para que puedas probar la integración sin conectar cuentas reales:


- **Sandbox URL** :` https://sandbox.palenca.com/v1`
- **Producción URL** :` https://api.palenca.com/v1`
- **Tiempo estimado de integración** : 1–3 días de desarrollo
- **Documentación completa** :[docs.palenca.com](https://docs.palenca.com/?ref=blog.palenca.com)


El proceso de autorización del trabajador se integra en tu flujo de solicitud usando el Widget de Palenca. Consulta la documentación completa en[developers.palenca.com](https://developers.palenca.com/?ref=blog.palenca.com) .


---


## Resumen


Para verificar ingresos de trabajadores gig en México para originación de crédito, los pasos son:


1. **El trabajador conecta su cuenta gig** a través del Widget de Palenca (embebido en tu flujo)
2. **Consultar el historial de ingresos** con el endpoint de búsqueda de ingresos al recibir el webhook` login.success`
3. **Calcular métricas** de promedio mensual, consistencia y mínimo para tu modelo de decisión
4. **Tomar la decisión** con datos verificados directamente de la fuente


El código completo de este artículo está disponible en nuestro repositorio de ejemplos:[github.com/palenca/blog-code-examples/income-verification-mexico](https://github.com/palenca/blog-code-examples?ref=blog.palenca.com)


---


¿Listo para integrar? Crea tu cuenta en el sandbox de Palenca y prueba estos ejemplos en minutos.[Solicita acceso al sandbox →](https://palenca.com/?ref=blog.palenca.com)


---


*Continúa aprendiendo:*
-Cómo construir un score crediticio para trabajadores informales usando datos de plataforma
-Palenca vs verificación manual de ingresos: qué cambia para tu equipo de operaciones
