# Trabajo práctico 3: Evaluación de performance en redes y ruteo interno dinámico Open Shortest Path First (OSPF)

*Facultad de Ciencias Exactas, Fisicas y Naturales de la U.N.C**

**Redes de Computadoras**

**Profesores**
- Santiago M. Henn
- Facundo N. Oliva C.
  
**Nombre del grupo : Kiritoro** 

**Integrantes**
- Federico Cechich
- Juan Manuel Ferrero
- Luciano Trachta


**24 de Abril de 2025**


**Información de contacto**:  _juan.manuel.ferrero@mi.unc.edu.ar_, _federico.cechich@mi.unc.edu.ar_, _ltrachta@mi.unc.edu.ar_ 

# 1. Introducción Teórica a OSPF


**OSPF (Open Shortest Path First)** es un protocolo de enrutamiento interior (IGP) basado en el estado de enlace. A diferencia de los protocolos de vector de distancia, OSPF permite que cada router tenga una visión completa de la topología de red dentro de su área, lo que le permite calcular rutas más eficientes. Para ello, cada router intercambia información de estado de enlace con sus vecinos y construye una base de datos topológica compartida.

Las **clases de redes** (Clase A, B y C) fueron utilizadas originalmente para dividir el espacio de direcciones IPv4 en bloques según el tamaño de las redes. Aunque el enrutamiento moderno se basa en CIDR (Classless Inter-Domain Routing), entender estas clases sigue siendo útil para comprender cómo se estructuran las redes y cómo OSPF puede organizar grandes topologías mediante áreas jerárquicas.

El algoritmo de **camino más corto** utilizado por OSPF es el **algoritmo de Dijkstra**, el cual permite calcular la ruta más corta desde el router local (nodo raíz) hacia todos los demás routers en la red. Este cálculo se basa en un grafo ponderado, donde los vértices representan routers y las aristas representan enlaces con un costo asignado.

### Teoría de grafos aplicada a OSPF

OSPF modela la red como un **grafo dirigido y ponderado**:

- **Vértices (nodos)**: representan routers.
- **Aristas (enlaces)**: representan las conexiones físicas o virtuales entre routers.
- **Pesos**: representan el costo de utilizar un enlace, que puede depender del ancho de banda, la latencia u otros factores.

Cada router construye este grafo mediante la recopilación de LSAs (Link-State Advertisements) y aplica el algoritmo de Dijkstra para generar su tabla de enrutamiento óptima. Así, la teoría de grafos proporciona la base matemática sobre la que se sustenta el funcionamiento interno de OSPF.

# 2. Diseño de la Red

Se ha diseñado de red según las especificaciones del TP3 utilizando Cisco Packet Tracer. La red está compuesta por 5 routers (R1 a R5), 1 switch (S1) y 5 hosts (h1 a h5). Los routers están conectados entre sí mediante enlaces seriales, mientras que los hosts se conectan a sus respectivos dispositivos de acceso: los hosts h1, h2 y h3 están conectados al switch S1, mientras que h4 está conectado al router R4 y h5 al router R5. Además, el router R1 tiene una interfaz Loopback configurada, que se utilizará para simulaciones futuras.

![image](https://github.com/user-attachments/assets/bd37b9e0-6a30-400e-a4a8-85a9b98948bd)


## Plan de Direccionamiento IP


| Red            | Máscara Subred     | Dispositivo | Subred          | IP           |
|----------------|--------------------|-------------|------------------|--------------|
| LAN 1          | 255.255.255.0      | h1          | 10.0.1.0/24      | 10.0.1.1     |
| LAN 1          | 255.255.255.0      | h2          | 10.0.1.0/24      | 10.0.1.2     |
| LAN 1          | 255.255.255.0      | h3          | 10.0.1.0/24      | 10.0.1.3     |
| LAN 2          | 255.255.255.0      | h4          | 10.0.2.0/24      | 10.0.2.1     |
| LAN 3          | 255.255.255.0      | h5          | 10.0.3.0/24      | 10.0.3.1     |
| R2 con R1      | 255.255.255.252    | R2          | 192.168.1.0/30   | 192.168.1.1  |
|                | 255.255.255.252    | R1          | 192.168.1.0/30   | 192.168.1.2  |
| R1 con R3      | 255.255.255.252    | R1          | 192.168.3.0/30   | 192.168.3.1  |
|                | 255.255.255.252    | R3          | 192.168.3.0/30   | 192.168.3.2  |
| R2 con R3      | 255.255.255.252    | R2          | 192.168.2.0/30   | 192.168.2.1  |
|                | 255.255.255.252    | R3          | 192.168.2.0/30   | 192.168.2.2  |
| R3 con R4      | 255.255.255.252    | R3          | 192.168.4.0/30   | 192.168.4.1  |
|                | 255.255.255.252    | R4          | 192.168.4.0/30   | 192.168.4.2  |
| R3 con R5      | 255.255.255.252    | R3          | 192.168.5.0/30   | 192.168.5.1  |
|                | 255.255.255.252    | R5          | 192.168.5.0/30   | 192.168.5.2  |
| R5 con R4      | 255.255.255.252    | R5          | 192.168.6.0/30   | 192.168.6.1  |
|                | 255.255.255.252    | R4          | 192.168.6.0/30   | 192.168.6.2  |
| R1 loopback    | 255.255.255.255    | R1          | —                | 1.1.1.1      |


# 3. Configurar cada router para que utilice el protocolo OSPF y verificar la conexión punto a punto entre los dispositivos enlazados. Verificar que las tablas de enrutamiento contienen rutas OSPF.

Primero se realizó la configuración de cada router para que utilice el protocolo OSPF.

## Verificación punto a punto

Router 1:

![image](https://github.com/user-attachments/assets/bc948217-0637-45fe-899c-e9bcda9a33c0)

Router 2:

![image](https://github.com/user-attachments/assets/31ef80f2-71ea-48db-94ab-db56ea35701f)

Router 3:

![image](https://github.com/user-attachments/assets/db366545-f1fb-4dc1-99cb-324da853928f)

Router 4:

![image](https://github.com/user-attachments/assets/820b56e7-1c3d-4fa8-9328-ffbfa5247166)

Router 5:

![image](https://github.com/user-attachments/assets/1d6d9da5-8454-4fa7-9e01-e41dbba94009)

## Verifico tablas de enrutamiento que tengan OSPF

Router 1:

![image](https://github.com/user-attachments/assets/4934ec24-5dca-4edf-a112-92bb9e3be35b)

Router 2:

![image](https://github.com/user-attachments/assets/e6225ffb-b9b9-469b-9829-ec829145fbe6)

Router 3:

![image](https://github.com/user-attachments/assets/87ce0c91-7854-4fef-92d2-6a27822a33bd)

Router 4:

![image](https://github.com/user-attachments/assets/1579eaef-f50a-45a9-911c-170983dcf932)

Router 5:

![image](https://github.com/user-attachments/assets/daaf317f-a6fc-4572-86fc-288c92488bd5)



# 4 Analisis de mensajes OSPF

El router 4 le manda un hello al router 3. Vemos que el área id es la que configuramos con los comandos para los routers y el network mask es el correspondiente al router.

![image](https://github.com/user-attachments/assets/332c0c36-2912-4e9f-894f-69988e709cc3)


![image](https://github.com/user-attachments/assets/2bddb5cd-d22e-4da7-97f6-d32cf28d143f)


Luego, en el router 2, se analizo el trafico en forma mas detallada

En base a la dirección de ip más alta, se elige al router 1 como DR y al router 2 como BDR. Además, se negocia quien actúa como master y quien slave antes del intercambio de DB-packets

![image](https://github.com/user-attachments/assets/6014342b-0cee-4c21-a13e-f1494c4ecb57)

Luego se hace el intercambio de DBD (Database Description) con un resumen de su LSDB. 

![image](https://github.com/user-attachments/assets/4281002d-5ab4-497c-af99-2c85974026bd)

Cuando el intercambio termina, se envía un mensaje LSR (Link-State Request) solicitando las LSAs que aún no tiene o que están desactualizadas en su LSDB. El vecino envía esas LSAs dentro de un paquete LSU.
Una vez que las LSDB se encuentran sincronizadas, el intercambio termina y la conexión se marca como full. 


# 5. a) Notificar las redes conectadas directamente al router.

![image](https://github.com/user-attachments/assets/284ebb3c-6fc1-4e2a-89e0-921d6f5f6c63)




## 6. Se definio las áreas de OSPF de la siguiente manera: R1 y R2 están en el área A, R4 y R5 están en el área B y R3 actua como router frontera

![image](https://github.com/user-attachments/assets/347c7275-efdc-498e-b0d5-040e2cee26ce)

![image](https://github.com/user-attachments/assets/b33708f7-e4ab-411a-a9e4-598e780e9720)














