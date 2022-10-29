# Obsidian Template

This is a template for an Obsidian vault that syncs between PC and Mobile (Android)

## Install and Setup

Los comandos están entre comillas oblicuas (`). Copia lo que hay entre ellas, pero **no copies las comillas**.

Los bloques para copiar están entre triple comillas oblicuas (```) que es como se designan bloques de código. Copia lo que hay entre ellas, pero **no copies las comillas**.

1. Descarga e instala [F-Droid][fdroid]
2. Descarga e instala [Termux][termux]
3. Abre Termux
4. Actualiza la lista de paquetes con `apt -qy update`
5. Actualiza los paquetes con `apt -qy upgrade`
   * Te preguntará varias veces si quieres reemplazar archivos -> responde que sí con `y`
6. Instala SSH para actualizar automáticamente con `pkg install -y openssh`
7. Genera tu clave de SSH con `ssh-keygen -t rsa -b 4096 -f ~/.ssh/[nombre de la clave]`
    * e.g. ssh-keygen -t rsa -b 4096 -f ~/.ssh/BojMobile
    * Te pedirá una passphrase (algo así como una contraseña) pero puedes dejarlo en blanco
8. Muestra la clave pública con `cat ~/.ssh/[nombre de la clave].pub`
9. Copia el valor de la clave pública
    * empieza por ssh-rsa
    * termina por usuario@maquina
      * e.g. u0_r56@localhost
10. Ve a tu cuenta de git > Configuración > Claves SSH y GPG > Añadir clave SSH
11. Ponle un nombre en el campo 'Título'
12. Pega la clave pública en el campo 'Clave'
13. Haz clic en el botón "Añadir clave SSH"
14. Vuelve a Termux
15. Crea un archivo de configuración de SSH para facilitar el uso de git con el comando `nano ~/.ssh/config`
    * el comando te abrirá un editor
16. Escribe lo siguiente:

    ```text
    Host github
        HostName github.com
        User git
        IdentityFile ~/.ssh/[nombre de la clave]
    ```

    * e.g.:

    ```text
    Host github
        HostName github.com
        User git
        IdentityFile ~/.ssh/BojMobile
    ```

17. Guarda los cambios con Ctrl+O + Intro
18. Cierra con Ctrl+X
19. Comprueba que tienes la configuración correcta con `ssh github`
    * la primera vez, te pedirá confirmación para conectar, responde que sí con `yes
    * Si todo ha ido correctamente verás un mensaje como el siguiente:
      Hi [username]! You've succesfully authenticated, but GitHub does not provide shell access.
      Connection to github.com closed.
20. Instala Git para actualizar automáticamente con `pkg install -y git`
21. Configura tu usuario de git con `git config --global user.name "TU USUARIO"`
    * e.g. git config --global user.name "Borja LoFe"
22. Configura tu correo de git con `git config --global user.email "tu.correo@tudominio.com"`
    * e.g. git config --global user.email "im@borjalofe.com"
23. Configura Termux para acceder a tus carpetas con `termux-setup-storage`
    * Android solicitará permiso para que Termux pueda conectarse, acepta para poder continuar
24. Ve a GitHub y crea el repo para tu bóveda
25. Vuelve a Termux
26. Accede a tus carpetas con `cd storage/shared/`
27. Clona (copia) tu repo con `git clone git@github.com:[username]/[nombre del repo].git`
    * e.g. mi usuario es 'borjalofe' y el repo de mi bóveda es 'obsidian' -> `git clone git@github.com:borjalofe/obsidian.git`
28. Ya puedes abrir Obsidian y cargar tu bóveda desde la carpeta 'obsidian'
    * Con esto ya podrías hacerlo con actualizaciones manuales pero puedes seguir para configurar las actualizaciones automáticas
29. Instala wget con `pkg install -y wget`
30. Ve a la carpeta de documentos con `cd ~/storage/shared/Documentos`
    * Ojo, puede que la tuya se llame de otra manera, algo tipo 'Documents' o 'documentos'
31. Descarga el guión de actualización con `wget https://gist.githubusercontent.com/lucidhacker/0d6ea6308997921a5f810be10a48a498/raw/386fd5c640282daeaa3c9c5b7f4e875511c2946c/zk_sync.sh`
32. Abre el guión para adaptarlo con `nano zk_sync.sh`
33. En la primera línea:
    * cámbiala por `#!/data/data/com.termux/files/usr/bin/bash`
    * verás que empieza por '#!/'
34. Busca la línea que empieza por 'ZK_PATH=' y cambia lo que hay entre comillas dobles (") por la ruta a tu bóveda: `/data/data/com.termux/files/storage/shared/[repo de la bóveda]`
    * si has seguido los pasos que te he ido indicando, será la siguiente: `/data/data/com.termux/files/storage/shared/obsidian`
35. Opcional, personaliza el mensaje que se generará con cada automatización. Para ello, busca `git commit -q -m "Last Sync: $(date +"%Y-%m-%d %H:%M:%S")"` y cambia `Last Sync: $(date +"%Y-%m-%d %H:%M:%S")` por lo que quieras
    * el código `$(date +"%Y-%m-%d %H:%M:%S")` genera la fecha actual, así que lo dejaría para facilitar el seguimiento
36. Instala CRON para actualizar automáticamente con `pkg install -y cronie termux-services`
37. Cierra Termux con `exit`
38. Abre Termux (acabas de reiniciar para actualizar la configuración)
39. Abre la configuración de CRON con `crontab -e`
40. Configura CRON para que use el guión cada 30 minutos escribiendo `*/30 * * * * bash ~/storage/shared/Documentos/zk_sync.sh`
    * Ojo, puede que la tuya se llame de otra manera, algo tipo 'Documents' o 'documentos'
    * Personalmente, no encuentro sentido a que se actualice la bóveda mientras duermo, por lo que te sugiero que configures solo en horario diurno con `*/30 6-20 * * * bash ~/storage/shared/Documentos/zk_sync.sh`
      * Traducción: sincroniza cada media hora entre las 6 de la mañana y las 8 de la tarde.
41. Guarda los cambios con Ctrl+O + Intro
42. Cierra con Ctrl+X
43. En el móvil, clic sobre el icono cuadrado (que te muestra todas las apps abiertas)
44. Mantén pulsado sobre Termux un rato para que te salgan las opciones de la app
45. Selecciona 'Bloquear' (puede que te aparezca como 'Lock' o como un candado)
    * de esta manera, Termux no se cerrará y ejecutará la actualización de la bóveda "constantemente"

## Features

## Sources

1. Lucid's Newsletter: [Setting up Git syncing for Obsidian on Android][1]
2. Violeta González-Pérez: [Obsidian con Github en dispositivos móviles][2]
3. Samuel Wong's Desktop of Samuel: [How to sync Obsidian vault for free using Git?][3]

## Status

This project is closed but will be updated as needed.

## Contact

Created by [@borjalofe][github] - feel free to contact me!

[fdroid]: https://f-droid.org/F-Droid.apk
[github]: https://github.com/borjalofe/
[termux]: https://f-droid.org/repo/com.termux.118.apk
[1]: https://lucidhacker.substack.com/p/setting-up-git-syncing-for-obsidian
[2]: https://viogp.github.io/2021-11-29-obsidian-movil-gt/
[3]: https://desktop.ofsamuel.com/how-to-sync-obsidian-vault-for-free-using-git
