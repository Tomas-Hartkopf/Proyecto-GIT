# Aprendizaje Git - Conversación Copilot CLI

**Fecha:** 2026-02-17  
**Tema:** Fundamentos de Git, GitHub y Copilot CLI

---

## 1. Crear repositorio remoto desde terminal

**Pregunta:** ¿Cómo crear un repositorio remoto para hacer push?

**Opciones principales:**

### Con GitHub CLI
```powershell
gh repo create nombre-repo --source=. --remote=origin --push
# O manual:
gh repo create nombre-repo --source=.
git remote add origin https://github.com/usuario/nombre-repo.git
git branch -M main
git push -u origin main
```

### Configuración manual
```powershell
git remote add origin https://github.com/usuario/nombre-repo.git
git branch -M main
git push -u origin main
```

### Con servidor Git personalizado
```powershell
git remote add origin git@servidor.com:usuario/repo.git
git push -u origin main
```

### Verificar remoto actual
```powershell
git remote -v
```

---

## 2. Configurar nombre y email de commits

```powershell
# Configuración GLOBAL (para todos los repositorios)
git config --global user.name "Tu Nombre"
git config --global user.email "tu.email@ejemplo.com"

# Configuración LOCAL (solo para este repositorio)
git config user.name "Tu Nombre"
git config user.email "tu.email@ejemplo.com"
```

### Verificar configuración
```powershell
# Ver solo nombre y email
git config user.name
git config user.email

# Ver toda la configuración
git config --list
```

---

## 3. ¿Qué es una worktree (copilot-worktree)?

Una **worktree** es una característica de Git que permite trabajar en múltiples ramas **simultáneamente** en directorios diferentes, sin necesidad de hacer `git stash` o `git checkout`.

### Ventajas
- ✅ Trabaja en 2+ ramas al mismo tiempo
- ✅ No interfiere con rama principal
- ✅ Útil para tareas aisladas
- ✅ Evita hacer stash/checkout constante

### Comandos útiles
```powershell
# Ver todos los worktrees
git worktree list

# Ver en qué rama estás
git branch

# Cambiar de rama dentro de este worktree
git checkout otra-rama

# Eliminar el worktree cuando termines
git worktree remove ruta-worktree
```

---

## 4. Cómo usar worktrees - Ejemplo simple

```powershell
# 1. Estás en la rama 'main'
git branch
# Output: * main

# 2. Crear un worktree nuevo con una rama nueva
git worktree add ../nueva-feature feature/nueva-funcionalidad

# 3. Ahora tienes 2 directorios:
# ├── proyecto/           (rama 'main')
# └── nueva-feature/      (rama 'feature/nueva-funcionalidad')

# 4. Entrar al nuevo worktree
cd ../nueva-feature

# 5. Trabajar en este worktree (sin afectar 'main')
echo "código nuevo" > archivo.txt
git add .
git commit -m "Nueva funcionalidad"

# 6. Volver al proyecto original
cd ../proyecto

# 7. Cuando termines, eliminar el worktree
git worktree remove ../nueva-feature
```

### Comandos resumen
| Acción | Comando |
|--------|---------|
| Crear worktree | `git worktree add ruta rama` |
| Ver worktrees | `git worktree list` |
| Eliminar worktree | `git worktree remove ruta` |

---

## 5. Rama normal vs Worktree

| Aspecto | Branch | Worktree |
|--------|--------|----------|
| **Directorio** | Mismo directorio | Directorio separado |
| **Cambiar de rama** | `git checkout rama2` | Cambiar de carpeta |
| **Trabajar en 2 ramas** | ❌ No puedes (necesitas stash) | ✅ Sí, simultáneamente |
| **Espacio en disco** | 1 copia del código | 2+ copias del código |

### Cuándo usar cada uno
| Usa | Cuando |
|-----|--------|
| **Branch normal** | Solo trabajas en 1 rama a la vez |
| **Worktree** | Necesitas 2+ ramas abiertas simultáneamente |

---

## 6. Ubicación de worktrees

Los worktrees se almacenan **donde tú les digas**, pero típicamente en **carpetas hermanas** de tu proyecto principal.

### Ver todos los worktrees
```powershell
git worktree list
```

### Ejemplo de estructura
```
c:\Users\salud\Desktop\Proyecto GIT.worktrees\
├── copilot-worktree-2026-02-17T14-22-08/  (worktree)
└── proyecto original/                      (repositorio principal)
```

### Importante
- **Worktrees = Carpetas normales** en tu disco duro
- **Vinculadas** al repositorio original a través de `.git/worktrees/`
- **Comparten el mismo repositorio Git**, pero con rama propia
- **El directorio es tuyo** - los pones donde quieras

---

## 7. Crear nueva branch y hacer push

**Problema:** Si haces push desde main, NO crea la rama en GitHub.

### Proceso correcto
```powershell
# 1. Crear rama local
git checkout -b mi-nueva-rama

# 2. Hacer cambios y commits
echo "cambios" > archivo.txt
git add .
git commit -m "mi cambio"

# 3. Hacer PUSH de esa rama específica
git push -u origin mi-nueva-rama
# ↑ AQUÍ se crea en GitHub
```

### Lo que NO debes hacer
```powershell
# ❌ INCORRECTO: Esto solo empuja main
git checkout main
git push origin main
# La rama 'mi-nueva-rama' NO aparece en GitHub
```

### Verificar ramas en GitHub
```powershell
# Ver todas las ramas remotas
git branch -r

# Ver ramas locales
git branch

# Ver todas (local + remota)
git branch -a
```

---

## 8. Fast-forward merge

**Pregunta:** Si hago un fast-forward y luego el push desde main, ¿en el registro de commits es como si la rama nunca se hubiese creado?

**Respuesta:** ✅ Sí, es correcto.

### Ejemplo visual - Fast-forward
```
Rama feature con 2 commits:
main:     A ← B ← C
          ↑
feature:  A ← B ← C

Después de: git merge feature (fast-forward)
main:     A ← B ← C
          ↑
feature:  A ← B ← C

En git log se ve lineal:
commit C
commit B
commit A
# NO hay evidencia de que fue una rama separada
```

### Alternativa: Merge commit
```powershell
# Si usas: git merge --no-ff feature
# Crea un commit de merge visible:

commit M (merge commit)
       ↙ ↘
commit C   (de main)
commit B   (de feature)
commit A

# Aquí SÍ ves que hubo una rama bifurcada
```

### Comparación
| Tipo | Comando | Resultado en git log |
|------|---------|---------------------|
| Fast-forward | `git merge feature` | Commits lineales, sin rastro de rama |
| Merge commit | `git merge --no-ff feature` | Se ve la bifurcación claramente |

---

## 9. Git Fetch - Visualizar y acceder al repositorio remoto

### Después de hacer fetch, ¿qué tienes?
```powershell
# Descargar cambios del remoto
git fetch origin

# Ver todas las ramas (local + remota)
git branch -a
```

Output:
```
  main
  feature
  remotes/origin/main          ← Rama remota (después del fetch)
  remotes/origin/feature
  remotes/origin/develop
```

### Acceder a ramas remotas con checkout
```powershell
# ✅ Crear rama local basada en la remota
git checkout -b main origin/main

# ✅ Sintaxis corta (Git lo hace automáticamente)
git checkout main

# ✅ Ver cambios ANTES de hacer checkout
git log origin/main
git diff main origin/main
```

### Qué puedes hacer con fetch
| Acción | Comando | Uso |
|--------|---------|-----|
| Ver cambios remotos | `git log origin/main` | Revisar qué cambió |
| Comparar | `git diff main origin/main` | Ver diferencias |
| Crear rama local | `git checkout -b local origin/remota` | Trabajar en esa rama |
| Mergear remota | `git merge origin/main` | Actualizar tu main |
| Eliminar rama remota | `git push origin --delete rama` | Limpiar repositorio |

### Flujo recomendado
```powershell
# 1. Descargar cambios del remoto (SIN MODIFICAR tu código)
git fetch origin

# 2. Ver qué cambió
git log main..origin/main

# 3. Revisar cambios antes de aplicarlos
git diff main origin/main

# 4. Decidir si quieres mergear o hacer rebase
git merge origin/main        # Merge tradicional
# O
git rebase origin/main       # Rebase (historial lineal)

# 5. Hacer push de tu trabajo
git push origin main
```

### Ventaja: fetch vs pull
| Comando | Qué hace |
|---------|----------|
| `git fetch` | Descarga, NO modifica tu código |
| `git pull` | Descarga + mergea automáticamente |

**Fetch es más seguro:** Revisa primero, luego decides mergear.

---

## 10. Git Merge vs Git Rebase

Ambos unen ramas, pero de formas diferentes.

### Merge: Crea un commit de unión
```
Estado inicial:
main:      A ← B
           
feature:   A ← B ← C ← D

Después de: git merge feature
main:      A ← B ← M (commit de merge)
                ↙ ↘
feature:        C ← D

Resultado: Historial ramificado, se ve que hubo 2 caminos.
```

### Rebase: Reaplica commits linealmente
```
Estado inicial (mismo que arriba):
main:      A ← B
           
feature:   A ← B ← C ← D

Después de: git rebase main (desde feature)
main:      A ← B

feature:   A ← B ← C' ← D'
           (commits replicados sobre main)

Si luego haces: git merge feature (fast-forward)
main:      A ← B ← C' ← D'

Resultado: Historial LINEAL, más limpio.
```

### Diferencias clave
| Aspecto | Merge | Rebase |
|---------|-------|--------|
| **Historial** | Ramificado, ve bifurcaciones | Lineal, historial recto |
| **Commits** | Crea commit de merge | Reaplica commits existentes |
| **Reversible** | Fácil: `git revert` | Difícil (cambia historial) |
| **Ramas compartidas** | ✅ Seguro | ❌ Nunca en ramas públicas |

### Cuándo usar cada uno
```powershell
# MERGE: Ramas compartidas o remotas
git fetch origin
git merge origin/main
# ✅ Seguro, otros ven el cambio

# REBASE: Solo en ramas locales propias
git rebase main
# ✅ Limpio, pero NO hagas en ramas que otros usan
```

### ⚠️ Regla de oro
```
NUNCA hagas rebase en una rama que otros usan
Usa rebase SOLO en ramas locales propias
```

Porque rebase **reescribe el historial**, y otros quedarían con commits duplicados.

---

## 11. ¿Qué es un Fork?

Un **fork** es una **copia completa de un repositorio remoto a tu cuenta**.

Se usa principalmente en **proyectos open source** para contribuir sin permisos.

### Diferencia: Fork vs Clone
| Acción | Qué hace | Para qué |
|--------|----------|---------|
| **Fork** | Copia el repo a tu GitHub | Contribuir a proyectos de otros |
| **Clone** | Descarga el repo a tu PC | Trabajar localmente |

```powershell
# Fork: Lo haces en la web de GitHub (botón Fork)
# Resultado: https://github.com/TU-USUARIO/proyecto

# Clone: Lo haces desde terminal
git clone https://github.com/tu-usuario/proyecto
```

### Flujo típico de contribución
```powershell
# 1. Fork el repositorio (en web GitHub)
# → Se crea: tu-usuario/proyecto

# 2. Clonar tu fork localmente
git clone https://github.com/tu-usuario/proyecto
cd proyecto

# 3. Crear rama con tu cambio
git checkout -b feature/mi-cambio

# 4. Hacer cambios y commits
echo "mi aporte" > archivo.txt
git add .
git commit -m "Mi contribución"

# 5. Push a TU fork
git push origin feature/mi-cambio

# 6. Pull Request (en GitHub)
# Haces click en "New Pull Request"
# → Propones que fusionen tu cambio al proyecto original
```

### Mantener tu fork actualizado
```powershell
# Agregar repositorio original como "upstream"
git remote add upstream https://github.com/proyecto-original/proyecto

# Traer cambios del original
git fetch upstream
git merge upstream/main

# Empujar a tu fork
git push origin main
```

### Resumen
- ✅ **Fork** = Copia en TU GitHub
- ✅ **Clone** = Descarga a TU PC
- ✅ **Pull Request** = Propuesta para fusionar cambios
- ✅ Se usa para **open source** y contribuir

---

## 12. Sesiones de VS Code con GitHub Copilot

**Pregunta:** ¿Las sesiones de background permiten ver la conversación en otro dispositivo?

**Respuesta:** No automáticamente.

### Realidad
- Cada instalación de VS Code tiene su **historial de chat local separado**
- **No se sincroniza en la nube**, incluso con la misma cuenta de GitHub
- La diferencia background/foreground NO afecta la accesibilidad desde otro dispositivo

### Diferencia: Background vs Foreground
- **Background/Foreground** = Si el proceso corre visible en terminal o no
- **NO afecta** la sincronización entre dispositivos

### Soluciones para acceder desde casa

**Opción 1: Exportar la conversación** (Manual)
- Selecciona toda la conversación
- Copiar (Ctrl+A, Ctrl+C)
- Pegar en un archivo .txt o .md
- Sincronizar ese archivo (Google Drive, OneDrive, GitHub)
- Abrir desde casa

**Opción 2: Usar Settings Sync** (Parcial)
- Ctrl+Shift+P → "Settings: Open Settings Sync"
- Sincroniza extensiones y configuración
- Pero NO el historial de chat

**Opción 3: Usar un servicio en la nube** (Recomendado)
- Guardar conversaciones en Google Keep, Notion, OneNote
- GitHub (en un repositorio privado)
- Google Drive

**Opción 4: Usar Claude.ai directamente** (Alternativa)
- Ir a claude.ai con tu cuenta
- Todas las conversaciones sincronizadas en la nube
- Accesibles desde cualquier dispositivo

---

## Resumen de conceptos clave

### Git Workflow recomendado
```powershell
# 1. Clonar repositorio
git clone https://github.com/usuario/repo.git

# 2. Crear rama
git checkout -b feature/nueva-funcionalidad

# 3. Hacer cambios
# ... editar archivos ...

# 4. Commit
git add .
git commit -m "Descripción clara"

# 5. Fetch para verificar cambios remotos
git fetch origin

# 6. Merge o Rebase (según el caso)
git merge origin/main  # Para ramas públicas
# O
git rebase main        # Solo para ramas locales

# 7. Push
git push -u origin feature/nueva-funcionalidad

# 8. Pull Request en GitHub
# Proponer cambio para fusionar con main
```

### Comandos más usados
```powershell
git config --global user.name "Tu Nombre"     # Configurar usuario
git clone <url>                               # Clonar repositorio
git checkout -b rama                          # Crear nueva rama
git add .                                     # Preparar cambios
git commit -m "mensaje"                       # Confirmar cambios
git push origin rama                          # Enviar a GitHub
git fetch origin                              # Descargar cambios
git merge origin/main                         # Fusionar rama
git rebase main                               # Reordenar historial
git worktree add ../ruta rama                 # Crear worktree
git branch -a                                 # Ver todas las ramas
git log                                       # Ver historial
```

---

**Última actualización:** 2026-02-17T16:15:36Z
