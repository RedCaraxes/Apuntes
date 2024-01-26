---
tags:
  - git
  - github
aliases:
  - Comandos basicos de Git y GitHub

---

# Comandos básicos
![[Pasted image 20231031221413.png]]
>[!info] Configurar git 
>Primero poner el correo y nombre con los siguientes comandos
>```Git
>git config --global user.name "nombre"
>```
>```Git
>git config --global user.email "email"
>```
>Para verificar dicha configuracion
>```Git
>git config --list
>```
![[Pasted image 20231229170117.png]]

>[!info] git init
>git init --initial-branch=main  -> "inician la rama como main, por defecto es master para versiones antiguas" 
>git init -b main ""
>
>```Git
>git init
>```

>>[!info] git init --bare
Los repositorios bare se utilizan típicamente para almacenar copias de un repositorio de Git en un servidor remoto

>```Git
>git init --bare
>```

>[!info] git add
El comando `git add` prepara los cambios en el directorio de trabajo para el próximo commit. Cuando ejecuta `git add` sobre un archivo o directorio, Git lo agrega al área de preparación (staging area).
>```Git
>git add
>```

>[!info] git log
El comando `git log` muestra los commits realizados. Si se quiere mas detalle añadir el flag  `-p` con el numero de commits para visualizar en el siguiente formato, una alternativa mas resumida el flag  `--stat`
>```Git
>git log -p -"numero de commits"
>git log --stat
>```
>Alternativa mas resumida en una solo línea, el comando completo se muestra a continuación
>```Git
>git log --pretty=oneline
>$ git log --pretty=format:"%h - %an, %ar : %s"
>```
donde:

| Specifier | Description of Output                           |
| --------- | ----------------------------------------------- |
| %H        | Commit hash                                     |
| %h        | Abbreviated commit hash                         |
| %T        | Tree hash                                       |
| %t        | Abbreviated tree hash                           |
| %P        | Parent hashes                                   |
| %p        | Abbreviated parent hashes                       |
| %an       | Author name                                     |
| %ae       | Author email                                    |
| %ad       | Author date (format respects the --date=option) |
| %ar       | Author date, relative                           |
| %cn       | Committer name                                  |
| %ce       | Committer email                                 |
| %cd       | Committer date                                  |
| %cr       | Committer date, relative                        |
| %s        | Subject                                         |
Mas opciones:

| Option          | description                                                                      |
| --------------- | -------------------------------------------------------------------------------- |
| -p              | Show the patch introduced with each commit.                                      |
| --stat          | Show statistics for files modified in each commit.                               |
| --shortstat     | Display only the changed/insertions/deletions line from the --stat command.      |
| --name-only     | Show the list of files modified after the commit information.                    |
| --name-status   | Show the list of files affected with added/modified/deleted information as well. |
| --abbrev-commit | Show only the first few characters of the SHA-1 checksum instead of all 40.      |
| --relative-date | Display the date in a relative format (for example, “2 weeks ago”) instead of using the full date format.|
|--graph |Display an ASCII graph of the branch and merge history beside the log output.|
|--pretty |Show commits in an alternate format. Option values include oneline, short, full, fuller, and format (where you specify your own format).|
|--oneline |Shorthand for --pretty=oneline --abbrev-commit used together.|
Limitar búsqueda

| Option            | Description                                                                  |
| ----------------- | ---------------------------------------------------------------------------- |
| -<n>              | Show only the last n commits.                                                |
| --since, --after  | Limit the commits to those made after the specified date.                    |
| --until, --before | Limit the commits to those made before the specified date.                   |
| --author          | Only show commits in which the author entry matches the specified string.    |
| --committer       | Only show commits in which the committer entry matches the specified string. |
| --grep            | Only show commits with a commit message containing the string.               |
| -S                | Only show commits adding or removing code matching the string.               |

>[!info] git commit
El comando `git commit` confirma los cambios en el área de preparación al historial de commits. Cuando ejecuta `git commit`, Git crea un nuevo commit que contiene un hash único, un mensaje de commit y un árbol de objetos. La opción `-a` hace la función de `git add`, `--amend --no-edit` sobrescribe el commit ultimo y guarda los cambios en si mismo, se usa para pequeños cambios y para no crear mas commits.
>```Git
>git commit -a -m "text"
>git commit --amend --no-edit "Creata a styles"
>```

>[!warning] git branch "rama" & git checkout "rama" & git checkout -- "archivo"
>`git branch` crea una nueva rama, mientras que `git checkout` cambia a una rama, en conjunto pueden hacer los mismo que `git checkout -b "rama"`. Por otro lado `git checkout -- "archivo` puede traer devuelve un archivo eliminado (sin add ni commit posteriores al borrado). Tambien `git checkout <hash> .` trae los cambios de otro commit al directorio actual (es decir deshace los cambios y copia otro commit en el mismo commit)
>
>```Git
>git symbolic-ref HEAD refs/heads/main
>```
>
>```Git
>git branch "rama"
>git checkout "rama"
>```
>```Git
>git checkout -b "rama"
>```
>
>```Git
>git checkout -- "archivo"
>```
>```Git
>git checkout  "hash" .
>```

>[!warning] Recuperación de archivos
>Cuando se erra al untrackear un archivo y quiere recuperarse, primero se reestablece el ultimo commit y el archivo se trae de vuelta.
>```Git
>git rm index.html
>git reset HEAD "archivo"
>git checkout -- "archivo"
>```
>Despues de hacer un commit y haber borrado un archivo antes, esto retrocede al anterior commit y trae el archivo.
>```Git
>git reset --hard HEAD^
>git checkout -- "archivo"
>```

>[!info] git merge --ff-only "rama"
>El comando `git merge --ff-only "rama"` intenta fusionar la rama `rama` con la rama actual. Si la fusión (al commit mas reciente, no se crea otro commit) se puede realizar como una fusión rápida (fast-forward, solo si no hay divergencias entre ramas), el comando se realizará con éxito. De lo contrario, el comando fallará.
>```Git
>git merge --ff-only "rama"
>```


>[!info] Diferencias entre git merge "rama" --no-edit y git merge --ff-only "rama"

>|Característica | git merge "rama" --no-edit | git merge --ff-only "rama"|
>|---|---|---|
>Crea un nuevo commit de fusión|Sí|Solo si no se puede realizar una fusión rápida|
>|Mantiene un historial de commits lineal y limpio|No|Sí|
>|Falla si las ramas no están en un estado compatible para la fusión|Sí|Sí|
>|Falla si la fusión no se puede realizar sin conflictos|Sí|Sí|
>la opcion --no-edit le indica a Git que no abra el editor para que edite el mensaje de fusión.

>[!info] Revertir cambios a otro Commit 
>Para revertir todos los cambios realizados desde un punto determinado, puede usar el comando `git revert --no-edit <hash>`. El hash es el identificador único del commit desde el que desea revertir los cambios. La opcion --no-edit es para no cambiar el mensaje del commit, al crear un nuevo commit saldra con "revert".
>```Git
>git revert --no-edit "hash"
>```

>[!info] Git fetch
>Si clonas un repositorio, el comando de clonar automáticamente añade ese repositorio remoto con el nombre “origin”. Por lo tanto, `git fetch origin` se trae todo el trabajo nuevo que ha sido enviado a ese servidor desde que lo clonaste (o desde la última vez que trajiste datos). Es importante destacar que el comando `git fetch` solo trae datos a tu repositorio local - ni lo combina automáticamente con tu trabajo ni modifica el trabajo que llevas hecho. La combinación con tu trabajo debes hacerla manualmente cuando estés listo.
>```Git
>git fetch [remote-name]
>```

>[!info] Git Push
>Cuando tienes un proyecto que quieres compartir, debes enviarlo a un servidor. El comando para hacerlo es simple: `git push [nombre-remoto] [nombre-rama]`. Si quieres enviar tu rama `master` a tu servidor `origin` (recuerda, clonar un repositorio establece esos nombres automáticamente), entonces puedes ejecutar el siguiente comando y se enviarán todos los _commits_ que hayas hecho al servidor:
>```Git
>git push [nombre-remoto] [nombre-rama]
>```
>Si quieres ver más información acerca de un remoto en particular, puedes ejecutar el comando `git remote show [nombre-remoto]`.
>Si quieres cambiar el nombre de la referencia de un remoto puedes ejecutar `git remote rename`. Por ejemplo, si quieres cambiar el nombre de `pb` a `paul`, puedes hacerlo con `git remote rename`:


```Git
git config --global push.default simple
```

```Git
git push
```

>[!info] Git Pull
```Git
git pull [remote][rama]
```

```Git
git stash
```

# Combinar Ramas

>[!info] Git Merge
>El primer método para combinarlas que vamos a explorar es `git merge`. Hacer merge en Git crea un commit especial que tiene dos padres diferentes. Un commit con dos padres esencialmente significa "Quiero incluir todo el trabajo de estos dos padres , _y_ del conjunto de todos sus ancestros"

>[!info] Git Rebase
>El segundo modo de combinar el trabajo de distintas ramas es el _rebase_. Hacer rebase escencialmente selecciona un conjunto de commits, los "copia", y los aplica en algún otro lado.Aunque esto pueda sonar confuso, la ventaja de hacer rebase es que puede usarse para conseguir una secuencia de commits lineal, más bonita. El historial / log de commits del repositorio va a estar mucho más claro si sólo usas rebase.