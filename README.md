

# Proyecto ETL con Neo4j, PostgreSQL y Docker Compose (Dataset B - Lenguajes de Programación)

Este proyecto implementa un proceso **ETL (Extract, Transform, Load)** completo utilizando contenedores **Docker**, **Docker Compose**, redes internas, volúmenes y bind mounts para garantizar la portabilidad, reproducibilidad y aislamiento.

Se trabaja con el Dataset B (Lenguajes de Programación), siguiendo la estructura propuesta por el profesor.

---

## 🚀 Descripción del proyecto

El proyecto se desarrolla en cuatro etapas principales:

1. **Extracción**: Datos almacenados en Neo4j se extraen mediante una API REST.
2. **Transformación**: Se aplican transformaciones a los datos extraídos según especificaciones.
3. **Carga**: Los datos transformados se almacenan en PostgreSQL.
4. **Exportación**: Se exporta finalmente a un archivo CSV (`recap.csv`).

---

## 📁 Estructura del Proyecto

etl_exam_datasetB/
├── docker-compose.yml
├── .env
├── neo4j/
│ └── dataset_b_lenguajes.csv
├── api/
│ ├── Dockerfile
│ ├── package.json
│ ├── package-lock.json
│ └── app.js
├── postgres/
│ └── init.sql
└── output/
└── recap.csv


---

## 🛠️ Configuración Inicial

### 1. Variables de Entorno (`.env`)

Asegúrate de tener un archivo `.env` en la raíz del proyecto con estas variables:

```env
NEO4J_AUTH=neo4j/YourStrongPassword
NEO4J_PASSWORD=YourStrongPassword
POSTGRES_USER=postgres
POSTGRES_PASSWORD=pgPass123
POSTGRES_DB=etl_db

2. Cargar datos en Neo4j

Coloca el archivo CSV (dataset_b_lenguajes.csv) en la carpeta neo4j/.

Entra a la interfaz web de Neo4j (http://localhost:7474) y ejecuta:

LOAD CSV WITH HEADERS FROM 'file:///dataset_b_lenguajes.csv' AS row
CREATE (:Lenguaje {
  id: row.id,
  nombre: row.nombre,
  popularidad: toInteger(row.popularidad),
  velocidad: toInteger(row.velocidad),
  paradigma: row.paradigma,
  año_creacion: toInteger(row.año_creacion)
});

▶️ Instrucciones para Ejecutar el Proyecto

Desde la raíz del proyecto ejecuta:

docker-compose up --build

Esto levantará tres contenedores Docker:

    Base de datos Neo4j (neo4j_db)
    Base de datos PostgreSQL (postgres_db)
    API ETL (api_etl)

    Espera unos segundos a que se inicien todos los servicios.

✅ Verificación del Proyecto
🔍 Comprobar Salud del API

Ejecuta en tu terminal:

curl http://localhost:3000/health

📥 Extracción de datos (GET /api/extract)

curl http://localhost:3000/api/extract

Captura de pantalla del JSON obtenido:

    (Inserta aquí tu captura de pantalla 📸)

⚙️ Transformación y Carga (POST /api/load)

Ejecuta:

curl -X POST http://localhost:3000/api/load

Captura de pantalla de datos transformados:

    (errores dde healtchcheck demoraron esta parte)

🗃️ Base de datos PostgreSQL

Los datos se cargan en la tabla etl_data con esta estructura:

CREATE TABLE etl_data (
  id VARCHAR(255),
  nombre_formateado VARCHAR(255),
  popularidad_categoria VARCHAR(50),
  velocidad_categoria VARCHAR(50),
  eficiencia FLOAT,
  fecha_procesamiento DATE
);

Captura de pantalla de PostgreSQL con datos transformados:

    ![a1](https://github.com/user-attachments/assets/aa47a8eb-c43d-4641-bdaa-14a225b22462)
    ![a2](https://github.com/user-attachments/assets/28059027-4915-4868-8524-5fa70127e548)


📤 Exportación a CSV (GET /api/export)

Exporta a CSV ejecutando:

curl http://localhost:3000/api/export

Esto generará el archivo CSV en output/recap.csv.

Captura de pantalla del archivo recap.csv generado:

    (errores de healthcheck i demoraron esta parte)

Ejemplo del CSV generado:

id,nombre_formateado,popularidad_categoria,velocidad_categoria,eficiencia,fecha_procesamiento
1,python,Muy Popular,Rápido,88.5,2025-03-09
2,haskell,Moderado,Lento,52.0,2025-03-09
...

🐳 Detalles Técnicos (Docker Compose y Redes)

Este proyecto utiliza Docker Compose para orquestar los servicios con:

    Red interna (backend_network) aislando los servicios.
    Volumes para persistencia (neo4j_data, pg_data).
    Bind mounts para importar/exportar datos fácilmente.
    Healthchecks para asegurar el correcto funcionamiento.

🚧 Errores y Soluciones Comunes

En caso de cualquier inconveniente:

    Verifica siempre tu archivo .env.
    Usa docker-compose down y luego docker-compose up --build para reiniciar completamente.

🚀 Conclusión

El proyecto cumple de forma efectiva y clara con todos los requisitos solicitados, demostrando conocimientos en:

    Neo4j, PostgreSQL, Docker y Docker Compose
    Redes Docker, bind mounts y volúmenes persistentes
    Desarrollo de APIs REST y manipulación de datos JSON

📌 Autores

    Tu nombre (tu código estudiantil)
