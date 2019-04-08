# IPFS
Sistema de archivos Inter Planetario

Vamos a desplegar nuestro propio nodo en la red IPFS.
Se trata de una red p2p que reutiliza mucho código de ethereum.

Primero nos aseguramos de que tenemos instalado go:

$ go version
go version go1.11.5 linux/amd64

Despues descargamos el tarball de IPFS desde:

https://dist.ipfs.io/go-ipfs

En esta web vemos todas las versiones compiladas con go de IPFS.
La última versión en estos momentos es la: v0.4.9

De manera que descargamos el tar con este comando:

$wget https://dist.ipfs.io/go-ipfs/v0.4.9/go-ipfs_v0.4.9_linux-amd64.tar.gz

Lo descomprimimos en cualquier carpeta:

$tar xzvf go-ipfs_v0.4.9_linux-amd64.tar.gz

Y ejecutamos el installer que lleva dentro:

$sudo ./go-ipfs/install.sh

Lo hacemos con sudo porque el instalador tratará de colocar el binario de IPFS
en una carpeta del sistema que está en nuestro PATH pero que tiene accesos restringidos.

/usr/local/bin

# Configuración

Antes de iniciar nuestro sistema IPFS podemos personalizar algunas variables.
Por ejemplo, podemos especificar donde se guardará la base datos local.

Para ello declaramos la variable de sistema correspondiente.
Hay 2 maneras, a nivel de todo el sistema, añadiendo la variable en el servicio
del sistema:

IPFS_PATH=/data/ethereum/ipfs

O bien a nivel del usuario. Añadiendo la variable en 

~/.bashrc
export IPFS_PATH=/data/ethereum/ipfs

En nuestro caso, como vamos a correr el daemon de ipfs como un servicio del
sistema hemos preferido hacerlo en el servicio.

Editamos nuestro archivo de servicio:

$sudo vim /etc/systemd/system/ipfs.service

Y añadimos el siguiente contenido:

[Unit]
Description=IPFS Daemon
After=syslog.target network.target remote-fs.target nss-lookup.target

[Service]
Environment="IPFS_PATH=/data/ethereum/ipfs"
Type=simple
ExecStart=/usr/local/bin/ipfs daemon
User=user

[Install]
WantedBy=multi-user.target

Tras esto podemos habilitar el servicio para que se arranque automáticamente en cada reinicio.

$sudo systemclt 



