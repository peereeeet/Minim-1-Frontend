## MINIM 1 FRONTEND

Se ha implementado un sistema de valoraciones para los eventos
del proyecto, permitiendo a los usuarios (sin autenticación)
añadir puntuaciones de 1 a 5 estrellas con comentario opcional.

Cada evento muestra sus valoraciones y el promedio (Media) junto
con el número total de valoraciones. Los datos se guardan en una
colección específica de MongoDB ("valoracions") relacionada con
la colección de eventos.

• Eliminación del campo "usuario":
  Inicialmente las valoraciones estaban asociadas a un usuario,
  pero se decidió simplificar el modelo para que cualquier persona
  pueda valorar sin autenticarse. Por tanto, el esquema de la 
  colección Valoracion solo incluye:
  - evento (ObjectId)
  - puntuacion (1..5)
  - comentario (opcional)
  - createdAt / updatedAt

• Revisión de índices en MongoDB:
  Se detectó un índice único antiguo (evento, usuario) que impedía
  insertar más de una valoración por evento. Se eliminó manualmente
  desde MongoDB Compass para permitir múltiples valoraciones.

• Actualización de agregados:
  Cada vez que se crea o elimina una valoración, el backend recalcula
  el promedio (avgRating) y el número total (ratingsCount) en el 
  documento del evento mediante un pipeline de agregación.

• Estructura del backend:
  - Nueva colección: valoracions
  - Nuevo modelo Mongoose: Valoracion
  - Nuevo servicio (valoracionServices.ts)
  - Nuevo controlador y rutas REST (valoracionController.ts, valoracionRoutes.ts)
  - Endpoints:
      POST   /api/ratings/event/:eventoId
      GET    /api/ratings/event/:eventoId
      PUT    /api/ratings/:id
      DELETE /api/ratings/:id

• Cambios en el frontend (Angular):
  - Nuevo componente standalone: ValoracionComponent
  - Nuevo servicio: ValoracionService
  - Nuevo modelo: valoracion.model.ts
  - Nueva ruta: /events/:id/ratings
  - Actualización de evento.component.html para enlazar
    la vista de valoraciones desde el botón “Ver / Valorar”.

• Visuales:
  - Sistema de estrellas con ★ y ☆ dinámicas (hover).
  - Modal de confirmación de borrado similar al de eventos.
  - Encabezado con "Actividad", "Media" y "Valoraciones" en línea.