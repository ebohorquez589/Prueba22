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

## 2. Clase WhatsAppDestination

Esta clase modela y prepara el destino del mensaje, ya sea un grupo o un contacto individual.

### Propósito
Recibe un identificador (enlace de grupo o número de teléfono) y determina automáticamente si es un grupo o un individuo, generando la URL específica de WhatsApp Web para iniciar el chat.

### Métodos clave

#### `__init__`
Recibe el `identifier` y llama a `_detect_type()` para establecer `self.is_group`.

#### `_detect_type`
Utiliza expresiones regulares (`re`) y lógica simple para autodetectar el tipo de destino:
- `True` si parece un enlace de grupo
- `False` si parece un número de teléfono

#### `_generate_url`
Construye la URL de destino final:

- **Para grupos:**  
  `https://web.whatsapp.com/accept?code=...`

- **Para individuos:**  
  `https://web.whatsapp.com/send?phone=...`

