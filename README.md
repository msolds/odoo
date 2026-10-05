# Entorno para probar la instalación de Odoo en un contenedor
## 1. Iniciar sesión en [GitHub](https://github.com/)
## 2. Crear un Repositorio nuevo 
## 3. Crear un codespace 
## 4. Añadir un fichero docker-compose.yml con el siguiente contenido
```yaml
services:
  db:
    container_name: postgres
    image: postgres:16    
    environment:
      - POSTGRES_DB=postgres
      - POSTGRES_USER=odoo
      - POSTGRES_PASSWORD=odoo
    volumes:
      - postgres_data:/var/lib/postgresql/data

  odoo:
    container_name: odoo
    image: odoo:19
    ports:
      - "8069:8069"
    depends_on:
      - db
    environment:
      - HOST=db
      - USER=odoo
      - PASSWORD=odoo      
    volumes:
      - odoo_data:/var/lib/odoo      
      - ./odoo/custom_addons:/mnt/extra-addons

volumes:
  postgres_data:
  odoo_data:
```
## 5. Ejectar el comando
``` bash
docker compose up -d
```
