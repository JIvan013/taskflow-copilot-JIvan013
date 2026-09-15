# Arquitectura de TaskFlow

Este documento es una guía rápida para un desarrollador que llega nuevo al proyecto TaskFlow. Explica la estructura por capas, el flujo completo de la petición `POST /projects/{projectId}/tasks`, dónde viven las reglas de negocio, cómo funciona la seguridad JWT y cómo están organizados los tests.

## Capas y paquetes

- `controller` — capa HTTP: controla rutas REST y valida DTOs.
  - Ejemplo: `src/main/java/com/taskflow/controller/ProjectController.java`
- `dto` — objetos de transferencia (records) usados en los controladores. Validación con Bean Validation (`@Valid`).
  - Ejemplo: `src/main/java/com/taskflow/dto/TaskRequest.java`
- `mapper` — mapeo manual entre DTOs y entidades (no se usa MapStruct aquí).
  - Ejemplo: `src/main/java/com/taskflow/mapper/TaskMapper.java` (método `TaskMapper.aResponse(...)`).
- `service` — casos de uso y lógica de aplicación (orquesta llamadas, transacciones, verificaciones de permiso).
  - Ejemplo: `src/main/java/com/taskflow/service/TaskService.java`
- `repository` — acceso a datos con Spring Data JPA.
  - Ejemplo: `src/main/java/com/taskflow/repository/TaskRepository.java`
- `model` — entidades con comportamiento y reglas de negocio.
  - Ejemplo: `src/main/java/com/taskflow/model/Task.java` (contiene métodos como `crear(...)` y `estaVencida()`).
- `security` — configuración y utilidades JWT y seguridad (filtros, proveedor de tokens, `ProjectSecurity`).
  - Ejemplos: `src/main/java/com/taskflow/config/SecurityConfig.java`, `src/main/java/com/taskflow/security/JwtService.java`, `src/main/java/com/taskflow/security/ProjectSecurity.java`
- `exception` — manejo centralizado de errores.
  - Ejemplo: `src/main/java/com/taskflow/advice/GlobalExceptionHandler.java`
- `seed` / `data` — clases para poblar datos en perfiles de desarrollo.
  - Ejemplo: `src/main/java/com/taskflow/config/DataSeeder.java`

Convenciones importantes: inyección por constructor, DTOs como `record`, controladores usan `@Valid` en `@RequestBody`, rutas sin prefijo `/api` (por ejemplo `/projects`, `/tasks`).

---

## Recorrido de `POST /projects/{projectId}/tasks`

Resumen paso a paso, desde la petición hasta la BD:

1. Petición HTTP
   - Cliente hace `POST /projects/{projectId}/tasks` con JSON body conforme a `TaskRequest`.
2. Controlador (capa REST)
   - Clase: `src/main/java/com/taskflow/controller/TaskController.java`.
   - En `TaskController.createTask` se llama `projectService.buscarPorId(projectId)` y si el Optional está vacío lanza `ProjectNotFoundException` (404).
   - El método recibe `@PathVariable Long projectId` y `@RequestBody @Valid TaskRequest request`.
3. Servicio y mapeo
   - `TaskController` delega a `src/main/java/com/taskflow/service/TaskService.java` llamando `taskService.crear(request, projectId)`.
   - En `TaskService.crear` el DTO se convierte a entidad mediante `TaskMapper.aEntidadNueva(request, projectId)` (que a su vez llama a `Task.crear`) y la entidad se persiste con `TaskRepository.save`.
4. Respuesta HTTP
   - `TaskController.createTask` construye la cabecera `Location` apuntando a `/tasks/{id}` y responde `201 Created` con el body `TaskMapper.aResponse(creada)`.
5. Errores y excepciones
   - Excepciones que suben (por ejemplo `TaskValidationException`, `ProjectNotFoundException`) son manejadas por `src/main/java/com/taskflow/advice/GlobalExceptionHandler.java` y devuelven respuestas uniformes (400, 404, 422, etc.).

---

## Dónde viven las reglas de negocio

- Reglas de ámbito de la entidad (invariantes, transiciones de estado, reglas de creación): en `src/main/java/com/taskflow/model/Task.java` (métodos como `crear(...)`, `estaVencida()`, transiciones de estado).
- Reglas de aplicación (coordinación entre repositorios, verificación cross-entity, envío de eventos, permisos específicos): en `src/main/java/com/taskflow/service/*` (por ejemplo `TaskService`).
- Evitar duplicar lógica: los controladores deben ser delgados (validación básica + delegación) y no contener reglas de negocio.

---

## Seguridad con JWT (visión general)

- Arquitectura: JWT sin estado. El endpoint `POST /auth/login` autentica credenciales y devuelve un JWT firmado.
  - Implementaciones típicas: `src/main/java/com/taskflow/security/JwtService.java` (crea/valida tokens) y `src/main/java/com/taskflow/security/JwtAuthenticationFilter.java` (intercepta requests y valida token).
  - Configuración central: `src/main/java/com/taskflow/config/SecurityConfig.java` registra filtros y reglas de autorización.
- Flujo de autorización:
  1. Cliente obtiene token desde `/auth/login`.
  2. Cliente incluye cabecera `Authorization: Bearer <token>` en las llamadas que requieren auth.
  3. `JwtAuthenticationFilter` valida el token, extrae el `username`/roles y rellena el `SecurityContext`.
  4. Los controladores y servicios usan `@PreAuthorize` o checks programáticos y `ProjectSecurity` para permisos finos (por ejemplo borrar proyecto solo propietario o `ADMIN`).
- Rutas públicas: `/auth/**`, `/info`, la consola H2 (`/h2-console/**`), Swagger y los archivos en `src/main/resources/static`.
- Importante: es stateless — no hay sesiones en servidor.

---

## Organización de tests

- Unit tests: JUnit 5 + Mockito, sin Spring. Clases como `TaskServiceTest` se ejecutan con `mvn -q test "-Dtest=TaskServiceTest"`.
- Slice tests: `@WebMvcTest`, `@DataJpaTest` para aislar capas con soporte de Spring.
- Integration tests: `@SpringBootTest` con perfil `test` o `h2`. Tests `*IT.java` usan Testcontainers y NO se ejecutan con `mvn test` a menos que se pase `-Ddocker.tests=true`.
- Comandos habituales:
  - `mvn -q test` — suite normal
  - `mvn -q test "-Dtest=TaskServiceTest"` — clase concreta
  - `mvn spring-boot:run "-Dspring-boot.run.profiles=h2"` — iniciar con H2 y `DataSeeder`
- Datos de desarrollo: con perfil `h2` la base se inicializa en memoria y `DataSeeder` inserta usuarios (`ana`, `luis`, `admin`), proyectos y tareas de ejemplo.

---

## Notas finales rápidas

- Respeta las reglas de la entidad (`Task`) — reutiliza sus métodos en lugar de replicar lógica.
- Controladores devuelven DTOs, nunca entidades.
- Transacciones en servicios.
- No modificar tests existentes para hacer que pasen; arreglar código si un test falla.

Referencias rápidas (ejemplos de rutas):
- `src/main/java/com/taskflow/controller/ProjectController.java`
- `src/main/java/com/taskflow/dto/TaskRequest.java`
- `src/main/java/com/taskflow/mapper/TaskMapper.java`
- `src/main/java/com/taskflow/service/TaskService.java`
- `src/main/java/com/taskflow/repository/TaskRepository.java`
- `src/main/java/com/taskflow/model/Task.java`
- `src/main/java/com/taskflow/advice/GlobalExceptionHandler.java`
- `src/main/java/com/taskflow/config/SecurityConfig.java`
- `src/main/java/com/taskflow/config/DataSeeder.java`

Si se necesitan diagramas o ejemplos de trazado de una petición concreta, añadiré un apartado con secuencias y extracts de código bajo pedido.Las fechas límite se validan en `Task.crear` (`src/main/java/com/taskflow/model/Task.java`).
