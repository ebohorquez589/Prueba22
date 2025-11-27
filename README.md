# Descripción
Bot automatizado para enviar mensajes a WhatsApp Web que soporta tanto grupos como usuarios individuales. Utiliza Selenium para automatizar la interacción con WhatsApp Web.

## Características

-  Envío automático a grupos de WhatsApp
-  Envío automático a usuarios individuales
-  Detección automática del tipo de destino (grupo/individuo)
-  Configuración flexible y reutilizable
-  Compatibilidad con código anterior
-  Logging completo de operaciones
-  Soporte para mensajes con formato 


## Requisitos
```bash
pip install selenium webdriver-manager pyperclip
```
### Dependencias del sistema

- Google Chrome o Chromium
- ChromeDriver (se instala automáticamente con `webdriver-manager`)


## 1. Clase WhatsAppConfig

Esta clase se encarga de gestionar la configuración del navegador Chrome para Selenium.

### Propósito
Define las rutas necesarias y las opciones de inicio para el driver de Chrome, asegurando que se utilice un perfil de usuario específico (`user_data_dir`) para mantener la sesión de WhatsApp Web iniciada (evitando escanear el código QR cada vez).

### Métodos clave

#### `__init__`
Inicializa las rutas al binario de Chrome, al directorio de datos del usuario (perfil de WhatsApp) y descarga o usa el ChromeDriver necesario usando `ChromeDriverManager`.

#### `get_chrome_options`
Crea y retorna un objeto `Options` de Selenium con argumentos importantes como maximizar la ventana, deshabilitar extensiones y, crucialmente, configurar la ubicación del perfil de usuario (`user-data-dir`).
