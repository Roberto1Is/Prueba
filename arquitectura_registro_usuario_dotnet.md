# Propuesta de Arquitectura: Módulo de Registro de Usuario (usando .NET)

## Descripción General
La arquitectura propuesta para el módulo de registro de usuario está basada en una arquitectura **Clean Architecture** (Arquitectura Limpia) con separación de capas, aprovechando el ecosistema de **.NET 7**. Este diseño asegura un alto grado de modularidad, escalabilidad y facilidad de mantenimiento.

---

## Componentes Principales

### 1. **Frontend**
- **Tecnología sugerida:** Razor Pages, Blazor o React/Angular integrado con una API REST.
- **Responsabilidades:**
  - Renderizar el formulario de registro con validaciones básicas en el cliente.
  - Consumir la API REST desarrollada en el backend para enviar solicitudes de registro.
  - Mostrar mensajes de éxito o error según la respuesta del servidor.
- **Detalles:**
  - Usar la biblioteca `FluentValidation` para asegurar la validación de datos en el cliente (si Razor Pages o Blazor son empleados).

---

### 2. **Backend**
El backend estará compuesto por múltiples capas siguiendo el principio de Clean Architecture.

#### **Capas de la Arquitectura**
1. **Capa de Presentación (API):**
   - Framework: ASP.NET Core Web API.
   - Responsabilidades:
     - Exponer los endpoints para el registro de usuario.
     - Validar datos de entrada mediante filtros o validadores (por ejemplo, `FluentValidation` o atributos `[DataAnnotations]`).
     - Retornar respuestas HTTP claras según el resultado del proceso.

2. **Capa de Aplicación:**
   - Responsabilidades:
     - Contiene la lógica de negocio relacionada con el registro del usuario.
     - Define los casos de uso mediante patrones como **Command/Query** usando **MediatR**.
   - Detalles:
     - Implementar validaciones adicionales en esta capa.
     - Manejar excepciones de negocio.

3. **Capa de Dominio:**
   - Responsabilidades:
     - Define las entidades principales (por ejemplo: Usuario).
     - Contiene la lógica de negocio pura.
   - Detalles:
     - Implementar patrones como **Value Objects** para atributos como contraseñas.

4. **Capa de Infraestructura:**
   - Framework: Entity Framework Core (EF Core) con SQL Server.
   - Responsabilidades:
     - Manejo de la persistencia de datos en la base de datos.
     - Implementar repositorios para las entidades del dominio.
     - Hash de contraseñas mediante bibliotecas como `Microsoft.AspNetCore.Identity`.

---

### 3. **Base de Datos**
- **Tecnología sugerida:** SQL Server.
- **Estructura de la Tabla:**
```sql
CREATE TABLE Usuarios (
    Id UNIQUEIDENTIFIER PRIMARY KEY DEFAULT NEWID(),
    Nombre NVARCHAR(100) NOT NULL,
    Correo NVARCHAR(150) NOT NULL UNIQUE,
    ContraseñaHashed NVARCHAR(MAX) NOT NULL,
    FechaCreacion DATETIME DEFAULT GETDATE()
);
```
- **Detalles:**
  - Asegurar que el campo `Correo` sea único.
  - Almacenar las contraseñas siempre en formato cifrado.

---

### 4. **API REST**
- **Puntos de acceso sugeridos:**
  - **POST /api/v1/registro**
    - Descripción: Endpoint para registrar un usuario.
    - Entrada esperada (JSON):
      ```json
      {
          "nombre": "Juan Pérez",
          "correo": "juan.perez@example.com",
          "contraseña": "Contraseña123"
      }
      ```
    - Respuesta de éxito (HTTP 201):
      ```json
      {
          "mensaje": "Registro completado exitosamente."
      }
      ```
    - Respuesta de error (HTTP 400):
      ```json
      {
          "error": "El correo electrónico ya está en uso."
      }
      ```
  - **GET /api/v1/usuarios/{id}** (opcional)
    - Descripción: Recuperar detalles del usuario (sin incluir la contraseña).

---

### 5. **Validaciones de Seguridad**
- **Backend:**
  - Validar que los campos no estén vacíos y cumplan con las reglas de negocio.
  - Implementar hashing de contraseñas utilizando `Microsoft.AspNetCore.Identity` o `BCrypt`.
  - Evitar exponer datos sensibles en las respuestas.
- **Transmisión de Datos:**
  - Usar HTTPS para proteger la transmisión de datos sensibles.
  - Configurar medidas de seguridad adicionales como **CORS** y protección contra ataques **CSRF**.

---

### 6. **Autenticación**
- Utilizar **JWT (JSON Web Tokens)** para manejar la autenticación después del registro.
- **Flujo sugerido:**
  1. El usuario se registra correctamente.
  2. Luego de registrarse, se le redirige a la página de inicio de sesión.
  3. Un token JWT es emitido al iniciar sesión correctamente.

---

## Diagrama Resumido de Arquitectura

```plaintext
[Frontend] --> Formulario de Registro ---> [API REST .NET]
    (Razor Pages/Blazor/React)                   (ASP.NET Core)
                                                   |
                                                   |
                                    [Capa de Aplicación: Casos de Uso]
                                                   |
                                                   |
                                    [Capa de Infraestructura: EF Core]
                                                   |
                                                   |
                                    [Base de Datos: SQL Server]
```

---

## Tareas Pendientes
1. Implementar el formulario en Razor Pages o Blazor.
2. Configurar la API REST en ASP.NET Core.
3. Crear los casos de uso en la capa de aplicación con MediatR.
4. Diseñar y probar la base de datos con EF Core.
5. Implementar seguridad para las contraseñas y el transporte de datos.
6. Probar el flujo de registro de extremo a extremo.