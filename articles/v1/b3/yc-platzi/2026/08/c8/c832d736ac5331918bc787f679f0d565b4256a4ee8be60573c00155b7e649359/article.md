---
schema_version: "1.0.0"
document_id: "c832d736ac5331918bc787f679f0d565b4256a4ee8be60573c00155b7e649359"
company_key: "yc-platzi"
company: "Platzi"
source_id: "yc-platzi-news-import-15d3b242e96e"
canonical_url: "https://platzi.com/blog/instala-el-modelo-abierto-de-openai-gpt-oss/"
published_at: null
first_seen_at: "2026-08-04T07:49:07.734297+00:00"
fetched_at: "2026-08-04T09:43:50.004226+00:00"
content_hash: "sha256:34f802836a96456829cc253afd6f8cbd43117b9f7bdf62f1a3f1809e5d2503c2"
---

# Instala el modelo abierto de OpenAI - GPT-OSS

A principios de año les contábamos[cómo instalar DeepSeek](https://platzi.com/blog/deepseek-r1-instalar-local/) , el modelo armado en China que revolucionaba por ese entonces al mundo. Un modelo “económico”, que se podía correr en “cualquier” laptop o computadora personal.


Ahora volvemos al ruedo, de la mano de OpenAI para instalar su nuevo modelo que acaba de salir al mercado y promete una performance similar o mejora a modelos que hasta hace muy poco, eran de pago.


---


## 1. ¿Qué es GPT-OSS?


GPT-OSS es un modelo LLM open source lanzado por OpenAI. Su nombre “OSS” viene de “Open Source Series”, y representa el primer paso oficial de OpenAI en el mundo de los modelos de código abierto.


## 2. ¿Por qué GPT-OSS es especial?


GPT‑OSS representa un hito para OpenAI: es su primer modelo con pesos abiertos (open‑weight) desde GPT‑2, liberado tras casi seis años de estrategia cerrada.
Ofrece dos variantes: gpt-oss-120b, con cerca de 120 mil millones de parámetros, que iguala o supera el rendimiento de su modelo propietario o4‑mini en tareas de razonamiento, codificación y salud; y gpt‑oss-20b, más compacto, optimizado para correr en PCs o laptops con solo 16 GB de memoria, pero que aun así iguala o supera el desempeño del o3‑mini en benchmarks clave.


Ambos modelos están disponibles bajo licencia Apache 2.0, lo que permite su uso comercial, inspección, redistribución y ajustes personalizados. También incluyen soporte para razonamiento paso a paso (chain‑of‑thought), uso de herramientas (como búsqueda web o ejecución de código Python), y configuraciones ajustables de intensidad de razonamiento para optimizar entre velocidad y calidad. Además, han sido sometidos a evaluaciones de seguridad rigurosas, incluyendo simulaciones de uso adversarial y revisión por expertos externos, alcanzando niveles de seguridad similares a los modelos propietarios de OpenAI.


---


## 3. Requisitos previos para instalar y correr GPT-OSS


- Conocimientos básicos de la terminal o línea de comandos.
- Tener un sistema Linux/macOS (o en Windows con WSL2).
- [Ollama](https://ollama.ai/) para descargar y correr el modelo localmente (igualmente en este blog te ayudo a instalarlo).


---


## 4. Instalando Ollama


[Ollama](https://ollama.ai/) es una herramienta que permite gestionar y correr modelos de lenguaje localmente. Es como un “docker” de modelos de AI


1. **Instalación** :


**en Linux**


```text
curl -fsSL https://ollama.com/install.sh | sh
ollama -v  # Verificá la versión instalada


```


**o en MAC**


```text
brew install ollama
brew services start ollama  # Inicia el servicio de ollama con brew para no tener que ejecutar ´ollama serve´
ollama -v  # Verificá la versión instalada


```


Si ya tienes instalado Ollama, vas a necesitar actualizarlo. Siguiendo los mismos pasos en linux para instalar o en MAC:


```text
brew   reinstall ollama
brew   services start ollama


```


1. **Descarga de GPT-OSS con Ollama**
Elegí el modelo que se ajuste a tu GPU (o CPU).


-


**20B parámetros** (aprox. 14GB):


```text
ollama run gpt‑oss:20b


```


-


**Versión completa 120B** :


```text
ollama run gpt‑oss:latest


```


---


## 5. Probando la api


Una vez que Ollama descargó el modelo y tenés la interfaz corriendo (opcional), ya podés empezar a hacerle preguntas desde la terminal o desde la UI. Para la terminal:


```text
ollama run gpt-oss-20b


```


Y luego puedes probar el API con el siguiente código en python:


```text
from openai  import   OpenAI


client   = OpenAI(
base_url="http://localhost:11434/v1",     # La IP de Ollama
api_key="platzi"                          # Cualquier Key
)


response   = client.chat.completions.create(
model="gpt-oss:20b",
messages=[
{ "role"  :  "system"  ,  "content"  :  "You are a helpful assistant."  },
{ "role"  :  "user"  ,  "content"  :  "Saludame de una forma divertida!!!!"  }
]
)


print(response.choices[ 0  ].message.content)```


```


---


## 6. Conclusiones sobre el uso de GPT-OSS


- Usá un modelo de código abierto respaldado por OpenAI, con rendimiento comparable a modelos comerciales.
- Evitá depender de APIs externas: podés correrlo en tu propia infraestructura, incluso en una laptop.
- Personalizalo a tu gusto: la licencia Apache 2.0 te da libertad total para ajustarlo y redistribuirlo.
- Con soporte para razonamiento paso a paso, ideal para tareas complejas de lógica, programación y ciencia.


Además, su foco en seguridad y control lo convierte en una excelente opción si buscás equilibrio entre potencia, privacidad y autonomía. Probalo en tu próximo proyecto y vas a ver por qué GPT‑OSS es mucho más que “otro modelo open source”.


## Continúa aprendiendo sobre inteligencia artificial


También puedes explorar la **[Escuela de Data Science e Inteligencia Artificial](https://platzi.com/escuela/datos/)** en donde encontrarás rutas de aprendizaje sobre:


- [Modelos de Difusión: Generación de Imágenes con AI](https://platzi.com/ruta/generacion-imagenes-ia/)
- [Herramientas de AI para Programadores](https://platzi.com/ruta/datos-ia-devs/)
- [AI: Desarrollo de Apps con LLMs](https://platzi.com/ruta/datos-llm/)


---


## Referencias y lecturas recomendadas sobre GPT-OSS y su instalación


- [Ollama: instalación y documentación](https://ollama.ai/)
- [Presentacion de GPT-OSS](https://openai.com/es-419/index/introducing-gpt-oss/)
