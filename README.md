# Reto de Refactorización: TechNova Cloud v2.0

**Alumno:** Álvaro López de San Román  
**Profesor:** Willman Acosta Lugo  
**Módulo:** AEE – RA6 — Almacenamiento de información en formatos de intercambio de datos  
**Tarea:** Refactorización de esquema XSD — CatalogoCloud v2.0  

---

## Cambios realizados

**Tarea A:** He creado un `attributeGroup` llamado `datosGestion` que agrupa los atributos `id`, `estado` y `ubicacion`, que antes se repetían en varios elementos del esquema.

**Tarea B:** He definido un `group` llamado `especificacionesTecnicas` con los elementos `cpu`, `ram`, `gpu` y `discos` en una `sequence`, para no tener que declararlos cada vez que se necesiten.

**Tarea C:** He añadido un `choice` dentro de `identificacion` para que el técnico elija entre `mac_address` o `id_interno`, pero nunca los dos a la vez.

**Tarea D:** He usado un `all` en `notas_mantenimiento` para que los campos `fecha`, `autor` y `comentario` puedan escribirse en cualquier orden.

**Tarea E:** He limitado los discos a un máximo de 4 con `minOccurs="0" maxOccurs="4"` y he añadido un indicador `unique` para que no puedan existir dos servidores con el mismo `id`.

---

## ¿Cuántas líneas de código hemos ahorrado al usar grupos?

Aproximadamente 24 líneas, al centralizar en un solo lugar la declaración de los atributos y elementos que antes se repetían en múltiples sitios del esquema.

---

## ¿Qué error da VS Code si intentamos poner dos servidores con el mismo ID?
```
cvc-identity-constraint.4.1: Duplicate unique value [srv-web-01] found for identity constraint "unicidadIdServidor" of element "centro_datos".
```

VS Code señala en rojo la línea del segundo servidor duplicado e impide que el documento sea válido.
