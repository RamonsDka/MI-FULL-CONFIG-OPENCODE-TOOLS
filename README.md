# 🚀 OpenCode Ultimate Setup — Gentleman Edition

Bienvenido a la documentación definitiva del entorno más avanzado y automatizado para **OpenCode**. Este repositorio contiene una arquitectura robusta orientada al desarrollo impulsado por especificaciones (SDD), integración profunda con **Gentle SDD Router (GSR)**, un sistema visual de gestión de perfiles (TUI) y un motor de resiliencia (Fallbacks) inigualable.

---

## 🌟 Características Principales

1. **Gestor de Perfiles Visual (TUI)**: Interfaz nativa (`alt+p`) para cambiar entre configuraciones de agentes al vuelo.
2. **GSR Integrado**: Presets preconfigurados (`free`, `mixto`, `premium`) para equilibrar costo y rendimiento.
3. **Motor de Fallbacks**: Hasta 3 modelos de respaldo por agente para garantizar que el workflow nunca se detenga.
4. **Themes (Arch Mask)**: 8 esquemas de colores cyber/neon aplicables en tiempo real (`alt+m`).
5. **Flujo SDD Completo**: Agentes especializados (Orchestrator, Explore, Propose, Spec, Design, Tasks, Apply, Verify, Archive).

---

## 📂 1. Arquitectura del Directorio

El sistema se instala directamente en `~/.config/opencode/` (Windows: `C:\Users\TU_USUARIO\.config\opencode\`).

```text
~/.config/opencode/
├── opencode.json             # Archivo principal de configuración (Agentes, MCPs, Tools)
├── tui.json                  # Registro de plugins TUI (carga arch-mask-opencode)
├── AGENTS.md                 # Prompt maestro y reglas de personalidad (El "Gentleman")
├── arch-mask-opencode/       # 💎 Plugin TUI: Motor de la interfaz visual, menús y temas
├── router/                   # ⚙️ Configuración del Gentle SDD Router (GSR)
│   ├── router.yaml           # Estado global de GSR y preset activo
│   ├── profiles/             # Presets en YAML (free, mixto, premium, sdd-debug)
│   └── catalogs/             # Contratos y workflows estrictos para agentes
├── profiles/                 # 💾 Snapshots JSON de perfiles personalizados creados por el usuario
├── plugins/                  # Scripts en background y MCPs locales (Engram)
├── skills/                   # Archivos de instrucciones .md para cada sub-agente SDD
└── commands/                 # Comandos inteligentes de chat (ej: /perfiles.md)
```

---

## 🧠 2. Gentle SDD Router (GSR) y Sistema de Fallbacks

**GSR** es un enrutador inteligente que gestiona qué modelo de IA asume cada rol dentro del ciclo de desarrollo. 

### Los Presets
Hemos implementado 3 presets principales accesibles desde el Tab switcher (`gsr-*`):
- 🟢 **`gsr-free`**: Utiliza exclusivamente modelos gratuitos (Mistral, Qwen, Gemini Free) ideal para tareas cotidianas.
- 🟡 **`gsr-mixto`**: El punto dulce. Usa modelos rápidos y económicos (GPT-5.3-instant, Claude Haiku) para coordinación, pero delega código pesado a modelos mayores.
- 🔴 **`gsr-premium`**: Toda la artillería (Claude 3.7 Sonnet, GPT-5.4, Gemini Pro) para tareas de arquitectura compleja.

### El Motor de Fallbacks
Si la API de Anthropic cae, o OpenAI da timeout, el sistema **no se detiene**. Cada agente tiene configurado un sistema de cascada (`fallback` en `opencode.json`):

```json
"gsr-mixto": {
  "model": "openai/gpt-5.3-instant",
  "fallback": "mistral/mistral-large-3, opencode-go/glm-5, opencode/qwen3.6-plus-free"
}
```
*Si GPT-5 falla, salta a Mistral; si falla, va a GLM; si falla, a Qwen.* Todo esto ocurre en milisegundos y de forma transparente para el usuario.

### 🛠️ Cómo implementarlo en tu repo:
1. Instalá GSR globalmente: `npm install -g gentle-sdd-router`
2. Copiá la carpeta `router/` de este repositorio a tu `.config/opencode/`
3. Usá el comando `gsr apply opencode --apply` para inyectar los agentes en tu `opencode.json`.

---

## 🖥️ 3. Sistema de Perfiles Avanzado (Arch Mask TUI)

Olvida editar el `opencode.json` a mano. Hemos integrado un plugin interactivo inspirado en Arch Linux.

### Atajos de teclado:
- **`Alt + P`**: Abre el Gestor de Perfiles.
- **`Alt + M`**: Abre el Gestor de Temas (Mask).

### Funcionalidades del Gestor (`Alt + P`):
1. **Crear Perfil**: Toma tu configuración actual, le ponés un nombre y se guarda en `/profiles/`.
2. **Asignar Perfil**: Carga un perfil previamente guardado. ¡Te indica con un **✓ ACTIVO** cuál estás usando!
3. **Editor de Agentes Visual**: 
   - Entrás al perfil, elegís un agente (ej: `sdd-orchestrator`).
   - Te muestra una lista de proveedores (OpenAI, Anthropic, Mistral).
   - Elegís el modelo exacto.
   - Te abre el **Gestor de Fallbacks** para elegir gráficamente los 3 modelos de respaldo.
4. **Presets GSR**: Cambia el enrutamiento base de GSR sin reiniciar OpenCode.

### 🛠️ Cómo implementar el Plugin:
1. Copiá la carpeta `arch-mask-opencode/` a tu `.config/opencode/`.
2. Editá `tui.json` para cargar el plugin:
```json
{
  "plugin": [
    [
      "C:\\Ruta\\A\\Tu\\.config\\opencode\\arch-mask-opencode",
      { "enabled": true, "theme": "j0k3r-dev-rgl", "show_sidebar": true }
    ]
  ]
}
```

---

## 🎨 4. Temas y Personalización

El plugin TUI viene con 8 temas integrados. Se aplican en caliente usando `Alt + M`:
- `j0k3r-dev-rgl` (Predeterminado - Rosa/Morado cyber)
- `arch-cyber` (Verde matrix)
- `arch-neon` (Cian brillante)
- `tokyo-night-dev` (Azul oscuro/Morado)
- `arch-overclock` (Rojo agresivo)

*El tema elegido se guarda en la memoria local (`api.kv`) y persiste entre sesiones.*

---

## ⚙️ 5. El Flujo de Trabajo SDD (Spec-Driven Development)

Este setup está diseñado para que no programes "a lo loco", sino con ingeniería real. El flujo es:

1. Abrís OpenCode y usás `Tab` para seleccionar a **`sdd-orchestrator`**.
2. Le decís: *"Quiero agregar autenticación JWT a mi backend"*.
3. El Orchestrator **no escribe código**. Delega autónomamente a los sub-agentes ocultos:
   - 🕵️‍♂️ `sdd-explore`: Investiga tu código actual.
   - 📝 `sdd-propose`: Crea un plan de ataque.
   - 📐 `sdd-design`: Define la arquitectura.
   - 💻 `sdd-apply`: Escribe el código exacto.
   - ✅ `sdd-verify`: Valida que todo funcione.
4. Vos solo supervisás y aprobás los PRs.

---

## 🚀 6. Guía Rápida de Instalación para Amigos

Si querés replicar esta locura cósmica en tu máquina, seguí estos pasos:

### Paso 1: Clonar e Instalar dependencias
Asegurate de tener NodeJS instalado.
```bash
npm install -g @opencode-ai/cli gentle-sdd-router
```

### Paso 2: Copiar archivos
Copiá todo el contenido de este repositorio dentro de tu directorio de configuración de OpenCode:
- **Windows**: `C:\Users\TU_USUARIO\.config\opencode\`
- **Mac/Linux**: `~/.config/opencode/`

Asegurate de que las carpetas `arch-mask-opencode`, `router`, `profiles` y `skills` queden en la raíz del `.config/opencode/`.

### Paso 3: Instalar dependencias del Plugin TUI
El plugin necesita sus módulos para funcionar:
```bash
cd ~/.config/opencode/arch-mask-opencode
npm install
```

### Paso 4: Ajustar Rutas
⚠️ **MUY IMPORTANTE**: En `tui.json`, actualizá la ruta absoluta para que apunte a TU usuario:
```json
"C:\\Users\\TU_NOMBRE\\.config\\opencode\\arch-mask-opencode"
```

### Paso 5: Autenticación
Autenticate en los proveedores que vayas a usar:
```bash
opencode auth openai
opencode auth anthropic
opencode auth mistral
```

### Paso 6: ¡A disfrutar!
Abrí la terminal y ejecutá:
```bash
opencode
```
Presioná **Alt + P** para maravillarte con tu nuevo sistema de perfiles.

---
*Desarrollado con pasión, resiliencia y obsesión por la automatización.* 🎩🥂
