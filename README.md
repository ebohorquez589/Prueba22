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

## 3. Clase WhatsAppBot

Esta es la clase central que contiene la lógica para automatizar la interacción con WhatsApp Web.

### Propósito
Controla el ciclo de vida del navegador (iniciar, enviar mensaje, cerrar) y utiliza la configuración de `WhatsAppConfig` y el destino de `WhatsAppDestination`.

### Métodos clave

#### `_initialize_driver`
Inicializa el WebDriver de Chrome usando `Service` y las opciones configuradas.

#### `enviar_mensaje`
El método principal:

- Abre la URL de destino en el navegador (`self.driver.get(destino.url)`).
- Usa `WebDriverWait` (con timeout) y `expected_conditions (EC)` para esperar a que el cuadro de mensaje esté cargado y localizable por su **XPATH**.
- Copia el mensaje a enviar al portapapeles (`pyperclip.copy`).
- Pega el mensaje en el cuadro de texto usando **Ctrl+V** (`Keys.CONTROL, 'v'`), lo cual es mejor para formato y mensajes largos.
- Presiona **Enter** (`Keys.RETURN`) para enviar el mensaje.

#### `cerrar`
Cierra el navegador usando `self.driver.quit()`.

## 3.1 Función `enviar_mensaje_y_mantener_abierto` (de WhatsAppBot)

Este método es un *wrapper* alrededor de `enviar_mensaje`.

### Propósito
Envía el mensaje y luego pausa la ejecución del script.

### Comportamiento
Mantiene la ventana del navegador abierta hasta que el usuario presione **Enter** en la terminal, lo cual es útil para verificar el envío o para mantener la sesión de WhatsApp activa después de enviar el mensaje.

## 4. Función `enviar_mensaje_whatsapp_selenium`

Esta función es una capa de compatibilidad o un acceso directo simple.

### Propósito
Simplificar el uso del bot para usuarios que solo necesitan enviar un mensaje rápidamente sin interactuar directamente con las clases.

### Comportamiento
Recibe los argumentos necesarios, crea internamente instancias de `WhatsAppConfig` y `WhatsAppBot`, llama a `bot.enviar_mensaje()`, y garantiza que el navegador se cierre al finalizar (`finally: bot.cerrar()`).

## 5. Ejemplo de Uso (`if __name__ == "__main__":`)

Este bloque demuestra cómo usar las clases del módulo.

### Ejemplo 1 (Grupo)
Muestra el proceso completo para enviar un mensaje a un enlace de grupo usando `WhatsAppBot` y `enviar_mensaje_y_mantener_abierto`.

### Ejemplo 2 (Individuo)
Muestra cómo enviar un mensaje a un número de teléfono (formato internacional) usando el mismo flujo.

### Ejemplo 3 (Compatibilidad)
Muestra el uso de la función `enviar_mensaje_whatsapp_selenium` para un envío rápido a un individuo.

### Nota
Los ejemplos 1 y 2 usan `enviar_mensaje_y_mantener_abierto()` porque permiten verificar el envío manualmente antes de cerrar el navegador.




