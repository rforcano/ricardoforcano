# Despliegue de ricardoforcano.es en GitHub Pages

Guía paso a paso para publicar la web en el dominio `ricardoforcano.es`.

---

## Resumen del camino

1. Registrar el dominio en **Dondominio**
2. Crear un repositorio en **GitHub** y subir los archivos
3. Activar **GitHub Pages** y configurarlo con el dominio personalizado
4. Configurar los registros DNS en Dondominio
5. Esperar propagación (5 min – 24 h) y activar HTTPS

---

## 1. Registrar `ricardoforcano.es` en Dondominio

1. Ve a [www.dondominio.com](https://www.dondominio.com) y busca `ricardoforcano.es`.
2. Añádelo al carrito y completa la compra (≈ 6-8 €/año para .es).
3. Confirma desde el email de verificación si te lo piden.

**Antes de configurar DNS, espera a que el dominio aparezca activo en tu panel de Dondominio.**

---

## 2. Crear el repositorio en GitHub

1. Entra a [github.com](https://github.com) con tu cuenta.
2. **New repository**:
   - **Name**: `ricardoforcano-es` (o el que prefieras; no necesita coincidir con el dominio)
   - **Visibility**: Public (GitHub Pages gratis solo permite repos públicos en cuentas Free)
   - Sin README, sin .gitignore (los añadiremos opcionalmente)
3. Click **Create repository**.

### Subir los archivos

**Opción A — Drag & drop por web (más sencillo, sin git)**

1. En la página del repo recién creado, click en **"uploading an existing file"**.
2. Arrastra **todo el contenido** de la carpeta `Web personal` excepto los `.docx` (los dos archivos de inventario no son parte de la web):
   - `index.html`
   - `CNAME`
   - Todos los `.jpg`, `.jpeg`, `.pdf`
3. Mensaje de commit: *"Initial site"* → **Commit changes**.

**Opción B — Por git/terminal (si lo manejas)**

```bash
cd ~/Documents/Claude/Projects/Web\ personal
git init
git add .
git commit -m "Initial site"
git branch -M main
git remote add origin https://github.com/TU-USUARIO/ricardoforcano-es.git
git push -u origin main
```

(Sustituye `TU-USUARIO`).

> El archivo `CNAME` que he creado en la carpeta ya contiene `ricardoforcano.es`. GitHub lo reconocerá automáticamente al activar Pages.

---

## 3. Activar GitHub Pages

1. En el repo, ve a **Settings** (pestaña arriba a la derecha).
2. En el menú lateral izquierdo, **Pages**.
3. **Build and deployment**:
   - **Source**: *Deploy from a branch*
   - **Branch**: `main`, **Folder**: `/ (root)` → **Save**
4. Espera 1-2 minutos. Verás un mensaje verde con la URL `https://TU-USUARIO.github.io/ricardoforcano-es/`. Ya puedes comprobar que la web carga ahí (sin imágenes de la carpeta padre, todo relativo).
5. En el campo **Custom domain** escribe: `ricardoforcano.es` → **Save**.
   - GitHub avisará que falta configurar el DNS. Continúa en el paso siguiente.

---

## 4. Configurar los DNS en Dondominio

Entra a tu panel de Dondominio → **Mis dominios** → `ricardoforcano.es` → **Servidor DNS / Zona DNS**.

### Borra cualquier registro A o CNAME que tenga el dominio por defecto (parking)

### Añade estos registros A para el apex (`@` o ricardoforcano.es)

Apuntan a los servidores de GitHub Pages:

| Tipo | Nombre / Subdominio | Valor              | TTL  |
|------|--------------------|--------------------|------|
| A    | @                  | `185.199.108.153`  | 3600 |
| A    | @                  | `185.199.109.153`  | 3600 |
| A    | @                  | `185.199.110.153`  | 3600 |
| A    | @                  | `185.199.111.153`  | 3600 |

### Añade un CNAME para `www` que redirija al apex

| Tipo  | Nombre | Valor                                | TTL  |
|-------|--------|--------------------------------------|------|
| CNAME | www    | `TU-USUARIO.github.io.`              | 3600 |

(Sustituye `TU-USUARIO` por tu usuario de GitHub. El punto final tras `.io` no estorba; algunos paneles lo añaden solos.)

Guarda los cambios.

---

## 5. Esperar propagación y activar HTTPS

1. La propagación DNS suele tardar **entre 5 minutos y 1 hora** (a veces hasta 24 h, pero raro).
2. Comprueba con [dnschecker.org](https://dnschecker.org/#A/ricardoforcano.es) que las cuatro IPs aparecen ya en muchos servidores.
3. Vuelve a **Settings → Pages** del repo y refresca. Cuando GitHub detecte el DNS:
   - Verás un check verde con tu dominio.
   - Aparecerá la opción **Enforce HTTPS** — actívala (puede tardar otros 5-30 min en generar el certificado SSL Let's Encrypt).
4. Listo: `https://ricardoforcano.es` carga tu web con SSL.

---

## Cómo actualizar la web en el futuro

Cada vez que quieras cambiar algo:

- **Drag & drop**: edita el archivo localmente, ve al repo en GitHub, abre el archivo, clic en el ✏️ (Edit), pega los cambios y haz commit. O usa "Add file → Upload files" para sustituirlo.
- **Git**: edita en local, `git add . && git commit -m "..." && git push`.

GitHub Pages reconstruye y publica los cambios automáticamente en ~30 segundos.

---

## Notas

- **Archivos `.docx` de inventario**: están en la carpeta pero no son parte de la web. Puedes dejarlos fuera del repo o ignorarlos con un `.gitignore`.
- **PDFs grandes** (`viaje-desde-la-ciencia-a-la-consciencia.pdf` pesa 12.7 MB): GitHub permite archivos hasta 100 MB; sin problema.
- **Si más adelante mueves de proveedor** (Cloudflare Pages, Netlify…), solo tendrías que cambiar los registros DNS en Dondominio. El dominio sigue siendo tuyo.
