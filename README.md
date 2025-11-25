# Asistente multiusuario con LangChain y Redis

Notebook de ejemplo que crea un asistente conversacional breve, recuerda el contexto de cada usuario usando Redis y formatea las respuestas con un modelo de OpenAI.

- Historial por persona con TTL de 24 horas en Redis.
- Prompts con `ChatPromptTemplate` y validación de salida con `PydanticOutputParser`.
- Loop interactivo opcional para hablar desde la terminal dentro del notebook.

## Requisitos
- Python 3.10+ y `pip`.
- Acceso a una instancia de Redis (local o remota).
- Clave de API de OpenAI.
- Jupyter (Notebook o VS Code) para ejecutar `tarea_grupal.ipynb`.

## Configuración rápida
1) Crea y activa un entorno virtual (opcional):
```bash
python3 -m venv .venv
source .venv/bin/activate
```
2) Instala dependencias principales (las mismas que en la celda 1):
```bash
pip install langchain langchain-openai langchain-community python-dotenv redis
# Si no tienes Jupyter:
pip install notebook
```
3) Crea un archivo `.env` en la raíz con tus credenciales:
```
OPENAI_API_KEY=tu_api_key
REDIS_URL=redis://:password@host:6379/0
```

## Uso del notebook
1) Abre `tarea_grupal.ipynb` y ejecuta las celdas en orden:
   - Dependencias.
   - Carga de entorno (`load_dotenv()`); falla si faltan variables.
   - Helpers de historial y prompt/modelo.
2) Inicia el loop interactivo ejecutando `chat_loop()` en la celda indicada. Escribe mensajes con el formato `Nombre: mensaje` (ejemplo: `Fabian: Hola, cómo estás?`). Escribe `salir` para terminar.
3) Consulta o limpia historiales con las funciones al final del notebook:
   - `print_history('Fabian')`
   - `clear_history('Fabian')`

## Notas
- Los nombres se normalizan a minúsculas y sin espacios iniciales/finales; el formato `Nombre: mensaje` es obligatorio.
- El historial se almacena en Redis con TTL de 24 horas. Asegúrate de que `REDIS_URL` apunte a una instancia accesible.
- El modelo usado por defecto es `gpt-4o-mini` con temperatura 0.4; puedes cambiarlo en la celda de configuración del LLM.
