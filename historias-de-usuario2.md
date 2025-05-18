# Historias de Usuario para la Aplicación de Gestión de Reservas de Salas de Reuniones

## Historia de Usuario 1: Consultar Disponibilidad de Salas
**Como** usuario,  
**quiero** ver qué salas están disponibles en una fecha y horario específicos,  
**para** poder planificar mejor mis reuniones.

### Criterios de Aceptación
- El sistema permitirá seleccionar una fecha y un horario específico.
- Se mostrará una lista de salas disponibles que no estén reservadas en ese intervalo de tiempo.
- Se incluirá información de cada sala, como el nombre, capacidad y equipamiento disponible.

---

## Historia de Usuario 2: Gestionar Reservas
**Como** usuario,  
**quiero** crear, modificar o cancelar una reserva,  
**para** gestionar el uso de las salas de reuniones según mis necesidades.

### Criterios de Aceptación
- El sistema permitirá:
  - Crear una reserva seleccionando una sala, fecha y horario.
  - Modificar los detalles de una reserva existente (sala, horario, etc.).
  - Cancelar una reserva existente.
- El sistema verificará que no haya solapamientos en las reservas de una misma sala.
- El usuario recibirá una confirmación al completar cada acción.

---

## Historia de Usuario 3: Información de las Salas
**Como** usuario,  
**quiero** ver el nombre, capacidad y equipamiento de cada sala,  
**para** elegir la sala más adecuada para mi reunión.

### Criterios de Aceptación
- Cada sala mostrará su nombre, capacidad máxima de personas y una lista de equipamiento disponible (por ejemplo: proyector, pizarra, sistema de videoconferencia, etc.).
- La información estará disponible antes de realizar una reserva.

---

## Historia de Usuario 4: Autenticación de Usuarios
**Como** usuario,  
**quiero** iniciar sesión en el sistema,  
**para** poder realizar reservas de salas de reuniones.

### Criterios de Aceptación
- El sistema requerirá que los usuarios se autentiquen utilizando un nombre de usuario y una contraseña válidos.
- Los usuarios no autenticados solo podrán consultar la disponibilidad de las salas, pero no podrán crear, modificar ni cancelar reservas.

---

## Historia de Usuario 5: Evitar Reservas Solapadas
**Como** usuario,  
**quiero** que el sistema no permita reservas solapadas en una misma sala,  
**para** evitar conflictos en el uso de las salas.

### Criterios de Aceptación
- El sistema verificará que no haya otras reservas en la misma sala para el intervalo de tiempo seleccionado antes de permitir crear o modificar una reserva.
- En caso de conflicto, el sistema mostrará un mensaje de error indicando que la sala ya está reservada para ese horario.