---
title: Configuración de instalación de la aplicación Clip-to-full
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
Los propietarios de aplicaciones/anunciantes pueden atribuir descargas de usuarios de la aplicación completa desde el clip de la aplicación. Para ello, el desarrollador necesita configurar tanto el clip de aplicación como la aplicación completa.
[block:api-header]
{
"title": "Procedures"
}
[/block]
Para configurar la aplicación Clip-to-full instale la atribución en el App Clip y la aplicación completa, es necesario completar la siguiente lista de verificación de procedimientos.

**¡Importante**! Hay que seguir los mismos pasos tanto en el App Clip como en la aplicación completa.
[block:parameters]
{
"data": {
"h-0": "Procedure checklist",
"0-0": "1. En el App Clip, active el título de los grupos de aplicaciones.",
"1-0": "2. En el App Clip, crea un App Group para pasar datos entre el App Clip y la aplicación completa.",
"2-0": "3. En el App Clip, actualice la Lista de Propiedades de Información (archivo `info.plist`).",
"3-0": "4. En la aplicación completa, repite los pasos 1-3, usando el mismo nombre del grupo de aplicaciones que para el clip."
},
"cols": 1,
"fila": 4
}
[/block]
### Activar el título de Grupos de Aplicaciones

**Para activar el título de grupos de aplicaciones**:

1. En Xcode, ve a tu página de proyecto.
2. En la sección **objetivos**, selecciona tu clip de aplicación o destino completo de la aplicación (dependiendo de lo que estés configurando).
3. Haz clic en la pestaña **Firmar y capacidades**.
4. Click **+Capability**.
   Se abre el menú Capacidades.
5. En el menú, seleccione Grupos de aplicaciones.

### Crear un grupo de aplicaciones

**Crear un grupo de aplicaciones para pasar datos entre el App Clip y la aplicación completa**:

1. En Xcode, ve a tu página de proyecto.
2. En la sección **objetivos**, selecciona tu clip de aplicación o destino completo de la aplicación (dependiendo de lo que estés configurando).
3. Haz clic en la pestaña **Firmar y capacidades**.
4. En la pestaña **Firmar y capacidades**, ve a la sección **Grupos de aplicaciones**.
5. Click **+**.
   Se abre la ventana **Agregar un nuevo contenedor**.
6. En la ventana abierta, introduce un nombre para tu grupo de aplicaciones, con la siguiente convención de nombres:
   [block:code]
   {
   "codes": [
   {
   "code": "group. ourAppGroupName. ppClipToFullApp",
   "idioma": "texto"
   }
   ]
   }
   [/block]
### Ejemplo
[block:image]
{
"images": [
{
"image": [
"https://files. eadme.io/cddf2e3-App_Group_Name.png",
"Nombre del grupo de aplicaciones. ng",
524,
177,
"#394148"
]
}
]
}
[/block]
### Actualizar la Lista de Propiedades de Información

**Para actualizar la Lista de Propiedades de Información (archivo `info.plist`)**:

- En la **Lista de Propiedades de Información** (archivo `info.plist`), añade una fila con la clave y el valor como se detalla en la siguiente tabla.
  [block:parameters]
  {
  "data": {
  "h-0": "Clave",
  "h-1": "Tipo",
  "h-2": "Valor",
  "0-0": "AppsFlyerAppGroupName",
  "0-1": "string",
  "0-2": "El nombre que le diste al grupo de aplicaciones en el paso anterior."
  },
  "cols": 3,
  "fila": 1
  }
  [/block]
### Configurar la aplicación completa

**Para configurar tu aplicación completa**:

- Repetir todos los pasos anteriores para la aplicación completa.
