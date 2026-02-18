# 📝 Resumen de la Conversación: Aprendizaje de Git y Antigravity

**Usuario:** Tomas
**Asistente:** Antigravity (IA)
**Fecha:** 14 de febrero de 2026

---

## 🚀 1. Bienvenida e Introducción
Tomas comenzó su primer día con Antigravity para practicar programación y control de versiones (Git). Se estableció un plan de trabajo para aprender conceptos clave como branches, merges, push, pull y resolución de conflictos.

---

## 🛠️ 2. El IDE Antigravity
Aprendimos que Antigravity no es solo un editor, sino un asistente que puede:
- Ejecutar comandos en la terminal por ti.
- Crear y editar archivos automáticamente.
- Explicar conceptos complejos en lenguaje natural.
- Navegar por el proyecto y buscar información.

---

## 📂 3. Conceptos Fundamentales de Git

### Las 3 Áreas de Git:
1.  **Working Directory:** Tu carpeta local (ej: `vscode101`). Donde editas archivos.
2.  **Staging Area (Index):** El área de preparación. Se añaden cambios aquí con `git add`.
3.  **Repository:** El historial guardado permanentemente mediante `git commit`.

### La Memoria de Git:
- Git rastrea cambios, no solo archivos.
- Incluso si eliminas un archivo del disco, Git lo "recuerda" en su índice hasta que confirmas la eliminación.
- Mover o renombrar archivos (`git mv`) también es un cambio que Git registra.

---

## 🎓 4. Ejercicios Prácticos Completados

### Ejercicio 1: Conceptos Básicos
- **Comandos:** `git status`, `git add`, `git commit -m "mensaje"`.
- **Logro:** Tomas realizó su primer commit con un archivo llamado `mi primer archivo.txt`.

### Ejercicio 2: Ramas (Branches)
- **Comandos:** `git branch`, `git checkout -b nombre-rama`.
- **Logro:** Creación de la rama `feature-biografia` y entendimiento de cómo los archivos "aparecen y desaparecen" al cambiar de rama.

### Ejercicio 3: Fusión (Merge)
- **Comandos:** `git merge`, `git branch -d`.
- **Logro:** Fusión de la biografía a la rama `main` mediante un **Fast-Forward Merge**.

### Ejercicio 4: Trabajo Remoto
- **Comandos:** `git remote -v`, `git push origin main`, `git fetch`.
- **Concepto:** Diferencia entre `fetch` (descargar sin tocar mis archivos) y `pull` (descargar y fusionar automáticamente).
- **Logro:** Sincronización exitosa con el repositorio en GitHub.

### Ejercicio 5: Resolución de Conflictos
- **Escenario:** Modificación de la misma línea en `main` y en `rama-conflicto`.
- **Logro:** Tomas aprendió a identificar los marcadores de conflicto (`<<<<<<<`, `=======`, `>>>>>>>`) y a resolverlos manualmente para finalizar el merge.

---

## 📖 5. Glosario de Comandos Rápidos

| Comando | Acción |
| :--- | :--- |
| `git status` | Ver qué ha cambiado |
| `git add .` | Preparar todos los cambios |
| `git commit -m "..."` | Guardar cambios permanentemente |
| `git log --oneline (una linea por commit) --graph(de forma grafica) --all(todas las ramas)` | Ver el historial visualmente |
| `git checkout -b <nombre>` | Crear y saltar a una rama nueva |
| `git merge <nombre>` | Traer cambios de otra rama | (el merge se realiza trayendo los cambios de otra rama a la rama actual)

Repositorio remoto
| `git clone (URL)` | Descargar repositorio |
`git clone https://github.com/usuario/proyecto.git mi-carpeta`| Descargar repositorio en carpeta especifica|
| `git (CLI) push (Accion) origin (Ubicacion) main (Destino)` | Subir cambios a GitHub |
| `git fetch origin` | Ver qué hay de nuevo en el servidor |

---

## 🎯 Próximos Pasos
Tomas decidió continuar su aprendizaje en un nuevo repositorio dedicado para mantener sus proyectos organizados y separados.

---

*Documento generado por Antigravity para Tomas.*
