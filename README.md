# Entorno para probar la instalación de Odoo en un contenedor
## 1. Iniciar sesión en [GitHub](https://github.com/)
## 2. Crear un Repositorio nuevo 
## 3. Añadir un fichero docker-compose.yml con el siguiente contenido
```yaml
# compose.yml
services:
  db:
    container_name: postgres
    image: postgres:15
    volumes:
      - ./odoo/postgres:/var/lib/postgresql/data
    environment:
      - POSTGRES_DB=postgres
      - POSTGRES_PASSWORD=odoo
      - POSTGRES_USER=odoo
    ports:
      - "5432:5432"

  adminer:
    container_name: adminer
    image: adminer
    ports:
      - "8080:8080"
    depends_on:
      - db

  odoo:
    container_name: odoo
    user: 1000:1001
    image: odoo:18
    ports:
      - "8069:8069"
    depends_on:
      - db
    volumes:
      - ./odoo/config:/etc/odoo
      - ./odoo/addons:/mnt/extra-addons
      - ./odoo/custom_addons:/mnt/custom_addons
      - ./odoo/odoo-data:/var/lib/odoo
      - ./odoo/customs:/customs
    environment:
      - HOST=db
      - USER=odoo
      - PASSWORD=odoo
      - POSTGRES_DB=postgres
```
## 4. Ejectar el comando
``` bash
docker compose up -d
```
