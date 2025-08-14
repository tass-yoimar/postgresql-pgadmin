# 📂 Carpeta `data` - Persistencia de Volúmenes

Esta carpeta contiene los volúmenes locales mapeados desde los contenedores Docker para **PostgreSQL** y **pgAdmin**.  
Su objetivo es garantizar la **persistencia de datos** entre reinicios o recreaciones de contenedores.

---

## 📁 Estructura

```
data/
├── postgres/       # Datos internos de PostgreSQL (tablas, índices, usuarios, etc.)
├── pgadmin/        # Configuración y datos de pgAdmin (servidores registrados, preferencias)
└── init-scripts/   # Scripts de inicialización (.sql / .sh) que se ejecutan al crear la base
```

---

## 📌 Notas importantes

- **No** modificar manualmente archivos dentro de `postgres/` o `pgadmin/` a menos que sepas lo que haces.
- Puedes agregar archivos `.sql` o `.sh` en `init-scripts/` para cargar datos iniciales o crear estructuras automáticamente.
- Esta carpeta está mapeada en el host para inspección y respaldo, pero las modificaciones internas deben gestionarse vía cliente SQL o pgAdmin.
- El contenido de `postgres/` y `pgadmin/` es **dependiente de la versión de la imagen Docker**; no es portable entre versiones sin un `pg_dump`.

---

## 🔄 Restaurar o reiniciar datos

- Para **limpiar todo** y empezar desde cero:
  ```powershell
  docker-compose down -v
  ```
  *(Esto borrará también el contenido de esta carpeta)*

---
