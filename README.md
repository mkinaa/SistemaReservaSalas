# SistemaReservaSalas
- Jorge Moncada
- Sección 2

# DIAGRAMA DE CASO DE USO (SOLUCIONADO)
---
![Image](https://github.com/user-attachments/assets/e2e2ab23-b12e-4d98-b0b6-49f3ff9aa350)

## Errores encontrados:
### 1. Actores conectados directamente con otros sistemas:
  - El administrador no debe interactuar directamente con APIReservaExterna ni con SistemaNotificaciones. Estas son responsabilidades del sistema principal..
  
### 2. Conectores no especifícan relaciones
  - Falta de conector (`<<include>>` o `<<extend>>`). Las relaciones entre casos de uso que dependen unos de otros deberían utilizar etiquetas claras para expresar su naturaleza.

### 3. Nombres poco claros:
  - "Notificar cambio por correo" parece una función interna del sistema más que un caso de uso independiente.
  - "Consultar disponibilidad externa" no debería ser accesible directamente por los actores, sino como parte de "Reservar sala".

---
# DIAGRAMA DE CLASES (SOLUCIONADO)
---
![Image](https://github.com/user-attachments/assets/dec3418e-44af-4e59-b481-c2285dcdfcc9)

## Errores encontrados:
### 1. Herencia incorrecta: Administrador hereda de Usuario
  - En términos de diseño orientado a objetos, el `Administrador` tiene responsabilidades adicionales muy específicas (como aprobar o rechazar reservas), lo que puede justificar una composición o asociación en lugar de herencia.
    
### 2. Falta de relación entre Usuario y Reserva
  - No hay ninguna relación entre `Usuario` y `Reserva`, lo cual es un problema, puesto que, los usuarios son los que hacen las reservas, entonces debería haber una relación directa ahí (como que un usuario tenga una lista de reservas).

### 3. Indeterminación en la responsabilidad de reservar() y cancelar()
  - Tanto `Usuario` como `SistemaReservas` tienen métodos para reservar y cancelar, lo cual puede generar confusión. Sería mejor definir con claridad quién se encarga de qué. Por ejemplo, el usuario podría solicitar la reserva pero el sistema debe gestionarla.

---
# DIAGRAMA DE IMPLEMENTACIÓN (SOLUCIONADO)
---
![Image](https://github.com/user-attachments/assets/abf581ae-aebb-40ff-991b-24dcf72fcfae)

## Errores encontrados:
### 1. Falta de conexiones directas entre módulos clave
  - Analizando el diagrama, me di cuenta que aunque el `Formulario de Reserva` y el `Módulo de Historial` se encuentran dentro del `Navegador del Estudiante`, no se muestra claramente cómo se comunican con el `Controlador de Reservas` del `Servidor Web`. Esto es importante porque el navegador debería enviar directamente las solicitudes al sevidor, y eso no está explícitamente representado.

### 2. Uso ambiguo del patrón Singleton
  - Se menciona que el `Sistema de Gestión de Reservas` se implementa como Singleton, pero no se explica por qué. Aunque usar Singleton para una clase central puede ser útil, no siempre es la mejor opción si se requiere escalabilidad o múltiples instancias para pruebas, es por esto que estaría limitando al sistema.

### 3. Acoplamiento entre el "Gestor de Notificaciones" y sus implementaciones 
  - Aunque se indica que se aplica el patrón Bridge en el `Gestor de Notificaciones`, visualmente el diagrama sigue mostrando conexiones directas a `Notificador SMS` y `Notificador Correo`. Esto contradice la idea del Bridge, que es desacoplar la abstracción de la implementación.
