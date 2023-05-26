# NodeRed-Flow1G12 -- 23/MAY/23

En esta práctica se realizará la demostracion de _flows_ utilizando NodeRed montado en un contenedor Docker. Así mismo, se hará mostrará cómo mostrar marcas de tiempo o _timestamps_ con sólo dos nodos. 

## Requisitos previos

1. Tener una máquina virtual con linux (En este caso se utilizó Ubuntu Desktop LTS 22.04) ya corriendo
2. Haber instalado Docker y Docker Compose 

    Nota: Para mayor facilidad utilice la guía oficial de [Docker utilizando su _script conveniente_](https://docs.docker.com/engine/install/ubuntu/#install-using-the-convenience-script)

3. El archivo _composer.yaml_ de [Código IoT](https://github.com/codigo-iot/servidor-IoT-basico-docker-compose) y guardarlo en la siguiente ubicación:

        ~/DockerCompose
 -   Nota 1: Si no cuenta con la carpeta de DockerCompose puede crearla usando

        mkdir ~/DockerCompose

- Nota 2: En el apartado
    ~~~
     image: nodered/node-red:latest
    #volumes:
      #- ~/DockerVolumes/NodeRED/config/settings.js:/data/settings.js
      #- ~/DockerVolumes/NodeRED/data:/data
    ~~~
    Descomente la línea _data_. Debe quedar así:
    ~~~
     image: nodered/node-red:latest
    #volumes:
      #- ~/DockerVolumes/NodeRED/config/settings.js:/data/settings.js
      - ~/DockerVolumes/NodeRED/data:/data
    ~~~

4. Tener su usuario agregado al grupo _docker_ para tener acceso de lectura y escritura en carpetas que se mencionarán más adelante

5. Crear algunas carpetas con los siguientes comandos

        mkdir -p ~/DockerVolumes/Mosquitto/config
        mkdir -p ~/DockerVolumes/MySQL/config
        mkdir -p ~/DockerVolumes/MySQL/data
        mkdir -p ~/DockerVolumes/NodeRED/config
        mkdir -p ~/DockerVolumes/NodeRED/data

- Nota 3: Como puede observar, algunas carpetas parecen estar de más. Pero se utilizarán en un futuro.

## Inicialización de Docker. 

Entre a la carpeta _~DockerCompose_ y abra una terminal ahí. Una vez abierta, ejecute lo siguiente: 

        docker compose up -d

Descargará tanto las imágenes de NodeRed, Mosquitto y MySQL, así como iniciará contenedores nuevos. Este proceso demorará un rato. Compruebe que NodeRed ya funciona yendo a [este enlace](127.0.0.1:1880).

- Nota: En caso de que los contenedores no hayan iniciado utilice el siguiente comando


        docker stop $(docker ps -a -q)
        
        o

        sudo docker stop $(sudo docker ps -a -q)

## NodeRed

Una vez dentro del [navegador](127.0.0.1:1880) NodeRed nos dará la bienvenida. Cerramos y veremos esta interfaz

![Imagen 1](https://raw.githubusercontent.com/Vanadiox/NodeRed-Flow1G12/main/imgs/1.png)

Vamos al menú "hamburguesa" (las tres lineas apiladas) que se encuentra al lado del botón _Deploy_ y damos click en _Manage Palette_.

Se abrirá una ventana. Vaya a donde dice _install_ y en la barra de búsqueda escriba _dashboard_. Seleccione **node-red-dashboard** e instale. Debe quedar como se mira en la siguiente captura. 

![Imagen 2](https://raw.githubusercontent.com/Vanadiox/NodeRed-Flow1G12/main/imgs/2.png)

## Nodos

Ahora procederemos a arrastrar los elementos a la cuadrícula. Empiece arrastrando la caja que dice "inject" y la caja que dice "debug". Quedará como la siguiente imágen. 

![Imagen 3](https://raw.githubusercontent.com/Vanadiox/NodeRed-Flow1G12/main/imgs/3.png)

Como puede notar, _timestamp_ tiene un cuadrito gris en la parte derecha, mientras que _debug1_ tiene uno en la izquierda. Haga click en el cuadrito de _timestamp_ pero no lo suelte. Arrastre hasta el cuadrito de _debug1_ y suelte. Quedará algo como esto: 

![Imagen 4](https://raw.githubusercontent.com/Vanadiox/NodeRed-Flow1G12/main/imgs/4.png)

Ya estamos listos para hacer _deploy_. Haga click en ese boton rojo llamado _Deploy_, el _flow_ ya estaría funcionando. Para comprobarlo, observe que hay un panel blanco a la derecha de la página. Fije su atención ahí, especialmente en el símbolo de un bicho. Este se llama _Debug Messages_. Haga click ahí. Verá que no hay nada, pero no pasa nada. Ahora fije su atención en _timestamp_. Si usted da click en el cuadro grande a la izquierda de este verá que aparecerán mensajes raros. Dentro de los mensajes, si da click en el texto azul, este cambiará a un formato de hora y fecha legible para el ser humano. La siguiente imagen demuestra lo anterior: 

![Imagen 5](https://raw.githubusercontent.com/Vanadiox/NodeRed-Flow1G12/main/imgs/5.png)

Ahora es hora de automatizar el proceso. Hága doble click en _timestamp_. Aparecerá una ventana. Fíjese en la parte inferior de esta. Verá que dice 

        timestamp: none

Haga click en el botón _none_ y seleccione _interval_. Ahora aparecerá como

        every 1 seconds

Usted puede modificar la velocidad de actualización. En este caso se moverá a 5. Ya realizado esto, pique el botón rojo que dice _Done_, en la parte superior del cuadro. La siguiente imagen muestra cómo debe quedar:

![Imagen 6](https://raw.githubusercontent.com/Vanadiox/NodeRed-Flow1G12/main/imgs/6.png)

Ahora vamos con _debug 1_, haciendo clic. Aquí nos interesa la opción de _debug window_. Active la casilla y de clic en _Done_. 

![Imagen 7](https://raw.githubusercontent.com/Vanadiox/NodeRed-Flow1G12/main/imgs/7.png)


Para finalizar, vuelva a dar click en "Deploy" y en automático se verán los mensajes cada cierto tiempo, en este caso, 5 segundos. 

![Imagen 8](https://raw.githubusercontent.com/Vanadiox/NodeRed-Flow1G12/main/imgs/8.png)

# Detener el proyecto

Si desea detener el proyecto, en la misma cinta donde está el ícono del bicho, vaya a la (i). Localice el _flow_ donde está trabajando (En este caso el flow1) y verá dos íconos: un ojo y un círculo. Si deja el cursor sobre el círculo, aparecerá un texto que dice "Disable". 

![Imagen 9](https://raw.githubusercontent.com/Vanadiox/NodeRed-Flow1G12/main/imgs/9.png)

Haga clic y de clic en _Deploy_ para guardar cambios. 

# ¡Y LISTO!

## Referencias

[Código IoT: Servidor básico con Docker Compose](https://github.com/codigo-iot/servidor-IoT-basico-docker-compose)

[Instalación de Docker](https://docs.docker.com/engine/install/ubuntu/#install-using-the-convenience-script)

## Créditos: 

[Manual realizado por Vanadiox (Kevin H)](https://github.com/Vanadiox)

[Referencias por Código IoT](https://github.com/codigo-iot)

