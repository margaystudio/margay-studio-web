# Evitar subir secretos a git (gitleaks)

Objetivo: que **nadie** suba una clave/token/contraseña al repo. Se logra con dos
capas, las dos **gratis**. La de CI es la que realmente garantiza; el pre-commit es
una red local opcional.

---

## Capa 1 — CI + branch protection (la garantía, no depende de nadie)

Corre en el servidor de GitHub, así que protege al equipo entero sin importar qué
tenga cada uno en su máquina. Ya está el workflow `.github/workflows/gitleaks.yml`
en el repo; falta exigir que pase antes de mergear.

**Setup (una vez por repo, lo hace un admin):**

1. GitHub → **Settings → Branches → Add branch ruleset** (o "Add rule").
2. Target: la rama principal (`main`; en `margay-dashboards` es `master`).
3. Activar **"Require a pull request before merging"**.
4. Activar **"Require status checks to pass"** y, en el buscador de checks,
   elegir **`gitleaks`**.
5. Guardar.

Desde ahí, ningún PR con un secreto detectado se puede mergear a la rama principal.

> Salvedad: el secreto igual "toca" GitHub por un momento en la rama del PR. Si el
> check salta, **rotá la clave igual** (regenerala en el proveedor) — borrarla del
> commit no alcanza, queda en la historia.

---

## Capa 2 — pre-commit (red local, opcional, la instala cada dev)

Frena el secreto **antes** de commitearlo, en la máquina de la persona. Es lo más
cómodo, pero es voluntario y se puede saltar (`--no-verify`), por eso la Capa 1 es
la que manda.

**Cada dev, una sola vez:**

```bash
brew install pre-commit          # macOS   (o: pipx install pre-commit)
cd <repo>
pre-commit install               # engancha el hook en este clon
```

Listo. **No hace falta instalar gitleaks a mano**: el archivo `.pre-commit-config.yaml`
del repo tiene fijada la versión (`rev: v8.21.2`) y `pre-commit` descarga y cachea
el binario solo en el primer commit. De ahí en más, cada `git commit` corre gitleaks
sobre lo staged y frena si encuentra algo.

---

## Si igual se te subió una clave (incidente)

1. **Rotala ya**: regenerala en el proveedor (Supabase, Gmail, OpenAI, etc.). Esto
   es lo único que la vuelve inofensiva — ya está en la historia de git.
2. Guardá la nueva en **Secret Manager** y referenciala desde `ops/*.config.toml`
   (`[secrets]`), nunca el valor en texto.
3. Escaneá la historia para confirmar qué quedó expuesto:
   ```bash
   brew install gitleaks
   gitleaks detect --source . --redact --config .gitleaks.toml
   ```

---

## Falsos positivos

El anon key de Supabase (`NEXT_PUBLIC_SUPABASE_ANON_KEY`) es un JWT **público** (viaja
al browser), no es un secreto. Está allowlisteado en `.gitleaks.toml` para no generar
ruido. Si aparece otro falso positivo legítimo, se agrega ahí.
