<img width="1920" height="1080" alt="Captura de pantalla (522)" src="https://github.com/user-attachments/assets/d4b55443-50ec-4534-9afa-2f9b36985891" />
# 🧃 PulpApp - Sistema Distribuido (Base de Datos con Liquibase)

## 📌 Descripción General

Este módulo corresponde a la **gestión de base de datos versionada** del sistema distribuido **PulpApp**, el cual permite la venta online de pulpas naturales.

Se implementó una solución profesional utilizando:

* 🐘 PostgreSQL (Base de datos)
* 🐳 Docker (Contenerización)
* ⚙️ Liquibase (Control de versiones de la base de datos)

---

## 🎯 Objetivo

Garantizar que la base de datos:

* Sea **portable**
* Sea **reproducible en cualquier entorno**
* Permita **versionamiento y trazabilidad de cambios**
* Evite errores manuales en ejecución de scripts SQL

---

## 🏗️ Arquitectura

El sistema se ejecuta mediante contenedores Docker:

* `postgres`: Base de datos
* `liquibase`: Motor de migraciones

Liquibase se conecta automáticamente a PostgreSQL y ejecuta los cambios definidos en los archivos YAML.

---

## 📂 Estructura del Proyecto

```
BaseDeDatos/
│
├── 01_ddl/           # Creación de tablas
├── 02_dml/           # Inserción de datos
├── 03_dcl/           # Permisos (opcional)
├── 04_tcl/           # Transacciones (opcional)
├── 05_rollbacks/     # Rollbacks (opcional)
│
├── changelog-master.yaml
├── docker-compose.yml
├── liquibase.properties
└── postgresql-42.7.10.jar
```

---

## ⚙️ Configuración Docker

Archivo clave: `docker-compose.yml`

### Servicios:

### 🐘 PostgreSQL

* Puerto: `5436`
* Usuario: `postgres`
* Contraseña: `1234`
* Base de datos: `pulpapp_db`

### ⚙️ Liquibase

* Ejecuta automáticamente migraciones
* Usa el archivo `changelog-master.yaml`

---

## 🔄 Funcionamiento de Liquibase

Liquibase ejecuta los archivos en orden:

1. `01_ddl` → creación de tablas
2. `02_dml` → inserción de datos

Cada cambio queda registrado en la tabla:

```
databasechangelog
```

Esto permite:

* Saber qué cambios se ejecutaron
* Evitar duplicados
* Controlar versiones

---

## 🧪 Tablas creadas

* `category`
* `products`
* `users`
* `orders`
* `order_items`

Relaciones:

* `products → category`
* `orders → users`
* `order_items → orders`
* `order_items → products`

---

## ❌ Problemas encontrados y soluciones

### 1. Error YAML (estructura incorrecta)

**Error:**

```
ParserException: while parsing a block mapping
```

**Causa:**
Mala indentación en `changelog.yaml`

**Solución:**
Se corrigió la estructura y alineación de los bloques YAML.

---

### 2. Error de clave foránea

**Error:**

```
constraintName is required for addForeignKeyConstraint
```

**Causa:**
Faltaba el atributo `constraintName`

**Solución:**
Se añadió correctamente:

```yaml
constraintName: fk_products_category
```

---

### 3. Error en Docker Compose

**Error:**

```
additional properties 'liquibase' not allowed
```

**Causa:**
Mala indentación del servicio `liquibase`

**Solución:**
Se alineó correctamente dentro de `services:`

---

### 4. Error Liquibase (missing changelog)

**Error:**

```
Invalid argument '--changelog-file'
```

**Causa:**
Liquibase no encontraba el archivo

**Solución:**
Se configuró correctamente:

* `working_dir`
* `volumes`
* `command`

---

### 5. Error Docker entrypoint

**Error:**

```
no such file or directory
```

**Causa:**
Ruta incorrecta en volumen

**Solución:**
Se ajustó:

```yaml
volumes:
  - .:/liquibase/changelog
```

---

## ✅ Resultado Final

Liquibase ejecutó correctamente:

```
Liquibase: Update has been successful
```

* ✔ 6 changeSets ejecutados
* ✔ Tablas creadas
* ✔ Datos insertados
* ✔ Historial almacenado

---

## 🔍 Verificación en pgAdmin

Consulta:

```sql
SELECT * FROM databasechangelog;
```

Resultado:

* Registro de cambios
* Orden de ejecución
* Autor

---

## 🚀 Cómo ejecutar el proyecto

### 1. Levantar base de datos

```bash
docker compose up -d postgres
```

### 2. Ejecutar migraciones

```bash
docker compose run --rm liquibase update
```

---

## 🧠 Conclusión

Se implementó un sistema robusto de versionamiento de base de datos utilizando Liquibase, garantizando:

* Control de cambios
* Reproducibilidad
* Buenas prácticas de ingeniería

Este enfoque es utilizado en entornos profesionales y facilita la integración con arquitecturas distribuidas.

---

## 👨‍💻 Autor

**Julian Guerra**

Proyecto académico - Sistemas Distribuidos

---

🔥 Proyecto listo para entrega y sustentación 🔥
