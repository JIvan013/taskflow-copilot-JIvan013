Plan: Implementar specs/unassigned.md

Problema
- Implementar exactamente la especificación specs/unassigned.md: endpoint GET /tasks/unassigned y lógica en TaskService.sinResponsable(), con tests unit y slice.

Enfoque propuesto
- Reusar los predicados/ordenadores existentes: ReportService.SIN_ASIGNAR y TaskOrders.POR_FECHA.
- Añadir método público List<Task> sinResponsable() en TaskService que consulte el repositorio, filtre usando SIN_ASIGNAR y ordene con POR_FECHA.
- Añadir endpoint GET /tasks/unassigned en TaskController que devuelva TaskMapper.aResponse(...) y respete seguridad existente.
- Añadir tests: Unit en TaskServiceTest (Nested SinResponsable) y slice en TaskControllerTest (MockMvc) según la spec.

Detalle del caso unitario de orden (requisito de la spec)
- El test unitario "sinResponsable_devuelveLasSinResponsableEnCualquierEstado_porFechaYSinFechaAlFinal" debe mockear repository.findAll() devolviendo, EN ESTE ORDEN:
  1) Una tarea sin responsable con dueDate = hoy + 10 días
  2) Una tarea con responsable
  3) Una tarea sin responsable sin dueDate
  4) Una tarea sin responsable con dueDate = hoy + 2 días
- El método sinResponsable() debe filtrar las sin responsable y devolverlas ordenadas por dueDate ascendente, con las sin fecha al final; la lista esperada de ids es: [id de la de 2 días, id de la de 10 días, id de la sin fecha]. El test compara la lista de ids exacta y falla si falta el .sorted(...).

Archivos clave a modificar
- src/main/java/com/taskflow/service/TaskService.java
- src/main/java/com/taskflow/controller/TaskController.java
- src/test/java/.../TaskServiceTest.java (añadir @Nested SinResponsable con el caso de orden descrito)
- src/test/java/.../TaskControllerTest.java (slice test para /tasks/unassigned)

Todos (seguimiento)
- sin-responsable-service: Implementando TaskService.sinResponsable() que usa ReportService.SIN_ASIGNAR y TaskOrders.POR_FECHA. (pending)
- sin-responsable-controller: Añadir endpoint GET /tasks/unassigned en TaskController. (pending)
- sin-responsable-service-tests: Añadir tests unitarios en TaskServiceTest (Nested SinResponsable) que verifiquen orden y vacío; incluye el caso de orden exacto descrito. (pending)
- sin-responsable-controller-test-slice: Añadir slice MockMvc en TaskControllerTest para GET /tasks/unassigned. El mock debe devolver UNA sola tarea; el test comprueba solo: status 200, el id de la tarea y que assigneeId es null. No probar ni afirmar orden en el slice. El nombre del test no debe mencionar 'orden'. (pending)
- run-tests-and-verify: Ejecutar mvn -q test y confirmar que la suite pasa. (pending)

Notas y restricciones
- No modificar tests existentes fuera de lo pedido. Solo tocar los archivos indicados si hace falta.
- Usar los utilitarios y predicados existentes conforme a la especificación (no reescribir la lógica duplicada).
- Al terminar, ejecutar mvn -q test y confirmar que pasa.
