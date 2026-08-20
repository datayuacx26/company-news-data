---
schema_version: "1.0.0"
document_id: "5abd3f9e796cc308f59928207425c8e851cf103d93fcbc39352d8513a64addd1"
company_key: "yc-fintoc"
company: "Fintoc"
source_id: "yc-fintoc-news-import-4ac2d54454f2"
canonical_url: "https://www.fintoc.com/cl/blog/presentando-la-cli-de-fintoc"
published_at: null
first_seen_at: "2026-07-21T22:44:19.627198+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:4dfaace173478d8711f75b77521b30ba5867d65b63546d853e305a03f8f3c4c3"
---

# Presentando la CLI de Fintoc

Muchos usuarios usan la documentación y nuestra API para automatizar su operación. Hoy la API por si sola no es suficiente: necesitábamos una forma de que un agente AI fácilmente pueda hacer tu integración por ti.


Cuando integras Fintoc, buena parte del trabajo no es escribir código nuevo, sino ir a revisar si un pago fue exitoso, ver qué le llegó a tu webhook handler, o crear un cargo de prueba para reproducir un caso. Eso, en la práctica, es saltar entre la terminal, el editor, el dashboard y la documentación, y mantener un túnel ngrok corriendo aparte para que los webhooks lleguen a tu local.


Hoy lanzamos la[CLI de Fintoc](https://docs.fintoc.com/docs/cli) : una herramienta que junta todo eso en un solo lugar. La puedes usar tú para construir y debuggear desde la terminal; o se la das a tu agente de IA para que integre Fintoc por ti — sin copiar datos entre el dashboard, las docs y tu código.


## Validar y debuggear desde el terminal


Cuando estás integrando algo, la mayoría de las veces estás revisando el estado de un recurso, reproducir un caso, ajustar una llamada. Antes eso significaba escribir un mini-script o saltar al dashboard. Ahora es un comando:


```text
$ fintoc payment_intents get pi_test_abc123
$ fintoc charges list --status failed --since 2026-05-01
$ fintoc charges create \
--amount 5000 --currency CLP \
--subscription-id sub_test_abc123
```


Cualquier recurso de la API pública se puede listar, leer, crear y (en algunos casos) expirar y borrar desde la terminal. Las operaciones que mueven dinero, como crear transferencias, siguen teniendo una capa adicional de seguridad, requiriendo además una[llave JWS configurada](https://docs.fintoc.com/docs/setting-up-jws-keys) para firmar la operación.


Si necesitas crear un recurso con un payload complejo, pásalo como JSON. Los flags individuales tienen prioridad sobre las keys del JSON, así que es fácil parametrizar:


```text
$ cat payload.json | fintoc charges create --from-json -
$ fintoc charges create \
--from-json payload.json --amount 7500
```


## Probar webhooks en local, fácil y rápido


Probar webhooks en local siempre ha sido más complicado de lo que nos gustaría. Hay que levantar algún túnel como ngrok, actualizar la URL del endpoint cada vez que reinicias, además de inscribir cada uno de los endpoints para probar recibirlos.


Con la CLI, no necesitas configurar ningún webhook endpoint en el Dashboard, haces forwarding opcionalmente y directo desde tu terminal.


` fintoc webhooks listen` recibe los webhooks reales de Fintoc directamente en tu` localhost` . La firma viene incluida en los headers como en cualquier evento y el Webhook Secret no cambiará entre reinicios al comando` listen` . Así puedes validar la integración end-to-end desde el primer evento.


```text
$ fintoc webhooks listen \
--forward-to http://localhost:3000/webhooks
ℹ Listening for webhooks. Your webhook signing secret \
is whsec_test_4e07408562bedb8b60ce05c1
```


## Para que tu agente integre por ti


Con la CLI habilitamos que un agente como Claude, Cursor o ChatGPT pueda integrar Fintoc por ti sin tener que ir copiando` id` s entre tu editor y el dashboard ni leer docs en otra ventana. Cada flag está pensado para que para un agente sea intuitivo de usar:


‍


- ` --json` para devolver JSON en vez de tablas
- ` --api-key` para overrides puntuales sin tocar tu config local.
- ` fintoc <command> --help` documenta cada flag en cada subcomando, así un agente descubre capacidades sin leer docs externas.


Combinado con` jq` u otras herramientas de shell,` --json` te deja armar pipelines simples para tareas que antes requerían flujos más complejos.` list` con` --json` devuelve directamente el array:


```text
$ fintoc charges list --json \
--status failed --since 2026-05-01 | \
jq -r '.[] | [.id, .amount, .subscription_id] | @csv'
```


## Empieza hoy


Vía npm (requiere Node.js 22+)


```text
npm install -g @fintoc/cli
```


O vía Homebrew:


```text
brew install fintoc-com/tap/fintoc
```


Revisa los docs para aprender cómo usarla en[docs.fintoc.com/docs/cli](http://docs.fintoc.com/docs/cli) .
