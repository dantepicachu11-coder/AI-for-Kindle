# AI Chat for Kindle Paperwhite

Una aplicación web de chat con IA diseñada específicamente para el navegador antiguo del Kindle Paperwhite 7ª generación (2015). Compatible también con navegadores modernos.

## Características

- **3 modelos de IA a elegir**: Groq (Llama 3.1), Google Gemini 2.5 Flash, Cohere Command R+
- **Carga de documentos desde GitHub**: Sube archivos `.txt` a la carpeta `docs/` de tu repo y cárgatelos como contexto en el chat
- **Sistema de instrucciones personalizadas**: Define cómo debe comportarse la IA (por defecto, configuración educativa/evaluativa)
- **Persistencia vía URL**: Guarda tus keys de API en un marcador del Kindle — se cargan automáticamente en cada visita
- **Compatible con Kindle 7ª gen**: Funciona con el navegador WebKit antiguo del Kindle — sin `fetch()`, sin ES6, sin flexbox

## Requisitos

Necesitas **al menos una key de API** de:

1. **Groq** (recomendado — gratuito, tier libre): https://console.groq.com
   - Llama 3.1 8B Instant, hasta 14,400 requests/día gratis
   - Key comienza con `gsk_`

2. **Google Gemini** (gratis con Google AI Studio): https://aistudio.google.com
   - Gemini 2.5 Flash
   - Key comienza con `AIza`

3. **Cohere** (gratuito): https://dashboard.cohere.com
   - Command R+ model
   - Key personalizada

Además necesitas:

- **GitHub token** (opcional pero recomendado para cargar documentos): https://github.com/settings/tokens
  - Marca solo `public_repo` scope
  - Key comienza con `ghp_`

## Instalación

1. Clona o haz fork de este repositorio
2. GitHub Pages debe estar habilitado en tu repo (Settings → Pages → Deploy from main)
3. El archivo debe llamarse `index.html` en la raíz

## Primeros pasos

### Opción A: Vía marcador (recomendado para Kindle)

1. Abre la página en cualquier navegador: `https://tuusuario.github.io/turepository/`
2. Rellena los campos con tus keys
3. Toca **Generate Bookmark URL**
4. Copia la URL larga que aparece
5. En el Kindle, guarda esa URL como marcador
6. La próxima vez que abras el marcador, las keys cargan automáticamente

**URL de ejemplo:**
```
https://tuusuario.github.io/turepository/?gk=TU_GROQ_KEY&gem=TU_GEMINI_KEY&gh=TU_GITHUB_TOKEN&ai=groq
```

Parámetros:
- `gk=` → Groq key
- `gem=` → Gemini key  
- `co=` → Cohere key
- `gh=` → GitHub token
- `ai=` → IA por defecto (`groq`, `gemini`, o `cohere`)

### Opción B: Escribir las keys en cada sesión

1. Abre la página
2. Escribe las keys en los campos
3. Chatea libremente
4. Al cerrar el navegador, las keys se pierden (limitación del Kindle)

## Cargador de documentos

1. Crea una carpeta `docs/` en tu repo
2. Sube archivos `.txt` (archivos de texto plano)
3. Dentro de la app, toca **Refresh file list**
4. Selecciona los archivos que quieres usar
5. El contenido se envía al chat como contexto

**Ejemplo:** Si tienes `apuntes.txt` en `docs/`, puedes cargar ese archivo y hacer preguntas basadas en su contenido.

## Instrucciones personalizadas

El campo **Instructions for the AI** controla el comportamiento de la IA.

**Instrucciones por defecto** (educativas, evaluativas):
- Responde en el idioma del usuario
- Para matemática: verifica cálculos paso a paso
- Para teoría: responde como en un examen
- Longitud: ~4 oraciones por respuesta

Puedes cambiarlas en cualquier momento. El botón **Reset to Default** las restaura.

Las instrucciones se guardan en localStorage del navegador (sin URL, solo durante la sesión).

## Limitaciones del Kindle Paperwhite 7ª gen

- **No localStorage persistente**: Los datos se pierden al cerrar el navegador (por eso usamos URL)
- **Sin position:fixed**: El layout se adapta al scroll natural de la página
- **Sin flexbox**: CSS básico solamente
- **Sin fetch() API**: Usa XMLHttpRequest en su lugar
- **Sin ES6**: Solo JavaScript clásico (`var`, sin arrow functions)
- **Sin CORS en raw.githubusercontent.com**: Por eso cargamos archivos vía GitHub API

## Estructura del repositorio

```
your-repo/
├── index.html              # La aplicación web
├── docs/                   # Carpeta de documentos (crea si no existe)
│   ├── apuntes.txt
│   ├── resumen.txt
│   └── ...
└── README.md              # Este archivo
```

## Solución de problemas

### Error 401 al usar Gemini
La key no es válida o está asociada a un proyecto con restricciones. Intenta generarla desde **aistudio.google.com** (no Google Cloud Console).

### "error loading" al cargar documentos
Asegúrate de:
- Tener la carpeta `docs/` en el repo
- Tener un GitHub token válido (opcional pero útil)
- Los archivos sean `.txt` (texto plano)

### Las keys no se guardan en el Kindle
Es una limitación del navegador Kindle con localStorage en dominios externos. Usa la opción de URL con marcador en su lugar.

### El chat se ve pequeño/cortado en el Kindle
Zoom en el navegador del Kindle (pinch-to-zoom o menú). La página está optimizada para leer.

## Rendimiento

- **Groq Llama 3.1**: Muy rápido (~1-2 seg), respuestas cortas (1024 tokens)
- **Gemini 2.5 Flash**: Rápido (~2-3 seg), respuestas largas
- **Cohere Command R+**: Moderado (~2-3 seg), muy detallado

Los tokens de Groq son limitados (14,400/día gratis). Gemini y Cohere tienen límites distintos según tu plan.

## Privacidad y seguridad

- Las keys se almacenan en la URL del marcador o en localStorage del navegador
- **Recomendación**: Si alguien tiene acceso físico a tu Kindle, puede ver tus keys
- No compartas URLs públicamente con tus keys incluidas
- Si revocas una key, actualiza el marcador

## Créditos y licencia

Desarrollado para funcionar con hardware antiguo (Kindle Paperwhite 7ª gen).

Utilizados:
- Groq API (https://groq.com)
- Google Gemini API (https://ai.google.dev)
- Cohere API (https://cohere.com)
- GitHub API (https://docs.github.com/es/rest)

## Preguntas frecuentes

**¿Funciona en otros navegadores?**
Sí, en navegadores modernos funciona igual. Las limitaciones del Kindle no afectan a otros dispositivos.

**¿Puedo usar otros modelos de IA?**
Claro. El código es modificable. Solo añade una nueva función `sendOtraIA()` siguiendo el patrón de Groq/Gemini/Cohere.

**¿Qué pasa si se agotan los tokens?**
Recibirás un error de la API (401, 429, etc.). Espera al siguiente período de facturación o usa otra IA.

**¿Puedo guardar el chat?**
No automáticamente. Puedes copiar la conversación manualmente desde el navegador.

---

**Última actualización**: Junio 2026
