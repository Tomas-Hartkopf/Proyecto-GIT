# 🎮 Proyecto de Práctica: Sistema de Tareas

Este es un proyecto simple para practicar Git con escenarios realistas.

## 📝 Descripción

Un sistema básico de lista de tareas donde practicarás:
- Crear ramas para nuevas características
- Hacer commits organizados
- Fusionar cambios
- Resolver conflictos
- Trabajar con remotos

## 🚀 Características a Implementar

Cada característica debe hacerse en su propia rama:

### 1. Feature: Agregar Tareas
- **Rama**: `feature-agregar-tareas`
- **Descripción**: Crear un archivo donde se puedan agregar tareas
- **Archivos**: `tareas.txt`

### 2. Feature: Categorías
- **Rama**: `feature-categorias`
- **Descripción**: Organizar tareas por categorías (Personal, Trabajo, Estudio)
- **Archivos**: `tareas_personal.txt`, `tareas_trabajo.txt`, `tareas_estudio.txt`

### 3. Feature: Prioridades
- **Rama**: `feature-prioridades`
- **Descripción**: Marcar tareas con prioridad (Alta, Media, Baja)
- **Archivos**: Modificar archivos existentes

### 4. Feature: Fechas
- **Rama**: `feature-fechas`
- **Descripción**: Agregar fechas límite a las tareas
- **Archivos**: Modificar archivos existentes

## 📋 Flujo de Trabajo Sugerido

Para cada feature:

1. **Crear rama**
   ```bash
   git checkout -b feature-nombre
   ```

2. **Hacer cambios**
   - Edita/crea archivos
   - Prueba que funcione

3. **Commit**
   ```bash
   git add .
   git commit -m "Descripción clara del cambio"
   ```

4. **Merge a main**
   ```bash
   git checkout main
   git merge feature-nombre
   ```

5. **Push al remoto**
   ```bash
   git push origin main
   ```

6. **Limpiar rama**
   ```bash
   git branch -d feature-nombre
   ```

## 🎯 Desafíos Adicionales

1. **Conflicto Intencional**: Crea dos ramas que modifiquen la misma línea
2. **Revert**: Deshaz un commit que "rompió" algo
3. **Stash**: Guarda cambios temporalmente sin hacer commit
4. **Tags**: Marca versiones importantes (v1.0, v2.0)

## 💡 Consejos

- Haz commits pequeños y frecuentes
- Escribe mensajes de commit descriptivos
- Prueba antes de hacer merge a main
- Usa `git status` constantemente

---

**¡Diviértete practicando!** 🎉
