# outer-joins-ministore

Auditoría de Inventario y Ventas — MiniStore

 1. ¿Por qué usaste LEFT JOIN para la Consulta 1 y no INNER JOIN? ¿Qué se perdería si usaras INNER JOIN?
Se utilizó `LEFT JOIN` con la tabla `productos` a la izquierda porque el objetivo de negocio era preservar la totalidad del catálogo de productos, sin importar si registraban o no transacciones asociadas. 

Si se hubiera utilizado un `INNER JOIN`, la consulta habría descartado automáticamente aquellos productos cuyo `producto_id` no tuviera coincidencia en la tabla `ventas`. En este dataset en concreto, se habrían perdido los productos **108 (Hub USB-C 7p)** y **109 (Parlante Bluetooth)**, imposibilitando identificar los artículos sin rotación.


 2. ¿Por qué usaste RIGHT JOIN para la Consulta 2? ¿Qué tabla está a la izquierda y cuál a la derecha en tu consulta?
En la Consulta 2, la tabla a la izquierda es `productos` y la tabla a la derecha es `ventas`. Se utilizó un `RIGHT JOIN` para forzar que la consulta devuelva la totalidad de las filas contenidas en la tabla `ventas` (derecha), buscando coincidencias en `productos` (izquierda).

Esta estructura permite detectar ventas registradas cuyo `producto_id` no existe en la base del catálogo (registros huérfanos), algo prioritario para auditar errores de carga de datos o inconsistencias transaccionales.


 3. ¿Qué representan los valores NULL en cada resultado? Explicá con un ejemplo concreto de los datos.
* **`venta_id` es NULL en la Consulta 1:** Representa un **producto sin rotación comercial**. Por ejemplo, para el `producto_id = 108` ('Hub USB-C 7p'), la presencia de `NULL` en los campos de la tabla `ventas` confirma que el producto existe físicamente/digitalmente en el inventario pero nunca ha sido comprado.
* **`producto_id` (de productos) es NULL en la Consulta 2:** Representa una **inconsistencia / registro huérfano**. En el caso de la `venta_id = 10`, la venta registra un `producto_id = 999`. Dado que 999 no existe en la tabla `productos`, el JOIN no encuentra coincidencia y puebla los datos del producto con `NULL`, evidenciando un error en el sistema emisor de la transacción.


 4. ¿Cuándo usarías FULL OUTER JOIN en un caso real de negocio?
Un `FULL OUTER JOIN` se aplica principalmente en procesos de **auditoría integral de calidad de datos (Data Quality & Data Cleaning)** o durante **migraciones entre sistemas legados e históricos**. 

Permite obtener en un único dataset consolidado el universo completo de ambas entidades: los registros de la izquierda sin coincidencia a la derecha, los de la derecha sin coincidencia a la izquierda y los cruzados exitosamente, facilitando detectar fallas de integridad referencial antes de alimentar paneles de BI o modelos de Machine Learning.
Permite obtener en un único dataset consolidado el universo completo de ambas entidades: los registros de la izquierda sin coincidencia a la derecha, los de la derecha sin coincidencia a la izquierda y los cruzados exitosamente, facilitando detectar fallas de integridad referencial antes de alimentar paneles de BI o modelos de Machine Learning.
