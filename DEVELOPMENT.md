# Margay Studio — desarrollo de la web

## Ramas

- `main`: producción. No fusionar cambios hasta aprobación de Santiago y Luis.
- `develop`: integración del rediseño. Trabajar y revisar cambios aquí.

## Vista previa en Netlify (pendiente de configurar)

1. En Netlify, abrir **Site configuration → Build & deploy → Continuous deployment → Branches and deploy contexts → Configure**.
2. Habilitar **Branch deploys** para la rama `develop` únicamente.
3. Confirmar la URL de branch deploy generada por Netlify (habitualmente `develop--<nombre-del-sitio>.netlify.app`; no asumir el nombre hasta comprobarlo).
4. Antes de compartir el enlace, revisar si el contenido es apto para ser accesible públicamente. Una URL distinta **no implica acceso privado**. Configurar protección de acceso si se requiere privacidad real.
5. No cambiar `Production branch`: debe seguir siendo `main`.

## Contenido y confidencialidad

- Distinguir funcionalidades reales de propuestas o prototipos.
- Usar logos y referencias nominadas solo con autorización (GPU, PyP y Servicial según la planificación).
- No publicar nombres ni logos de Santander, Rivera o Colonia sin aprobación.
- Validar textos y casos antes de pasar a `main`.

## Próximos pasos

- Revisar estructura del sitio y código existente antes de modificarlo.
- Implementar el rediseño incrementalmente en `develop`.
- Configurar vista previa y verificar build y navegación.
- Aprobar en conjunto y fusionar a `main` solo al lanzar.
