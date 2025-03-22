Aquí tienes una versión revisada del **README.md**, más breve y centrada en documentación clara, con una sección al final explicando por qué la versión inicial no funcionaba (**Fé de erratas**):

---

# Proyecto ETL con Neo4j, PostgreSQL y Docker Compose
## Dataset B - Lenguajes de Programación

Proyecto ETL (**Extract, Transform, Load**) utilizando **Docker** y **Docker Compose**, aplicando redes internas, volúmenes y bind mounts, siguiendo especificaciones para el Dataset B proporcionado en clase.

---

## 📁 Estructura del Proyecto

```
etl_exam_datasetB/
├── docker-compose.yml
├── .env
├── neo4j/
│   └── dataset_b_lenguajes.csv
├── neo4j_custom/
│   └── Dockerfile
├── api/
│   ├── Dockerfile
│   ├── package.json
│   └── app.js
├── postgres/
│   └── init.sql
└── output/
    └── recap.csv
```

---

## ⚙️ Instalación Rápida y Uso

**1. Variables de Entorno (`.env`)**

Define contraseñas y usuarios:

```env
NEO4J_AUTH=neo4j/YourStrongPassword
NEO4J_PASSWORD=YourStrongPassword
POSTGRES_USER=postgres
POSTGRES_PASSWORD=pgPass123
POSTGRES_DB=etl_db
```

**2. Construcción y Ejecución**

Desde la raíz del proyecto ejecuta:

```bash
docker-compose up --build
```

Esto inicia tres contenedores:

- **neo4j_db**
- **postgres_db**
- **api_etl**

---

## ▶️ Procedimiento ETL

**1. Carga inicial en Neo4j**

En [http://localhost:7474](http://localhost:7474), ejecuta:

```cypher
LOAD CSV WITH HEADERS FROM 'file:///dataset_b_lenguajes.csv' AS row
CREATE (:Lenguaje {
  id: row.id,
  nombre: row.nombre,
  popularidad: toInteger(row.popularidad),
  velocidad: toInteger(row.velocidad),
  paradigma: row.paradigma,
  año_creacion: toInteger(row.año_creacion)
});
```

**2. API REST disponible en `localhost:3000`**

- **Salud de API:**  
  ```bash
  curl http://localhost:3000/health
  ```

- **Extraer datos (GET):**
  ```bash
  curl http://localhost:3000/api/extract
  ```

- **Transformar y cargar en PostgreSQL (POST):**
  ```bash
  curl -X POST http://localhost:3000/api/load
  ```

- **Exportar CSV (GET):**
  ```bash
  curl http://localhost:3000/api/export
  ```

Archivo CSV generado: `output/recap.csv`.

---

## 📌 Fé de Erratas: Soluciones a problemas encontrados en la versión inicial

Durante la implementación inicial se presentaron los siguientes inconvenientes que impidieron el correcto funcionamiento del proyecto:

- **Healthcheck fallido en Neo4j:**  
  La imagen oficial de Neo4j no incluía `curl`, necesario para verificar su salud.  
  **Solución:** Se creó una imagen personalizada de Neo4j (`neo4j_custom/Dockerfile`) con `curl` instalado.

- **Healthcheck fallido en API (alpine):**  
  La imagen `node:16-alpine` no incluye `curl`, generando fallos en los chequeos.  
  **Solución:** Se añadió la instalación de `curl` en ambas etapas del Dockerfile del servicio API.

- **Permisos de directorios incorrectos:**  
  Problemas con permisos en el directorio `neo4j/` impedían cargar archivos CSV desde Neo4j.  
  **Solución:** Se corrigieron permisos ejecutando:
  ```bash
  sudo chmod -R 755 neo4j api postgres output
  ```

- **Archivo CSV no detectado por Neo4j:**  
  La ubicación del CSV era incorrecta o faltaba durante la ejecución inicial del contenedor.  
  **Solución:** Se aseguró de copiar el archivo CSV antes de ejecutar `docker-compose up --build`.

---

## 🚧 Recomendaciones finales

- Mantener siempre actualizado y correcto el archivo `.env`.
- Siempre ubicar y otorgar permisos adecuados a los archivos y carpetas antes de lanzar los servicios.
- Para una reconstrucción limpia usa:
  ```bash
  docker-compose down -v
  docker-compose up --build
  ```

---

**Autor:** Santiago Vargas 
