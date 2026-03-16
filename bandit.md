#OverTheWire: Bandit — Writeup

## Level 0
Conexión inicial por SSH:
```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```
---
## Level 0 → 1
Listar archivos y leer el readme:
```bash
ls
cat readme
```
Password: `ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If`
---
## Level 1 → 2
El archivo se llama `-`, por lo que hay que especificar la ruta completa para evitar que el shell lo interprete como un flag:
```bash
cat /home/bandit1/-
```
Password: `263JGJPfgU6LtdEvgfWU1XP5yac29mFx`
---
## Level 2 → 3
El archivo tiene espacios en el nombre. Se usan comillas para manejarlo correctamente:
```bash
cat "./-spaces in this filename--"
```
Password: `MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx`
---
## Level 3 → 4
El archivo está oculto dentro del directorio `inhere`. Se usa `ls -la` para ver archivos ocultos:
```bash
cd inhere
ls -la
cat "...Hiding-From-You"
```
Password: `2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ`
---
## Level 4 → 5
Se identifican los tipos de archivo dentro de `inhere` para encontrar el único archivo de texto legible:
```bash
cd inhere
file ./*
cat ./-file07
```
El comando `file` indica que `-file07` es el único archivo de texto ASCII.
Password: `4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw`
---
## Level 5 → 6
Se busca el archivo con las propiedades indicadas: tipo regular y tamaño exacto de 1033 bytes:
```bash
cd inhere
find . -type f -size 1033c
```
Password: `HWasnPhtq9AVKe0dmk45nxy20cvUa6EG`
---
## Level 6 → 7
La contraseña está en algún lugar del servidor (no solo en el directorio actual). Se busca por propietario, grupo y tamaño:
```bash
find / -type f -size 33c -user bandit7 -group bandit6 2>/dev/null
```
Archivo encontrado: `/var/lib/dpkg/info/bandit7.password`
```bash
cat /var/lib/dpkg/info/bandit7.password
```
Password: `morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj`
---
## Level 7 → 8
La contraseña está en `data.txt` junto a la palabra `millionth`. Se filtra con `grep`:
```bash
cat data.txt | grep "millionth"
```
> ⚠️ `cat data.txt grep "millionth"` no funciona — hay que usar el pipe `|`.
Password: `dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc`
---
## Level 8 → 9
La contraseña es la única línea que aparece una sola vez en `data.txt`. Se ordena y luego se filtran duplicados:
```bash
sort data.txt | uniq -u
```
Password: `4CKMh1JI91bUIZZPXDqGanal4xvAg0JM`
---
Writeup en progreso — más niveles próximamente.
