# 🐝 SwarmBees - Sistema de Agentes para Jenkins

Sistema de **agentes inteligentes especializados** que permite interactuar con Jenkins directamente desde VS Code usando lenguaje natural a través del Model Context Protocol (MCP).

> 📄 Para más detalles sobre el proyecto, visión y casos de uso, consulta [PRESENTACION.md](PRESENTACION.md)

## 🎯 Agentes Especializados

El proyecto incluye tres agentes especializados:

- **@scout-bee** 🔍 - Monitoreo y observación del sistema
- **@doctor-bee** 🩺 - Diagnóstico y análisis de errores
- **@worker-bee** 💪 - Ejecución de builds y tareas

## 📋 Requisitos Previos

- Python 3.10 o superior
- VS Code con GitHub Copilot
- Acceso a un servidor Jenkins
- Token de API de Jenkins

## Instalación

### 1. Instalar Python (si no lo tienes)

**Opción A - Desde Microsoft Store (Más fácil):**
1. Abre Microsoft Store
2. Busca "Python 3.12"
3. Instala Python 3.12

**Opción B - Descarga directa:**
1. Ve a https://www.python.org/downloads/
2. Descarga Python 3.12 o superior
3. Durante la instalación, **marca "Add Python to PATH"**

**Opción C - Con winget (requiere aceptar términos):**
```powershell
winget install Python.Python.3.12
```

Después de instalar Python, cierra y vuelve a abrir la terminal.

### 2. Instalar uv

Instala `uv` (gestor de paquetes Python moderno):

```powershell
python -m pip install uv
```

> **Nota:** Este proyecto actualmente utiliza el paquete **[mcp-jenkins](https://github.com/lanbaoshen/mcp-jenkins)** de la comunidad. `uvx` (incluido con `uv`) lo ejecutará automáticamente sin necesidad de instalación separada.

### 3. Configurar Variables de Entorno

Copia el archivo de ejemplo y completa tus datos:

```powershell
copy .env.example .env
```

Edita el archivo `.env` con tus credenciales de Jenkins:

```env
JENKINS_URL=http://tu-servidor-jenkins:8080
JENKINS_USERNAME=tu_usuario
JENKINS_PASSWORD=tu_token_de_api
MCP_PORT=3000
```

**Nota:** Es recomendable usar un token de API en lugar de tu contraseña. Puedes generar un token en Jenkins:
1. Ve a tu perfil de usuario
2. Configuración → API Token
3. Genera un nuevo token

### 4. Iniciar el Servidor MCP

En una terminal de PowerShell, ejecuta:

```powershell
uvx mcp-jenkins --jenkins-url http://tu-servidor-jenkins:8080 --jenkins-username tu_usuario --jenkins-password tu_token --transport sse --port 3000
```

O usando variables de entorno:

```powershell
uvx mcp-jenkins --jenkins-url $env:JENKINS_URL --jenkins-username $env:JENKINS_USERNAME --jenkins-password $env:JENKINS_PASSWORD --transport sse --port 3000
```

### 5. Configurar VS Code

El archivo `.vscode/mcp.json` ya está configurado. Solo necesitas:

1. Reiniciar VS Code
2. El servidor MCP debe estar ejecutándose antes de usar Copilot

## 💬 Uso

Una vez configurado, puedes usar los agentes especializados en VS Code Copilot:

### Ejemplos con @scout-bee (Monitoreo)
```
@scout-bee lista todos los jobs
@scout-bee ¿hay builds corriendo?
@scout-bee muestra el estado de los nodos
```

### Ejemplos con @doctor-bee (Diagnóstico)
```
@doctor-bee analiza por qué falló el job appJava
@doctor-bee muestra los logs del último build
@doctor-bee ¿qué errores hay en el build #45?
```

### Ejemplos con @worker-bee (Ejecución)
```
@worker-bee ejecuta el job de deploy
@worker-bee detén el build #23
@worker-bee lanza appJava con parámetro env=staging
```

## Herramientas Disponibles

El MCP proporciona las siguientes herramientas:

- `get_all_jobs` - Obtener todos los jobs
- `get_job_config` - Obtener configuración de un job
- `search_jobs` - Buscar jobs por campo específico
- `get_running_builds` - Obtener builds en ejecución
- `stop_build` - Detener un build
- `get_build_info` - Obtener información de un build
- `get_build_logs` - Obtener logs de un build
- `build_job` - Ejecutar un job con parámetros
- `get_test_results` - Obtener resultados de pruebas
- `get_all_nodes` - Obtener nodos
- `get_all_queue_items` - Obtener items en cola
- `cancel_queue_item` - Cancelar item en cola
- `get_multibranch_jobs` - Obtener jobs multibranch
- `scan_multibranch_pipeline` - Escanear pipeline multibranch

## ⚙️ Configuración Avanzada

### Modo de Transporte

El proyecto está configurado para usar **SSE (Server-Sent Events)**:

```json
{
  "servers": {
    "jenkins": {
      "url": "http://localhost:3000/sse",
      "type": "sse"
    }
  }
}
```

### Modo Solo Lectura

Para evitar modificaciones accidentales, ejecuta el servidor en modo solo lectura:

```powershell
uvx mcp-jenkins --jenkins-url xxx --jenkins-username xxx --jenkins-password xxx --read-only --transport sse --port 3000
```

## Troubleshooting

### El servidor no se conecta
- Verifica que la URL de Jenkins sea accesible
- Comprueba que las credenciales sean correctas
- Asegúrate de que el puerto 3000 no esté en uso

### VS Code no reconoce el MCP
- Reinicia VS Code completamente
- Verifica que el servidor MCP esté corriendo
- Revisa los logs de Copilot en VS Code

### Error de autenticación
- Usa un token de API en lugar de contraseña
- Verifica los permisos del usuario en Jenkins

## 📚 Estructura del Proyecto

```
jenkinsmcp/
├── .github/
│   └── agents/          # Definiciones de agentes
│       ├── scout-bee.agent.md
│       ├── doctor-bee.agent.md
│       └── worker-bee.agent.md
├── .vscode/
│   └── mcp.json         # Configuración MCP
├── images/              # Imágenes del proyecto
├── .env                 # Variables de entorno (no en git)
├── .env.example         # Plantilla de configuración
├── PRESENTACION.md      # Documentación completa del proyecto
└── README.md            # Este archivo
```

## 🔗 Recursos

- [Repositorio mcp-jenkins](https://github.com/lanbaoshen/mcp-jenkins)
- [Documentación MCP](https://modelcontextprotocol.io/)
- [PyPI Package](https://pypi.org/project/mcp-jenkins/)
- [CloudBees](https://www.cloudbees.com/) - Inspiración del concepto "bees"

## 📄 Documentación Completa

Para información detallada sobre la visión del proyecto, arquitectura, casos de uso y roadmap, consulta [PRESENTACION.md](PRESENTACION.md).

---

**🐝 "Haciendo que Jenkins sea tan fácil como hablar con un compañero"**
