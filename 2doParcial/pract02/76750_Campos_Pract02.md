# Práctica 02 - Terminal Personalizada

**Alumno:** Alan Campos Suarez (76750)
**Materia:** Concurrencia y Paralelismo

## 1. Implementar 2 comandos
Se implementaron más de dos comandos para la terminal, entre ellos `echo`, `date`, `len` y `reverse`.
- **`echo <texto>`**: Imprime en pantalla el texto que el usuario ingrese.
- **`date`**: Utiliza las clases `LocalDateTime` y `DateTimeFormatter` para mostrar la fecha y hora actual del sistema en tiempo real.

## 2. Comando Help
Se implementó un comando `help` que imprime una lista formateada con todos los comandos disponibles, su función y la sintaxis de uso, facilitando la interacción del usuario con la terminal.

## 3. Comando Login (Verificar usuario con regex)
Para el inicio de sesión, se utiliza la clase `Pattern` de Java. Se implementó una Expresión Regular (Regex) que valida que el texto ingresado tenga un formato de correo electrónico válido:
`^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,6}$`
Si el correo es válido, el sistema extrae el nombre antes del `@` y lo guarda como el usuario actual.

## 4. Personalizar Terminal
La terminal personaliza su "prompt" o línea de comandos de forma dinámica. Al inicio muestra `[invitado] CMD>`. Cuando el usuario hace `login` con un correo válido, el prompt cambia para reflejar el nombre del usuario logueado (por ejemplo, `[alan] CMD>`), demostrando así la personalización del entorno.
