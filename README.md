# PostgreSQL 17.5 + pgAdmin 4 - Ambiente de Desarrollo en Docker

Este proyecto levanta un entorno de **PostgreSQL 17.5** y **pgAdmin 4** utilizando `docker-compose`, con volúmenes mapeados a una ruta local en Windows para persistir datos y facilitar la inspección en desarrollo.

---

## 📂 Estructura de carpetas en Windows

```
C:\Services\postgresql-pgadmin\data\
│
├── postgres\                # Almacena los datos persistentes de PostgreSQL
├── pgadmin\                 # Almacena la configuración y datos de pgAdmin
└── init-scripts\            # Scripts SQL o SH que se ejecutan al iniciar PostgreSQL
```

> **Nota:** La carpeta `init-scripts` es opcional. Si existe y contiene archivos `.sql` o `.sh`, estos se ejecutarán automáticamente la primera vez que se cree la base de datos.

---

## 🛠 Servicios definidos en `docker-compose.yml`

### 1. **PostgreSQL 17.5**
- **Imagen:** `postgres:17.5`
- **Usuario por defecto:** `postgres`
- **Contraseña:** `postgres`
- **Base de datos inicial:** `tasstech`
- **Puerto expuesto:** `5432`
- **Volúmenes:**
  - `C:/Services/postgresql-pgadmin/data/postgres:/var/lib/postgresql/data` → persistencia de datos
  - `C:/Services/postgresql-pgadmin/data/init-scripts:/docker-entrypoint-initdb.d` → scripts de inicialización

---

### 2. **pgAdmin 4 (última versión estable)**
- **Imagen:** `dpage/pgadmin4:9.6.0`
- **Email de acceso:** `development@tass.com.co`
- **Contraseña:** `development`
- **Puerto expuesto:** `5050` (interfaz web accesible en `http://localhost:5050`)
- **Volumen:**
  - `C:/Services/postgresql-pgadmin/data/pgadmin:/var/lib/pgadmin` → persistencia de configuraciones

---

## 🚀 Cómo iniciar el entorno

1. **Crear carpetas locales:**
   ```powershell
   mkdir C:\Services\postgresql-pgadmin\data\postgres
   mkdir C:\Services\postgresql-pgadmin\data\pgadmin
   mkdir C:\Services\postgresql-pgadmin\data\init-scripts
   ```

2. **(Opcional) Añadir scripts de inicialización:**
   Coloca en `init-scripts` cualquier archivo `.sql` o `.sh` que quieras ejecutar al crear la base de datos.

3. **Levantar los contenedores:**
   ```powershell
   docker-compose up -d
   ```

4. **Verificar estado:**
   ```powershell
   docker ps
   ```

---

## 🔍 Acceso y uso

- **PostgreSQL:**
  - Host: `localhost`
  - Puerto: `5432`
  - Usuario: `postgres`
  - Contraseña: `postgres`
  - Base de datos: `tasstech`

- **pgAdmin 4:**
  - URL: [http://localhost:5050](http://localhost:5050)
  - Email: `development@tass.com.co`
  - Contraseña: `development`

> Dentro de pgAdmin, deberás crear una conexión al servidor PostgreSQL usando las credenciales anteriores.

---


## 🗂 Registro de la base de datos en pgAdmin

Por defecto, pgAdmin **no detecta automáticamente** la base de datos de PostgreSQL, incluso cuando ambos están en el mismo `docker-compose`. Es necesario registrarla manualmente.

### Pasos para registrar el servidor PostgreSQL:

1. **Accede a pgAdmin:**
   - URL: [http://localhost:5050](http://localhost:5050)  
   - Email: `development@tass.com.co`  
   - Contraseña: `development`

2. **Crear un nuevo servidor:**
   - En el panel izquierdo, clic derecho en **Servers** → **Register** → **Server...**

3. **Configurar pestaña "General":**
   - **Name:** `postgres:17.5` *(o el nombre que prefieras)*

4. **Configurar pestaña "Connection":**
   - **Host name/address:** `postgres` *(nombre del servicio definido en `docker-compose.yml`)*
   - **Port:** `5432`
   - **Maintenance database:** `tasstech`
   - **Username:** `postgres`
   - **Password:** `postgress`
   - Marca la casilla **Save Password** para no introducirla cada vez.

5. **Guardar y probar conexión.**
   - Si todo está correcto, el servidor aparecerá en la lista de **Servers** y podrás navegar por bases de datos, esquemas y tablas.

---

💡 **Notas importantes:**
- Si usas otro archivo `.env`, asegúrate de poner las credenciales correctas.
- El **Host name/address** debe ser exactamente el **nombre del servicio Docker** (`postgres`), no `localhost`.
- Si cambias puertos o credenciales en el `.env`, actualiza también la configuración en pgAdmin.

---



## 🧹 Detener y limpiar

Para **detener** los contenedores:
```powershell
docker-compose down
```

Para **eliminar todos los datos** (⚠ acción irreversible):
```powershell
docker-compose down -v
```

---

## 📌 Notas finales

- Este archivo está diseñado **exclusivamente para entornos de desarrollo local**.
- No usar estas credenciales ni configuración en producción.
- Asegúrate de que Docker Desktop tenga acceso al disco C: (Config → Resources → File Sharing).

---
