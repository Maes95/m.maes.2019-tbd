# Práctica 1 - Solución 
### Repositorios y modelos de desarrollo

A continuación se detalla como se ha resuelto la práctica utilizando Trunk-Based Development

### Paso 1: Creación y clonado del repositorio en GitHub

Desde la interfaz web https://github.com/ creamos un nuevo repositorio llamado `m.maes.2019-tbd`

Una vez creado, lo clonamos en nuestro local:
```
$ git clone https://github.com/Maes95/m.maes.2019-tbd.git
$ cd m.maes.2019-tbd
```

### Paso 2: Desarrollo de la Feature 1

Creamos una nueva rama:

$ git checkout -b feature-1

Añadidmos 3 nuevos commits, cada uno creando los ficheros A, B y C respectivamente.

```
$ echo "'A' file content" > a.txt
$ git add a.txt
$ git commit -m "F1 - Add file A"
```

```
$ echo "'B' file content" > b.txt
$ git add b.txt
$ git commit -m "F1 - Add file B"
```

```
$ echo "'C' file content" > c.txt
$ git add c.txt
$ git commit -m "F1 - Add file C"
```

Subimos estos cambios al repositorio remoto:

```
$ git push origin feature-1
```

Creamos master a partir de feature-1 y subimos los cambios
```
$ git fetch origin
$ git checkout -b master
$ git push origin master
```

A continuación publicamos la Release 1.0 y añadimos una nueva tag:

```
$ git checkout -b release-1.0
$ git tag "v1.0.0"
$ git push origin release-1.0
$ git push --tags
```

### Paso 3: Desarrollo de la Feature 2

Creamos una nueva rama:

```
$ git checkout master
$ git checkout -b feature-2
```

Añadidmos 2 nuevos commits, el primero añadiendo el fichero D y el segundo modificando A:

```
$ echo "'D' file content" > d.txt
$ git add d.txt
$ git commit -m "F2 - Add file D"
```

```
$ echo -e "F2-CHANGE $(cat a.txt)" > a.txt 
$ git add a.txt 
$ git commit -m "F2 - Change first line of A"
```

Subimos estos cambios al repositorio remoto:

```
$ git push origin feature-2
```

### Paso 4: Desarrollo de la Feature 3

Creamos una nueva rama:

```
$ git checkout master
$ git checkout -b feature-3
```

Añadidmos 2 nuevos commits, el primero añadiendo el fichero E y el segundo modificando A:

```
$ echo "'E' file content" > e.txt
$ git add e.txt
$ git commit -m "F3 - Add file E"
```

```
$ echo -e "F3-CHANGE $(cat a.txt)" > a.txt 
$ git add a.txt 
$ git commit -m "F3 - Change first line of A"
```

Subimos estos cambios al repositorio remoto:
```
$ git push origin feature-3
```

### Paso 5: Mergeamos los cambios de feature-2 con master y publicamos release

Comprobamos que master se puede mergear con la rama feature-2
```
$ git fetch origin
$ git checkout feature-2
$ git merge master
```

Realizamos el merge y subimos los cambios
```
$ git checkout master
$ git merge --no-ff feature-2
$ git push origin master
```

A continuación publicamos la Release 1.1 y añadimos una nueva tag:

```
$ git checkout -b release-1.1
$ git tag "v1.1.0"
$ git push origin release-1.1
$ git push --tags
```

### Paso 6: Corregimos el bug

Cambiamos a master e introducimos el hotfix
```
$ git checkout master
$ echo "AWESOME FIX" > f.txt 
$ git add f.txt 
$ git commit -m "BUGFIX - Hotfix to solve a bug in production"
$ git push origin master
```

Aplicamos el hotfix a la rama release-1.1 usando cherry-pick (Se añade el tag v1.1.1)

```
$ git checkout master
$ COMMIT=$(git log -1 --pretty=format:"%h")
$ git checkout release-1.1 
$ git cherry-pick $COMMIT
$ git tag "v1.1.1"
$ git push origin release-1.1 
$ git push --tags
```

### Paso 7: Mergeamos los cambios de feature-3 con master y publicamos release

Comprobamos que master se puede mergear con la rama feature-3
```
$ git fetch origin
$ git checkout feature-3
$ git merge master
```

En este punto, el merge fallara, deberemos solucionar el conflicto a mano.

Una vez solucionado, creamos a mano el commit de merge:

```
$ git add a.txt
$ git commit -am "Solve conflict between master and F3"
$ git merge master
```

```
$ git checkout master
$ git merge --no-ff feature-3
$ git push origin master
```

A continuación publicamos la Release 1.2 y añadimos una nueva tag:

```
$ git checkout -b release-1.2
$ git tag "v1.2.0"
$ git push origin release-1.2
$ git push --tags
```
