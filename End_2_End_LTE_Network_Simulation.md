# End 2 End LTE Network Simulation
Este tutorial muestra de manera simplificada la implementación de una red LTE en un solo computador, utilizando el proyecto SrsRAN (https://www.srslte.com/). Esta solución constituye un primer acercamiento a un esquema de radio definidio por software en donde no es necesario contra con hardware (como por ejemplo una USRP), ni tampoco es necesario instalar su driver (UHD). 

En este sentido, sólo será necesario realizar una compilación desde los archivos fuente de la herramienta. 

Instalación
=============

Operating System
-------------
La red completa se ejecutará sobre una máquina con sistema operativo Linux. Para una versión ligera, se recomienda decargar e instalar LinuxMint: Las siguienets indicaciones de instalación han sido implementadas utilizando la versión LinuxMint 22 Wilma de 64 bits disponible en https://linuxmint.com/download.php. 

Esta instalación se realizó mediante una máquina virtual instalada mediante Virtual Box (https://www.virtualbox.org/), asignando un tamaño de disco dinámico y 2 GB de memoria RAM. 

Recursos en VirtualBox
![](https://github.com/DiegoRenza/Mobile-Communications/blob/main/LinuxMint_Resources.png)

Versión del sistema operativo
![](https://github.com/braayans/Mobile-Communications/blob/main/LinuxMint22.png)

Instalar el driver ZeroMQ
-------------

Instalar libtool (ejecutar desde la carpeta raíz):

```python 
sudo apt-get update
sudo apt-get install libtool 
```

Instalar libzmq
```python 
git clone https://github.com/zeromq/libzmq.git
cd libzmq
./autogen.sh
./configure
make
sudo make install
sudo ldconfig
```

Instalar czmq (instalar desde carpeta raíz)
```python
git clone https://github.com/zeromq/czmq.git
cd czmq
./autogen.sh
./configure
make
sudo make install
sudo ldconfig
```


Instalación de librerías y paquetes para SrsRAN
-------------

Abrir una venta de terminal en Linux Mint, y ejecutar lo siguiente:

Instalación de librerías
```python 
sudo apt-get install build-essential cmake libfftw3-dev libmbedtls-dev libboost-program-options-dev
libconfig++-dev libsctp-dev
```

Instalación de Git
```python 
sudo apt install git
```

Descargar y compilar SrsRAN para uso de 4G.
```python 
git clone https://github.com/srsRAN/srsRAN_4G.git
```

Crear carpeta build desde la carpeta srsRAN_4G y acceder a esta
```python
cd srsRAN_4G
mkdir build && cd build
```

Instalar el paquete gcc-11 y g++-1 y exportar estos en la carpeta build
```python
sudo apt install gcc-11 g++-11
export CC=$(which gcc-11)
export CXX=$(which g++-11)
```

A continuacion, se abandona la carpeta build para aplicar el comando cmake de la siguiente manera
```python
cd ..
cmake ./ -B build
cmake --build build -j$(nproc)
```

Ingresar nuevamente a la carpeta build para continuar con el proceso
```python
cd build
make
make test
sudo make install
sudo srsran_install_configs.sh service
```

Compilar SrsRAN (desde la carpeta srsRAN/build)
```python
cmake ../
make
```

Es necesario verificar que la salida de la compilación incluya la detección del driver en las siguientes líneas:
```python
...
-- FINDING ZEROMQ.
-- Checking for module 'ZeroMQ'
-- No package 'ZeroMQ' found
-- Found libZEROMQ: /usr/local/include, /usr/local/lib/libzmq.so
...
```
