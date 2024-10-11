+++
title = 'Obtener la transcripción de videos de YouTube'
date = 2024-09-28
categories = ["python"]
tags = ["programación", "python", "youtube"]
draft = false
+++

En el post anterior explicaba una opción para descargar - con código python - el video (mp4) a partir de un enlace (url) de YouTube, usando la librería  [**pytubefix**](https://pypi.org/project/pytubefix/). 

Eso de tener el video en el disco duro está bien, pero últimamente estoy intentando hacer RAG locales usando modelos LLM de inteligencia artificial usando  Ollama y AnythingLLM. Para estos casos es súper util tener ficheros de texto con conocimiento específico de un tema en soporte local sobre los que se pasa un modelo de embbedding. Y qué mejor forma de obtener mucho conocimiento, -de cualquier tema- que usando la fuente de toda sabiduría moderna: YouTube!!.😂

Vamos a la faena, lo que busco es descargar las transcripciones de videos seleccionados de Youtube y para eso hay al menos dos formas:

 1. [instalar un pluggin en el navegador](#instalar-un-pluggin-en-el-navegador)
 2.[ mediante código python](#obtener-la-transcripción-por-código)


# Instalar un pluggin en el navegador
En Chrome hay algunas extensiones como la llamada "*YouTube Summary with ChatGPT & Claude*" que es muy útil y te permite obtener directamente la transcripción desde el navegador, y si lo conectas a tu cuenta e OpenAI o de Claude de Ahthropic puede hasta hacer un resumen de dicha transcripción con lo que se le puede sacar jugo.


# Obtener la transcripción por código.
Si buscamos algo más automatizado, que nos permita descargar no solo la transcripción de un video, sino de todos los videos de una lista, lo tenemos que hacer con python. Yo he encontrado esta forma usando llama_hub que me ha funcionado muy bien, y que en segundos me permite obtener cientos de transcripciones de videos ordenados en una carpeta.

Las transcripciones tienen sus problemas, ya que hablando repetimos muchas palabras, no existen puntuaciones de ortografía con lo que las frases se juntan sin fin, otras veces es la censura de YouTube con ciertas palabras el problema ya que he visto casos escandalosos, por ejemplo el quitar la palabra "negro", porque debe entender que es racista referirse al color oscuro 😂😂, en casos como vacuna, Covid etc, la censura es máxima y las quita directamente de la transcripción, por lo que depende el tema que trates, puede que debas hacer un repaso semiautomático para corregir muchas de estas cosas raras que nos dejan las compañías grandes que marcan la moral de la nueva sociedad.

Centrandonos en el código, voy a dejar dos sripts, el primero para descargar un solo video a partir de su url de youtube, y el segundo para descarga de una lista completa debiendo proporcionar la url de la lista y almacena las transcripciones en texto plano en una carpeta aparte.

## Descargar transcripcion de un video
Verás que hacemos uso de la librería `pytubefix` para obtener el título del video y renombrar con él el fichero de salida, que es un markdown, de texto simple.

Si los videos no los quieres en español, sino en la versión original debes quitar esto: ` language='es'`, o cambiarlo por el idioma correspondiente.

Pues el código para descargar una trascripción es este. En las ultimas líneas es donde debes poner la url del video en cuestión:

```python
# Hay que tener instalado:
# pip install pytubefix llama-hub llamaIndex
# https://llamahub.ai/l/readers/llama-index-readers-youtube-transcript?from=readers
import os
from pytubefix import YouTube
from llama_index.readers.youtube_transcript import YoutubeTranscriptReader

# Función para obtener el título del video de YouTube
def obtener_titulo_youtube(url):
    try:
        video = YouTube(url)
        return video.title
    except Exception as e:
        print(f"Error al obtener el título del video: {e}")
        return None

# Función para obtener la transcripción del video de YouTube
def obtener_transcripcion_youtube(url, language='es'):
    try:
        # Usar YoutubeTranscriptReader para obtener la transcripción
        loader = YoutubeTranscriptReader()
        transcript = loader.load_data(ytlinks=[url], languages=[language])
        return transcript
    except Exception as e:
        print(f"Error al obtener la transcripción del video: {e}")
        return None

# Función para guardar la transcripción en un archivo Markdown
def guardar_transcripcion_en_markdown(titulo, transcripcion):
    # Crear un nombre de archivo basado en el título, eliminando caracteres no válidos
    nombre_archivo = f"{titulo}.md".replace("/", "-").replace("\\", "-")
    
    # Crear el contenido en formato Markdown
    contenido_md = f"# Transcripción del video: {titulo}\n\n"
    
    # Iterar sobre los objetos Document y extraer el texto
    for doc in transcripcion:
        contenido_md += f"- {doc.text}\n"
    
    # Guardar el contenido en un archivo
    with open(nombre_archivo, "w", encoding="utf-8") as file:
        file.write(contenido_md)
    
    print(f"Transcripción guardada en {nombre_archivo}")

# Función para imprimir la estructura de la transcripción
def mostrar_componentes_transcripcion(transcripcion):
    print("\n--- Componentes de la transcripción ---\n")
    for idx, doc in enumerate(transcripcion):
        print(f"Documento {idx + 1}:")
        print(f"Texto: {doc.text}")
        print(f"Metadatos: {doc.metadata}")
        print("---------------------------\n")

# Función principal
def procesar_video_youtube(url):
    # Obtener el título del video
    titulo = obtener_titulo_youtube(url)
    if titulo:
        print(f"Título del video: {titulo}")
        
        # Obtener la transcripción del video
        transcripcion = obtener_transcripcion_youtube(url)
        if transcripcion:
            # Mostrar componentes de la transcripción
            mostrar_componentes_transcripcion(transcripcion)
            
            # Guardar la transcripción en un archivo Markdown
            guardar_transcripcion_en_markdown(titulo, transcripcion)
        else:
            print("No se pudo obtener la transcripción.")
    else:
        print("URL de video no válida o no se pudo obtener el título.")

# Ejemplo de uso
url_video = "https://youtu.be/YL0xdkAC5eY?si=sAB7avi_3er1ORGV"
procesar_video_youtube(url_video)

```

## Descargar transcripcion de una lista de videos
Lo bueno del código es que podemos automatizar esto y descargar de una tirada toda una lista de videos de youtube. esto como os dije me viene genial para crear conocimiento sobre un tema específico, no tengo más que pasar la url de una lista que contenga videos de la materia y esperar unos segundos. 
El script que he hecho guarda cada lista en una subcarpeta para que no se mezclen ya que son muchos y les pone a los ficheros de texto el nombre del título del video, quitando algunos símbolos que pueden dar error.

Además he añadido un control de errores para que si falla la bajada de alguna transcripción sigua con el siguiente video y no se pare.

La función `mostrar_componentes_transcripcion()` solo la he puesto para llevar un control de lo que va descargando, así voy viendo en directo lo que baja, pero se puede quitar sin problemas.

```python
# Hay que tener instalado
# pip install pytubefix llama-hub
# https://llamahub.ai/l/readers/llama-index-readers-youtube-transcript?from=readers
# https://pytubefix.readthedocs.io/en/latest/user/playlist.html
# Este script sirve para descargar las transcripciones de todos los videos de 
# una lista de youtube, proporcionando la url de la lista.
# Fernando Villalba 

import os
from pytubefix import YouTube, Playlist
from llama_index.readers.youtube_transcript import YoutubeTranscriptReader

# Función para obtener el título del video de YouTube
def obtener_titulo_youtube(url):
    try:
        video = YouTube(url)
        return video.title
    except Exception as e:
        print(f"Error al obtener el título del video {url}: {e}")
        return None

# Función para obtener la transcripción del video de YouTube
def obtener_transcripcion_youtube(url, language='es'):
    try:
        # Usar YoutubeTranscriptReader para obtener la transcripción
        loader = YoutubeTranscriptReader()
        transcript = loader.load_data(ytlinks=[url], languages=[language])
        return transcript
    except Exception as e:
        print(f"Error al obtener la transcripción del video {url}: {e}")
        return None

# Función para guardar la transcripción en un archivo Markdown dentro del subdirectorio de la lista
def guardar_transcripcion_en_markdown(titulo, transcripcion, subdirectorio):
    # Crear un nombre de archivo basado en el título, eliminando caracteres no válidos
    nombre_archivo = f"{titulo}.md".replace("/", "-").replace("\\", "-")
    
    # Crear el contenido en formato Markdown
    contenido_md = f"# Transcripción del video: {titulo}\n\n"
    
    # Iterar sobre los objetos Document y extraer el texto
    for doc in transcripcion:
        contenido_md += f"- {doc.text}\n"
    
    # Crear el subdirectorio si no existe
    if not os.path.exists(subdirectorio):
        os.makedirs(subdirectorio)
    
    # Guardar el archivo en el subdirectorio
    ruta_archivo = os.path.join(subdirectorio, nombre_archivo)
    with open(ruta_archivo, "w", encoding="utf-8") as file:
        file.write(contenido_md)
    
    print(f"Transcripción guardada en {ruta_archivo}")

# Función para imprimir la estructura de la transcripción
def mostrar_componentes_transcripcion(transcripcion):
    print("\n--- Componentes de la transcripción ---\n")
    for idx, doc in enumerate(transcripcion):
        print(f"Documento {idx + 1}:")
        print(f"Texto: {doc.text}")
        print(f"Metadatos: {doc.metadata}")
        print("---------------------------\n")

# Función para procesar un video de YouTube
def procesar_video_youtube(url, subdirectorio):
    try:
        # Obtener el título del video
        titulo = obtener_titulo_youtube(url)
        if titulo:
            print(f"Título del video: {titulo}")
            
            # Obtener la transcripción del video
            transcripcion = obtener_transcripcion_youtube(url)
            if transcripcion:
                # Mostrar componentes de la transcripción
                mostrar_componentes_transcripcion(transcripcion)
                
                # Guardar la transcripción en un archivo Markdown dentro del subdirectorio
                guardar_transcripcion_en_markdown(titulo, transcripcion, subdirectorio)
            else:
                print(f"No se pudo obtener la transcripción del video: {url}")
        else:
            print(f"URL de video no válida o no se pudo obtener el título para {url}.")
    except Exception as e:
        print(f"Error al procesar el video {url}: {e}")

# Función para crear un subdirectorio único para la lista de reproducción
def crear_subdirectorio_lista(nombre_base="lista"):
    contador = 1
    subdirectorio = nombre_base
    
    # Verificar si el subdirectorio ya existe y, si es así, crear uno numerado
    while os.path.exists(subdirectorio):
        subdirectorio = f"{nombre_base}{contador:02d}"
        contador += 1
    
    os.makedirs(subdirectorio)
    return subdirectorio

# Función para procesar una lista de reproducción de YouTube usando Playlist de pytubefix
def procesar_lista_reproduccion(url_playlist):
    try:
        # Cargar la lista de reproducción
        p = Playlist(url_playlist)
        
        # Crear un subdirectorio para la lista de reproducción
        subdirectorio_lista = crear_subdirectorio_lista("lista")
        print(f"Guardando transcripciones en el directorio: {subdirectorio_lista}")
        
        # Recorrer todas las URLs de videos en la lista de reproducción
        for url in p.video_urls:
            try:
                print(f"\nProcesando video: {url}")
                
                # Procesar el video y guardar la transcripción en el subdirectorio de la lista
                procesar_video_youtube(url, subdirectorio_lista)
            except Exception as e:
                # Manejo de errores en cada video
                print(f"Error al procesar el video {url}. Continuando con el siguiente video: {e}")
    except Exception as e:
        print(f"Error al procesar la lista de reproducción: {e}")


# Ejemplo de uso con una lista de reproducción de YouTube
url_playlist = "https://www.youtube.com/watch?v=9PdUj-KpyM0&list=PLpZJ7XCi1UtMtYAhZ19RHFI8Hk8CNEkAY"
procesar_lista_reproduccion(url_playlist)
```
