# Guía de Creación de Edificios para el Workshop

¡Bienvenido, Creador! ¡Gracias por tu interés en crear contenido personalizado! ¡Tu contribución ayudará a que la comunidad prospere! Esta guía te ayudará a crear contenido que cumpla con las especificaciones y subirlo al Workshop de Steam.

## Requisitos Técnicos
La subida de contenido depende de dos scripts: BuildingData y WorkshopUploadTool. BuildingData se utiliza para rellenar los datos del edificio, y WorkshopUploadTool se utiliza para la subida. También necesitas Steamworks.NET. Si necesitas actualizar Steamworks.NET, puedes descargarlo desde la página de GitHub: https://github.com/rlabrecque/Steamworks.NET/releases, importarlo a Unity y adjuntar el script Steam Manager a un GameObject vacío.
He empaquetado los scripts necesarios y Steamworks.NET en un archivo de paquete Unity. Puedes usar este archivo de paquete o descargarlos por separado: Enlace XX

### Especificaciones del Modelo de Edificio

> ⚠️ **Recordatorio de Componentes Requeridos**: Los prefabs de edificio deben contener los siguientes tres componentes principales. La falta de cualquiera de ellos causará problemas de funcionamiento:
> - **Mesh Filter**: Muestra la malla del modelo
> - **Mesh Renderer**: Renderiza el modelo (requiere asignación de material)
> - **Collider** (Box Collider): Se usa para el resaltado al pasar el mouse, detección de clics y detección de colisión de colocación

### Formatos de Recursos Soportados

- **Modelos**: FBX
- **Texturas**: PNG
- **Materiales**: Unity URP Lit Shader

### Límites de Tamaño de Archivo

- AssetBundle de edificio individual: ≤ 50 MB
- Imagen de vista previa: ≤ 1 MB
- Tamaño total de descarga: ≤ 100 MB

## Directrices de Diseño de Edificios

### 1. Especificaciones de Tamaño

Diseña según el tamaño de cuadrícula ocupado por el edificio. El tamaño de la base del edificio (cimiento) debe ser de al menos 1 metro de largo y ancho:
Para que la ocupación de cuadrícula del edificio se calcule correctamente, se recomienda usar cuadrados impares para el tamaño de la base del edificio, como: 1×1, 3×3, 5×5. No se recomiendan los cuadrados pares y las formas no cuadradas, como: 1×2, 2×2, 2×3, 4×4.
Altura: Sin límite estricto, pero se recomienda no menos de 0,1 metros

### 2. Estilo Visual

Para mantener la coherencia del juego, se recomienda:
- Usar estilo LowPoly
- Asegurar las proporciones generales y las relaciones de escala del edificio (referencia de escala predeterminada del juego: altura del personaje 0,1 metros)
- Usar diseños amigables para vista isométrica
- Evitar detalles excesivamente pequeños (difíciles de ver a distancia)
- Usar contornos claros y contraste de color
- Considerar los detalles desde diferentes ángulos

### 3. Recomendaciones de Optimización de Rendimiento

- Controlar razonablemente el conteo de polígonos
- Materiales transparentes (usar Alpha Test en lugar de Alpha Blend)

## Proceso de Creación

### Paso 1: Modelado en Software 3D

1. Crear modelos usando Blender, Maya, 3ds Max, etc.
2. Asegurarse de que el origen del modelo esté en el centro inferior
Usando Blender como ejemplo:
✅ Correcto: El punto central del edificio está en el origen mundial
((Imagen 1))
❌ Incorrecto: El edificio está desplazado o la rotación no está alineada
((Imagen 2))
3. Aplicar todas las transformaciones (Scale, Rotation)
4. Exportar en formato FBX

### Paso 2: Importar a Unity

1. Arrastrar el FBX al proyecto Unity
2. Establecer la Position del Transform del prefab del edificio en (0, 0, 0)
3. Asegurarse de que la base del edificio esté en el plano Y=0
4. El edificio debe mirar hacia la dirección positiva del eje Z

### Paso 3: Configurar Materiales y Texturas

1. Crear materiales (usando URP Lit Shader)
2. Ajustar los parámetros de los materiales

### Paso 4: Añadir Componentes Requeridos

Asegurarse de que el prefab del edificio contiene los siguientes tres **componentes requeridos**:

#### 1. Mesh Filter y Mesh Renderer

1. Seleccionar el objeto raíz del edificio
2. Confirmar que se ha añadido el componente **Mesh Filter** (si no, hacer clic en Add Component → Mesh Filter)
3. Confirmar que se ha añadido el componente **Mesh Renderer**
4. Asignar materiales en el Mesh Renderer (usar Standard o URP Lit Shader)

> ⚠️ **Importante**: ¡Los edificios sin Mesh Renderer no se mostrarán y no activarán el efecto de resaltado al pasar el mouse!

#### 2. Collider

1. Seleccionar el objeto raíz del edificio
2. Añadir un componente **Box Collider** (recomendado) o **Mesh Collider**
3. Ajustar el tamaño del Collider para cubrir todo el edificio
4. Asegurarse de que el Collider no se extienda más allá del rango del edificio, ya que esto causará colisión con otros edificios e impedirá la colocación
((Imagen 3))

> ⚠️ **Importante**: ¡Collider es una condición necesaria para el resaltado al pasar el mouse y la detección de clics! Los edificios sin Collider no pueden ser seleccionados o interactuados por los jugadores.

### Paso 5: Crear Prefab

1. Arrastrar el edificio a la ventana Project para crear un Prefab
2. Convención de nomenclatura: `{NombreEdificio}_Prefab`
3. Probar que el prefab puede instanciarse normalmente

### Paso 6: Crear BuildingData

1. Clic derecho → **Create → City Builder → Building Data**
2. Rellenar los siguientes campos:

| **Building Name** | Nombre para mostrar del edificio, puede usar nombre o número | Ej.: Ferriswheel, SSmbl2 |
| **Building ID** | Identificador único, se recomienda usar nombre de estilo más número, o número puro | Ej.: SunsetCity_01, SSmbl2 |
| **Style** ⭐ | **Determina en qué pestaña de estilo aparece el edificio en el menú de construcción del juego**, se recomienda usar tu nombre de desarrollador, nombre de paquete o nombre de tema. Los edificios de la misma serie deben tener el mismo valor | Ej.: Lyon'sPack, SunsetCity |
| **Category** ⭐ | **Determina en qué columna de categoría aparece bajo ese estilo**, puede usar categorías integradas del juego para compatibilidad (ver tabla abajo), o rellenar según tu propia clasificación de edificios | Ej.: Large, Residential |
| **Grid Size** | Tamaño de ocupación de cuadrícula del edificio | Ej.: 3x3 |
| **Building Prefab** | Arrastra tu prefab de edificio | — |

**Referencia de Categorías Integradas del Juego**:

| `Large` | Grande |
| `Medium` | Mediano |
| `Small` | Pequeño |

> ⚠️ **Los campos Style y Category son muy importantes**: Se empaquetarán en `metadata.json` (escrito automáticamente por la herramienta de subida). Cuando el juego carga tu mod, usa estos dos campos para determinar la posición del edificio en el menú. Asegúrate de rellenarlos, de lo contrario el edificio puede no categorizarse y mostrarse correctamente.

3. Crear un icono de edificio (PNG 128×128 recomendado), arrastrarlo al campo **Icon**

### Paso 7: Probar el Edificio

1. Probar el edificio en el juego:
   - ¿Se puede colocar correctamente?
   - ¿La rotación funciona normalmente?
   - ¿La detección de colisión es correcta?
   - ¿Estás satisfecho con el efecto visual?
2. Ajustar hasta estar satisfecho

### Paso 8: Preparar la Subida

- Crear una imagen de vista previa (600×600 recomendado)
- Confirmar que **Style** y **Category** en BuildingData están correctamente rellenados (la herramienta de subida los leerá automáticamente)
Lista de verificación pre-subida:
   - [ ] El conteo de triángulos del modelo está dentro de los límites
   - [ ] La resolución de texturas es razonable
   - [ ] Contiene el componente **Mesh Filter**
   - [ ] Contiene el componente **Mesh Renderer** (material asignado)
   - [ ] Contiene el componente **Collider**
   - [ ] El prefab puede instanciarse normalmente
   - [ ] El edificio está alineado a la cuadrícula
   - [ ] No hay materiales o texturas faltantes
   - [ ] Todos los campos de BuildingData están rellenados
   - [ ] La imagen de vista previa es nítida
   - [ ] Pasó la prueba en el juego

## Paso 9: Subir al Workshop

> ⚠️ **La subida se completa en dos pasos**, ya que el empaquetado de AssetBundle y la subida a Steam tienen diferentes requisitos para el estado del editor, por lo que deben realizarse por separado.

### Primer Paso: Empaquetar Recursos (Modo Non-Play)

1. Asegurarse de estar en **Modo Non-Play** (el editor no está ejecutándose)
2. Abrir **City Builder → Workshop Upload Tool**
3. Seleccionar tipo de mod: **Building**
4. Arrastrar **BuildingData** (la herramienta leerá automáticamente Style y Category, se puede modificar manualmente)
5. Arrastrar **prefab de edificio**
6. Rellenar información del workshop (principalmente el título, la descripción se puede editar en el Workshop)
7. Hacer clic en **"Primer Paso: Empaquetar recursos como AssetBundle"**
8. Cuando veas el mensaje **"¡Empaquetado completado! Por favor entra en modo Play y haz clic en el segundo paso para subir"**, fue exitoso

> 💡 La ruta de empaquetado se guardará automáticamente y no se perderá después de entrar en modo Play, no necesitas volver a ingresar la información.

### Segundo Paso: Subir a Steam (Modo Play)

1. Hacer clic en el botón **Play** del editor Unity, esperar la inicialización de SteamManager
2. Confirmar en la ventana de la herramienta que Steam está inicializado (sin mensajes de advertencia)
3. Hacer clic en **"Segundo Paso: Subir al Workshop de Steam"**
4. Esperar a que se complete la subida, anotar el **ID de elemento del Workshop** mostrado (para futuras actualizaciones)

> 💡 La herramienta de subida generará automáticamente `metadata.json` y lo empaquetará en el directorio de contenido AssetBundle, que contiene información de Style y Category. Después de que los jugadores se suscriban, el juego lo mostrará automáticamente en la categoría correcta.

### Paso 10: Publicar y Mantener

1. Completar la información en la página del Workshop de Steam
2. Añadir más capturas de pantalla

## Notas sobre Materiales de Cielo y Suelo
- Los shaders de cielo y suelo deben usar el pipeline URP
- Los shaders de suelo preferiblemente deben usar normales procedurales para manejar ajustes dinámicos del tamaño del área de cuadrícula

## Errores Comunes y Soluciones

### Error 0: Error de empaquetado "Building AssetBundles while in play mode is not allowed"

**Causa**: Se hizo clic en el botón de empaquetado en modo Play, pero el empaquetado de AssetBundle solo puede ejecutarse en modo Non-Play

**Solución**:
1. Salir del modo Play (hacer clic en el botón de detener)
2. Volver a hacer clic en "Primer Paso: Empaquetar recursos como AssetBundle" en la herramienta
3. Entrar en modo Play después de completar el empaquetado para subir

### Error 1: El Edificio Se Muestra Rosa

**Causa**: Material o shader faltante

**Solución**:
1. Asegurarse de que todas las texturas están correctamente asignadas
2. Verificar si los materiales están incluidos en el AssetBundle

### Error 2: El Edificio No Se Puede Colocar

**Causa**: Faltan componentes requeridos (Mesh Filter/Mesh Renderer/Collider) o error en la configuración del tamaño de cuadrícula

**Solución**:
1. Confirmar que el prefab del edificio contiene los componentes **Mesh Filter**, **Mesh Renderer** y **Collider**
2. Verificar si el Mesh Renderer tiene materiales asignados
3. Añadir Box Collider al objeto raíz del edificio
4. Verificar la configuración de Grid Size en BuildingData
5. Confirmar las dimensiones del edificio

### Error 3: Desplazamiento de Posición del Edificio

**Causa**: La configuración del Transform del prefab es incorrecta

**Solución**:
1. Establecer la Position del prefab en (0, 0, 0)
2. Asegurarse de que la base del edificio está en el plano Y=0
3. Verificar la posición del Pivot Point del modelo

### Error 4: Desplazamiento del Edificio Después de Rotar

**Causa**: El punto central del edificio no está en el centro de la cuadrícula

**Solución**:
1. Ajustar el origen del modelo en el software 3D
2. O ajustar la posición de los objetos hijos en Unity
3. Asegurarse de que el edificio rota alrededor del punto central

### Error 5: Archivo Demasiado Grande para Subir

**Causa**: Resolución de textura demasiado alta o modelo demasiado complejo

**Solución**:
1. Reducir la resolución de texturas (ej.: de 4K a 2K)
2. Optimizar el modelo para reducir el conteo de polígonos
3. Comprimir texturas (usar formato DXT5 o BC7)
4. Eliminar detalles innecesarios

## Técnicas Avanzadas

### Añadir Animaciones

Si tu edificio contiene animaciones (como un molino de viento giratorio):
1. Crear un Animator Controller, arrastrar la animación al controlador
2. Añadir un componente Animator al prefab
3. Arrastrar el Animator Controller al componente Animator

### Usar el Sistema LOD

Adjuntar scripts de culling de detalles para edificios grandes:
1. Añadir el script BuildingDetailCulling
2. Asignar diferentes niveles LOD al modelo
3. Establecer distancias de culling para diferentes niveles

### Efectos de Material Personalizados

Usar Shader Graph para crear efectos especiales:
1. Crear un asset Shader Graph
2. Diseñar efectos personalizados (como ventanas brillantes)
3. Crear un material que use ese shader
4. Asegurarse de que el shader es compatible con la plataforma objetivo

## Conclusión

Gracias por tus contribuciones a la creación de contenido para la comunidad. ¡Espero que el juego tenga una vitalidad duradera a través de tu contenido!

¡Espero ver tus creaciones añadir más posibilidades coloridas al juego!

---

**Versión**: 1.0  
**Última Actualización**: 2026  
**Aplicable a**: Unity 2022.3 LTS
