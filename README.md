<h1 align="center">IPFS</h1>

<p align="center">
<img src="images/ipfs.png">
</p>

Sistema de archivos [Inter-Planetario](https://es.wikipedia.org/wiki/Sistema_de_archivos_interplanetarios)

<sumary>

**¿Qué es IPFS?**
</sumary>
<details>

InterPlanetary File System (IPFS) es un [protocolo](https://es.wikipedia.org/wiki/Protocolo_de_comunicaciones) y una red diseñados para crear un método [peer-to-peer](https://es.wikipedia.org/wiki/Peer-to-peer) de almacenamiento y distribución de contenido [hipermedia](https://es.wikipedia.org/wiki/Hipermedia) en un [sistema de archivos distribuido](https://en.wikipedia.org/wiki/Clustered_file_system#Distributed_file_systems).

IPFS es un sistema de archivos distribuido de par a par que busca conectar todos los dispositivos informáticos con el mismo sistema de archivos. IPFS podría verse como un único grupo de [BitTorrent](https://es.wikipedia.org/wiki/BitTorrent), intercambiando objetos dentro de un repositorio [Git](https://es.wikipedia.org/wiki/Git). En otras palabras, IPFS proporciona un modelo de almacenamiento en [bloque](https://es.wikipedia.org/wiki/Bloque_(informática)) de alto rendimiento y con dirección de contenido, con [hipervínculos](https://es.wikipedia.org/wiki/Hiperenlace) con dirección de contenido.

Se puede acceder al sistema de archivos de varias maneras, incluyendo a través de [FUSE](https://es.wikipedia.org/wiki/Sistema_de_archivos_en_el_espacio_de_usuario) y de [HTTP](https://es.wikipedia.org/wiki/Protocolo_de_transferencia_de_hipertexto).

* [Repositorio](https://github.com/ipfs/ipfs) IPFS.

* [Documentación](https://docs.ipfs.io/introduction/overview/) IPFS.

</details>

Vamos a desplegar nuestro propio nodo en la red IPFS.
Se trata de una red p2p que reutiliza mucho código de Ethereum.

Primero nos aseguramos de que tenemos instalado [Go](https://golang.org):

```
go version

go version go1.11.5 linux/amd64
```

<sumary>

:: **Si no lo tenemos instalado, seguimos los siguientes pasos** ::
</sumary>
<details>
  
```
wget -c 'https://dl.google.com/go/go1.11.5.linux-amd64.tar.gz' -O go1.11.5.linux-amd64.tar.gz 
```

```
sudo tar -C /usr/local -xzf go1.11.5.linux-amd64.tar.gz 
```

```
sudo rm -Rf go1.11.5.linux-amd64.tar.gz 
```
**Añadimos lo siguiente en nuestro `.profile`**

```
PATH="\$PATH:/usr/local/go/bin"
GOPATH="\$HOME/go"
PATH="\$PATH:\$GOROOT/bin:\$GOPATH/bin"
```
> Podemos hacerlo directamente copiando lo siguiente en nuestra terminal:

```
cat <<EOT >> ~/.profile
#Go:
PATH="\$PATH:/usr/local/go/bin"
GOPATH="\$HOME/go"
PATH="\$PATH:\$GOROOT/bin:\$GOPATH/bin"
EOT
```
> Recargamos nuestro profile con:

```
source /home/$USER/.profile 
```

</details>

Después, descargamos el [tarball de IPFS](https://dist.ipfs.io/go-ipfs)

> En su web vemos todas las versiones compiladas con `Go` de IPFS.
> La última versión en estos momentos es la `v0.4.9`

Descargamos el `tar` con este comando:

```
wget https://dist.ipfs.io/go-ipfs/v0.4.9/go-ipfs_v0.4.9_linux-amd64.tar.gz
```

Lo descomprimimos en cualquier carpeta:

```
tar xzvf go-ipfs_v0.4.9_linux-amd64.tar.gz
```

Y ejecutamos el installer que lleva dentro:

```
sudo ./go-ipfs/install.sh
```

> Lo hacemos con sudo porque el instalador tratará de colocar el binario de IPFS
en una carpeta del sistema que está en nuestro PATH pero que tiene accesos restringidos.

```
/usr/local/bin
```

<sumary>

# Configuración
</sumary>
<details>
Antes de iniciar nuestro sistema IPFS podemos personalizar algunas variables.
Por ejemplo, podemos especificar donde se guardará la base datos local.

Para ello declaramos la variable de sistema correspondiente.
Hay 2 maneras, a nivel de todo el sistema, añadiendo la variable en el servicio
del sistema:

```
IPFS_PATH=/data/ethereum/ipfs
```

O bien a nivel del usuario. Añadiendo la variable en 

```
~/.bashrc
export IPFS_PATH=/data/ethereum/ipfs
```

En nuestro caso, como vamos a correr el daemon de ipfs como un servicio del
sistema hemos preferido hacerlo en el servicio.

Editamos nuestro archivo de servicio:

```
sudo vim /etc/systemd/system/ipfs.service
```

Y añadimos el siguiente contenido:

```
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
```

Tras esto podemos habilitar el servicio para que se arranque automáticamente en cada reinicio.

```
sudo systemclt 
```
</details>