# Auditoría de Infraestructura Cloud — XPath Notebook

**Módulo:** AEE – RA6 (Almacenamiento de información en formatos de intercambio de datos)  
**Ciclo:** 1 DAM — Desarrollo de Aplicaciones Multiplataforma  
**Alumno:** Álvaro López de San Román  
**Profesor:** Willman Acosta Lugo  

---

## Descripción

Este proyecto consiste en una auditoría de infraestructura cloud mediante consultas XPath sobre un catálogo XML de servidores de la empresa ficticia TechNova Cloud.

Se utiliza VS Code con la extensión XPath Notebook de DeltaXML para ejecutar las consultas directamente sobre el archivo `CatalogoCloud.xml`, previamente validado por su esquema `CatalogoCloud.xsd`.

El objetivo es demostrar el uso de XPath como lenguaje de consulta sobre documentos XML de intercambio de datos, correspondiente al criterio de evaluación CE 6.e y CE 6.i del módulo AEE.

---

## Archivos del repositorio

| Archivo | Descripción |
|--------|-------------|
| `auditoria_lopez_alvaro.xbook` | Notebook con las 5 misiones XPath resueltas |
| `CatalogoCloud.xml` | Documento XML con el inventario de servidores por centros de datos |
| `CatalogoCloud.xsd` | Esquema XSD que valida la estructura del XML |
| `README.md` | Este documento explicativo |

---

## Misiones de Auditoría

### Misión 1 — Mapeo de Seguridad (Navegación Absoluta/Relativa)

```xpath
/catalogo_cloud/centro_datos[@ubicacion="Paris"]//servicio/@puerto
```

He utilizado una ruta absoluta desde la raíz del documento hasta `centro_datos`, filtrando por el atributo `ubicacion` con valor "Paris". Desde ahí se navega con `//` de forma relativa hasta todos los elementos `servicio` y se extrae su atributo `puerto`. He usado `//` en lugar de la ruta exacta porque los servicios están anidados dentro de varias etiquetas intermedias.

**Resultado:** Se obtienen 3 puertos — `8888` (Jupyter Hub), `22` (OpenSSH) y `873` (Rsync), correspondientes a los dos servidores del centro de datos de París.

---

### Misión 2 — Auditoría de Mantenimiento (Filtrado por Valores)

```xpath
/catalogo_cloud//servidor[@id="srv-db-01"]/software/so/@version
```

Se localiza el servidor filtrando por su atributo `id` con valor exacto "srv-db-01". A partir de ese nodo se navega directamente hasta el atributo `version` del elemento `so`. El uso de `//` antes de `servidor` permite encontrarlo sin importar en qué centro de datos esté ubicado.

**Resultado:** Se obtiene el valor `15`, correspondiente a la versión de Debian GNU/Linux instalada en ese servidor.

---

### Misión 3 — Inventario de Alta Capacidad (Predicados Matemáticos)

```xpath
//disco[@capacidad_tb >= 8]
```

Se buscan todos los elementos `disco` en cualquier punto del documento que tengan un atributo `capacidad_tb` con valor numérico igual o superior a 8. El predicado matemático `>=` permite comparar directamente el valor del atributo como número.

**Resultado:** Se obtienen 2 nodos completos — el disco NVMe de 8 TB del servidor `srv-db-01` y el disco HDD de 50 TB del servidor `srv-backup-01`.

---

### Misión 4 — Eficiencia Energética (Funciones / Índices)

```xpath
(//servidor[@estado="apagado"])[1]
```

Se obtienen todos los servidores con atributo `estado` igual a "apagado" y se agrupan entre paréntesis para formar un conjunto de nodos. El índice `[1]` selecciona únicamente el primero en orden de aparición en el documento. Sin los paréntesis, el índice se aplicaría de forma incorrecta a cada contexto por separado.

**Resultado:** Se obtiene el nodo completo del servidor `srv-backup-01`, que es el único servidor apagado en todo el catálogo.

---

### Misión 5 — Desafío del Auditor Senior (Navegación Inversa / Existencia)

```xpath
//servidor[hardware/gpu]/hardware/cpu/@arquitectura
```

Se filtra el servidor que contenga un elemento `gpu` dentro de `hardware`, usando la existencia del nodo como condición del predicado. Una vez localizado ese servidor, se navega hasta el atributo `arquitectura` de su CPU. Este enfoque evita tener que conocer el identificador del servidor, localizándolo por una característica estructural de su hardware.

**Resultado:** Se obtiene el valor `x86_64`, correspondiente al procesador AMD EPYC del servidor `srv-ia-01`, que es el único que dispone de GPU en toda la infraestructura.

---

## Cómo reproducir las consultas tal cual lo he hecho

1. Clona o descarga este repositorio
2. Abre la carpeta en VS Code
3. Instala la extensión XPath Notebook de DeltaXML
4. Abre el archivo `auditoria_lopez_alvaro.xbook`
5. Haz clic derecho sobre `CatalogoCloud.xml` y selecciona "Set as XPath Notebook Context Item"
6. Pulsa "Run All" para ejecutar todas las consultas
