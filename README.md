# Despliegue y configuración de un Servidor Terminológico SnowStorm para la interoperabilidad con Registros Clínicos Electrónicos
Snowstorm es un servidor terminológico de código abierto desarrollado por la IHTSDO (International Health Terminology Standards Development Organisation) para gestionar y consultar terminologías clínicas internacionales. Proporciona una API REST para facilitar la integración y el acceso a los datos terminológicos en aplicaciones de salud

El presente repositorio indica como desplegar SnowStorm de forma local y en la nube, cargarle terminologías locales e internacionales y crear una integración interoperable con Bahmni para ser utilizado en su registro clínico electrónico, mostrando así una de sus posibles usabilidades.

# Despliegue y configuración de SnowStorm
Para desplegar el servidor, configurarlo, crear la integración interoperable con Bahmni y subirlo a la nube se debe seguir lo mostrado en los enlaces que están a continuación.
- [Despliegue de SnowStorm](https://github.com/SIMSADIs/Terminology-Server-SnowStorm/blob/deploy-snowstorm/deploy-snowstorm.md)
- [Carga de terminología](https://github.com/SIMSADIs/Terminology-Server-SnowStorm/blob/load-terminology/load-terminology.md)
- [Despliegue de Bahmni e integración con SnowStorm](https://github.com/SIMSADIs/Terminology-Server-SnowStorm/blob/snowstorm-deployment/setup-bahmni.md)
- [Configuración de la nube](https://github.com/SIMSADIs/Servidor-Terminologico-SnowStorm/tree/setting-cloud)


# Requisitos para desplegar y configurar SnowStorm

## Requisitos del sistema

- Sistema operativo Linux
- 8G de memoria RAM
- Disco SSD
- CPU de 4 nucleos

1.- Instalar Zip y Unzip.
```
sudo apt install zip
```
1.- Instalar Git.
```
apt-get install git
```

### Docker

1.- Actualizar lista de paquetes disponibles en el sistema.
```
sudo apt update && sudo apt upgrade -y
```

2.- Utilizar gestor de paquetes apt e instalar certificados y herramienta de transferencia de datos "curl".
```
sudo apt-get install ca-certificates curl
```

3.- Crer un directorio seguro para llaves de repositorios APT.
```
sudo install -m 0755 -d /etc/apt/keyrings
```

4.- Descargar y guardar clave GPG de Docker en el sistema.
```
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
```

5.- Otorgar permisos de lectura a todos los usuarios para la clave GPG de Docker.
```
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

6.- Agregar el repositorio de Docker a las fuentes de Apt y actualizar la lista de paquetes.
```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

7.- Instalar Docker Engine, CLI, Containerd, Buildx y Compose plugins.
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### Docker-compose

```
sudo apt-get update
```
```
sudo apt-get install docker-compose-plugin
```

```
COMPOSE_VERSION=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)
```
```
sh -c "curl -L https://github.com/docker/compose/releases/download/${COMPOSE_VERSION}/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose"
```
```
chmod +x /usr/local/bin/docker-compose
```
```
sh -c "curl -L https://raw.githubusercontent.com/docker/compose/${COMPOSE_VERSION}/contrib/completion/bash/docker-compose > /etc/bash_completion.d/docker-compose"
```

Para revisar que se instaló correctamente y la versión que quedó en el entorno se puede usar el comando:

```
docker-compose --version
```


## Requisitos para carga de terminología a SnowStorm

Se necesitan instalar los recursos:
- Java
- HAPI-FHIR Tool
  

### Instalar Java
1.- Primero se actualiza la lista de paquetes de software disponibles en los repositorios oficiales.
```
sudo apt update
```
2.- Luego, se instala Java Runtime.
```
sudo apt install openjdk-17-jre-headless  
```

### Instalar HAPI-FHIR Tool

Para instalar Hapi-Fhir Tool se utiliza la documentación entregada en https://hapifhir.io/hapi-fhir/docs/tools/hapi_fhir_cli.html.

1.- Descargar el archivo comprimido llamado hapi-fhir-[versión]-cli.zip desde https://github.com/hapifhir/hapi-fhir/releases .

2.- Descomprime el archivo descargado, utiliza la terminal, ubícate en el directorio donde se encuentra el archivo e ingresa el siguiente comando, recuerda cambiar la versión a la que estés ocupando.
```
unzip hapi-fhir-7.4.2-cli.zip
```

3.- Ejecuta el archivo hapi-fhir-cli.jar.
```
java hapi-fhir-cli.jar
```

4.- Inicia HAPI-FHIR Tool.
```
 ./hapi-fhir-cli
```

## Requisitos para desplegar Bahmni


Para desplegar Bahmni se necesita docker, docker-compose y Java instalados de la forma ya mostrada, adicional a ello se debe instalar Tomcat en su versión más reciente, la que se utilizó en este trabajo fue la 10.

1.- Se actualizan los paquetes.
```
sudo apt-get update
```
2.- Se instala tomcat.

```
sudo apt install tomcat10 tomcat10-admin
```

