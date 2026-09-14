# Actividad 1. Desafío Git: trazabilidad y evolución de una API

**Estudiante:** SERGIO ALONSO ARBOLEDA SÁNCHEZ

**Repositorio de trabajo:** API representativa de gestión de estudiantes, desarrollada como ejercicio de trazabilidad y evolución mediante Git.

**Repositorio GitHub:** `https://github.com/sergiosr17-web/PROGRAMACI-N-V-`

---

## Desafío 1. Construcción del historial de ramas

### Comandos ejecutados

El repositorio se inicializó localmente y se construyó un historial de commits mediante ramas de funcionalidades.

```bash
git init

git add -A

git commit -m "Estructura inicial de la API de gestion academica"
# C1 en main

git branch feature/telefono-estudiante

git branch feature/consulta-estudiantes

git switch feature/telefono-estudiante

git add -A

git commit -m "Agrega campo telefono al estudiante"
# C2

git switch -c feature/validar-telefono
# C4, creada desde feature/telefono-estudiante

git add -A

git commit -m "Agrega validacion de formato para telefono del estudiante"

git switch feature/consulta-estudiantes
# C3, creada originalmente desde main

git add -A

git commit -m "Agrega endpoint de consulta de estudiante por id"

# Conexión con el repositorio remoto

git remote add origin https://github.com/sergiosr17-web/PROGRAMACI-N-V-.git

# Sincronización de las ramas con GitHub

git push -u origin main
git push -u origin feature/telefono-estudiante
git push -u origin feature/consulta-estudiantes
git push -u origin feature/validar-telefono
```

> **Nota:** en el repositorio de trabajo se utiliza la cuenta `sergiosr17-web`. Los hashes de los commits son generados automáticamente por Git y, por tanto, son diferentes de los utilizados en otros ejemplos.

---

## Estructura esperada del historial

El historial debe representar la siguiente evolución:

```text
                         C4
                         │
                         │ feature/validar-telefono
                         │
                    C2 ──┘
                   /
C1 ───────────────
                   \
                    C3
                     │
                     └── feature/consulta-estudiantes
```

De forma conceptual:

```text
C1  Estructura inicial
│
├── C2  Agrega campo telefono
│    │
│    └── C4  Valida formato del telefono
│
└── C3  Consulta estudiante por ID
```

Para visualizar el árbol real del repositorio se utilizó:

```bash
git log --all --graph --oneline --decorate
```

### Resultado

**Aquí debe pegarse el resultado REAL obtenido en el repositorio de Sergio:**

```text
PEGAR AQUÍ LA SALIDA DE:

git log --all --graph --oneline --decorate
```

---

## Hash corto de cada commit y rama

Los hashes deben obtenerse directamente del repositorio mediante:

```bash
git log --all --oneline --decorate
```

La tabla final debe quedar así:

| Commit | Hash corto    | Rama                           |
| ------ | ------------- | ------------------------------ |
| C1     | `[HASH REAL]` | `main`                         |
| C2     | `[HASH REAL]` | `feature/telefono-estudiante`  |
| C3     | `[HASH REAL]` | `feature/consulta-estudiantes` |
| C4     | `[HASH REAL]` | `feature/validar-telefono`     |

**No se deben copiar los hashes utilizados en ejemplos de otros estudiantes.**

---

## ¿Por qué `feature/validar-telefono` se origina desde `feature/telefono-estudiante` y no directamente desde `main`?

La rama `feature/validar-telefono` se crea a partir de `feature/telefono-estudiante` porque la validación depende de que previamente exista el campo `Telefono` en el modelo `Estudiante`.

El campo `Telefono` se incorpora en el commit C2, realizado en `feature/telefono-estudiante`. Por esta razón, crear la rama de validación desde C2 permite continuar directamente sobre ese desarrollo y mantener una relación lógica entre ambas funcionalidades.

Si la rama de validación se hubiera creado directamente desde `main`, el campo `Telefono` todavía no estaría incluido en esa línea de desarrollo, por lo que sería necesario incorporar primero los cambios de la funcionalidad de teléfono.

---

# Desafío 2. Investigación del historial

## 1-4. Commit donde se agregó el teléfono

Para localizar el commit correspondiente se utilizó:

```bash
git log --all --oneline --grep="telefono" -i
```

Posteriormente se consultó el detalle del commit mediante:

```bash
git show [HASH-C2] --stat
```

### Resultado

```text
PEGAR AQUÍ LA SALIDA REAL DE:

git show [HASH-C2] --stat
```

### Respuesta

El commit donde se agregó el campo `Telefono` corresponde al commit **C2**, perteneciente a la rama:

```text
feature/telefono-estudiante
```

El mensaje del commit es:

```text
Agrega campo telefono al estudiante
```

El autor, correo y fecha deben corresponder a los datos registrados realmente por Git en el repositorio.

Para consultarlos se utilizó:

```bash
git show [HASH-C2]
```

---

## 5. ¿Qué archivos fueron modificados en ese commit?

Se utilizó:

```bash
git show --name-only [HASH-C2]
```

### Resultado

```text
PEGAR AQUÍ LA SALIDA REAL DEL COMANDO
```

### Respuesta

Los archivos modificados corresponden a los archivos que aparecen en el resultado de `git show --name-only`.

En particular, debe verificarse si el proyecto utiliza archivos como:

```text
Models/Estudiante.cs
Controllers/EstudiantesController.cs
```

o una estructura diferente.

**La respuesta final debe utilizar exactamente los nombres que tenga el repositorio de Sergio.**

---

## 6. ¿Cuál fue el commit donde se implementó la validación del teléfono?

Se utilizó:

```bash
git log --oneline --all --grep="validacion" -i
```

El resultado esperado debe identificar el commit cuyo mensaje sea:

```text
Agrega validacion de formato para telefono del estudiante
```

Ese commit corresponde a **C4**, ubicado en:

```text
feature/validar-telefono
```

El hash corto debe obtenerse del resultado real de Git.

---

# Desafío 3. Recuperación de cambios

## Contexto

El objetivo de este desafío es demostrar la recuperación de una modificación realizada en el directorio de trabajo antes de convertirla en un commit.

Primero se cambió a:

```bash
git switch feature/telefono-estudiante
```

Posteriormente se realizó una modificación manual en `Program.cs`.

Se verificó el estado:

```bash
git status
```

Luego se revisaron las diferencias:

```bash
git diff
```

Finalmente se descartó la modificación:

```bash
git restore Program.cs
```

Y se verificó nuevamente:

```bash
git status
```

### Resultado esperado

```text
On branch feature/telefono-estudiante
nothing to commit, working tree clean
```

### Explicación

`git restore` permite recuperar la versión registrada de un archivo cuando existen modificaciones en el directorio de trabajo que todavía no han sido confirmadas mediante un commit.

Por esta razón, al ejecutar:

```bash
git restore Program.cs
```

la modificación realizada en `Program.cs` fue descartada y el archivo volvió al estado correspondiente al último commit de la rama.

### ¿Qué habría ocurrido si el cambio ya hubiese sido incluido en un commit?

Si la modificación ya estuviera incluida en un commit, `git restore` no sería la herramienta adecuada para eliminar ese commit del historial.

En ese caso se podría utilizar:

```bash
git revert [HASH]
```

para crear un nuevo commit que revierta los cambios.

También existe:

```bash
git reset
```

que puede mover el puntero de la rama hacia otro commit, pero esta operación puede reescribir el historial. Por esta razón, cuando un commit ya fue publicado y compartido, `git revert` suele ser una opción más segura.

---

# Desafío 4. Deshacer un commit

## Rama desde la cual se creó `feature/cambio-prueba`

La rama se creó desde:

```text
main
```

Comandos:

```bash
git switch main

git switch -c feature/cambio-prueba
```

Posteriormente se realizó una modificación sencilla en `Program.cs`.

Se agregó el cambio al área de preparación:

```bash
git add -A
```

Y se creó un commit:

```bash
git commit -m "Agrega nota de version pendiente"
```

Para consultar el historial:

```bash
git log --oneline -3
```

---

## Deshacer el commit

Se utilizó:

```bash
git reset --soft HEAD~1
```

Posteriormente:

```bash
git status
```

### ¿Qué ocurrió?

El comando:

```bash
git reset --soft HEAD~1
```

movió la referencia de la rama al commit anterior, pero conservó los cambios del commit deshecho en el área de staging.

Por esta razón, después del `reset --soft`, el archivo modificado continúa apareciendo como:

```text
Changes to be committed
```

El commit original deja de estar referenciado por la rama, aunque puede permanecer temporalmente accesible mediante el reflog.

---

## Corrección y nuevo commit

Después de corregir la modificación se ejecutó:

```bash
git add -A
```

y posteriormente:

```bash
git commit -m "Agrega version de la API en el arranque"
```

El nuevo commit recibe un hash diferente al commit original porque Git identifica cada commit mediante su contenido, metadatos y relación con sus padres.

Para consultar el nuevo hash:

```bash
git log --oneline -3
```

---

## ¿Qué diferencia existiría si el commit ya hubiera sido publicado y compartido en GitHub?

Si el commit ya hubiera sido publicado y otros colaboradores hubieran trabajado sobre él, utilizar `git reset --soft` modificaría el historial local de la rama.

Al intentar sincronizar posteriormente con GitHub podrían aparecer diferencias entre el historial local y el remoto.

En un escenario colaborativo se recomienda utilizar:

```bash
git revert [HASH]
```

porque `git revert` conserva el commit original y genera otro commit que deshace sus modificaciones.

En cambio, `git reset` mueve la referencia de la rama y puede reescribir el historial que ya fue compartido.

---

# Nota final de verificación

El repositorio utilizado para la actividad corresponde a:

```text
https://github.com/sergiosr17-web/PROGRAMACI-N-V-
```

La estructura de ramas requerida es:

```text
main
feature/telefono-estudiante
feature/consulta-estudiantes
feature/validar-telefono
feature/cambio-prueba
```

La verificación final se realiza mediante:

```bash
git status
```

```bash
git branch -a
```

```bash
git log --all --graph --oneline --decorate
```

Además, se debe comprobar que la API compile y ejecute correctamente:

```bash
dotnet build
```

y, si el proyecto es ejecutable como API:

```bash
dotnet run
```

La comprobación funcional debe realizarse sobre las funcionalidades implementadas en las respectivas ramas, verificando especialmente la existencia del campo `Telefono`, su validación y el endpoint de consulta de estudiantes.

---

## Comandos principales utilizados

```bash
git init
git add -A
git commit
git branch
git switch
git switch -c
git log
git show
git diff
git restore
git reset --soft
git remote
git fetch
git push
git status
git branch -a
git log --all --graph --oneline --decorate
dotnet build
dotnet run
```
