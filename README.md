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


# b) Leer las entradas de la Base de Datos de Estado de Enlace (LSDB) en cada uno de los routers.

show ip ospf database

Este comando te muestra todos los LSAs que el router conoce, y forman el "mapa de la red" de OSPF.

Router 1:

![image](https://github.com/user-attachments/assets/3920f99a-de22-4d69-80c6-b978b22d7828)

## 6. Se definio las áreas de OSPF de la siguiente manera: R1 y R2 están en el área A, R4 y R5 están en el área B y R3 actua como router frontera

![image](https://github.com/user-attachments/assets/347c7275-efdc-498e-b0d5-040e2cee26ce)

![image](https://github.com/user-attachments/assets/b33708f7-e4ab-411a-a9e4-598e780e9720)


# 7) Verificar el funcionamiento de OSPF:

## a) En el router R2 consultar la información acerca de los vecinos R1 y R3 de OSPF.

![image](https://github.com/user-attachments/assets/d40bfea2-e8ac-4d19-8d46-ce1507e47189)

![image](https://github.com/user-attachments/assets/7501ede6-a664-471e-b7bb-3d4a15062e53)

## b) En el router R2 consultar la información sobre las operaciones del protocolo de enrutamiento.

 show ip protocols
 
Este comando te muestra:

- Qué protocolo de enrutamiento se está usando (en este caso OSPF).
- Las redes que está anunciando.
- Los tiempos de actualización.
- Las interfaces involucradas.
- El router ID.


En router 3:

![image](https://github.com/user-attachments/assets/1a979997-33b4-4da7-a145-92e2d5803a47)

Esto muestra las rutas aprendidas específicamente por OSPF. Si ves entradas que comienzan con una "O", significa que están siendo aprendidas por OSPF.

![image](https://github.com/user-attachments/assets/c768a62b-b495-4197-932c-1d7dbdaa7e02)

# 8. Configurar el costo de OSPF 

El tráfico desde h4 (10.0.2.1) hacia h1 (10.0.1.1) puede ir por dos trayectos. Por defecto, todos los enlaces tienen coste=1, así que OSPF elegirá el camino con menor número de saltos (R4–R3–R2–R1).

Este es el gigabitethernet0/0 del router 4 conectado al router 5. Costo 1.

![image](https://github.com/user-attachments/assets/1491f834-6e51-464e-bd0c-1482360df221)

Aqui es desde el router 5 al router 3 y también tiene costo 1. 

![image](https://github.com/user-attachments/assets/a1b26b8c-b222-41d8-8757-eed9491556e3)

Configuro del router 4 al 5 para que el coste sea 100. Lo mismo se hizo para el router 5 al 3.

![image](https://github.com/user-attachments/assets/d47efbc6-5331-4b67-9535-c0287f281520)

En esta imagen vemos como el enlace del router 4 al 3 y del router 5 al 3 el costo es 100, va de h4 al router 4, en el paso 2 va del router 4 al router 3, en el paso 3 va del router 3 al router 2 y en el paso 4 llega a h1.

![image](https://github.com/user-attachments/assets/41d2b9c9-16fc-40c6-9d84-7dce2be0cfbf)

En cambio en esta imagen, le pusimos que el coste en el enlace entre el router 5 al router 3 es de 1 otra vez, por lo que en el paso 2 envés de ir del router 4 al 3, va del router 4 al router 5 y de ahí al 3.

![image](https://github.com/user-attachments/assets/2df95617-a8f1-48b9-b63a-0cff940b89f9)


# 9. Redistribuir una ruta OSPF predeterminada

## a) y b) Configurar una dirección de loopback en R1 para simular un enlace a un proveedor de servicios de Internet (ISP). Configurar una ruta estática predeterminada en el router R1.

![image](https://github.com/user-attachments/assets/58b26bc6-6736-4819-90c6-faa79d27024f)

En estas imagenes podemos ver cómo se configuró una ruta estática predeterminada en el router R1. Con esto le decimos: “cualquier destino que no conozca, sácalo por la interface Loopback0(nuestra loopback)”.

## c) Incluir la ruta estática predeterminada en las actualizaciones de OSPF enviadas desde el router R1.

![image](https://github.com/user-attachments/assets/62104c2a-1461-4c4e-b0c9-8e190c288297)

Verificamos que en R1 que se generó el LSA externo.

Link State ID: 0.0.0.0 → es una ruta por defecto.

Advertising Router: 1.1.1.1 → R1 está anunciándola.

Metric Type: 2 → es un costo externo tipo E2, el valor por defecto.

Routing Bit Set → indica que la ruta es válida y está en la tabla de enrutamiento.

Verificamos que se vean en los demás routers:

Router 2:

![image](https://github.com/user-attachments/assets/11d670a6-1d51-4154-b502-7088aaeb185a)

Router 3:

![image](https://github.com/user-attachments/assets/4c15fac5-9bb7-4820-b187-b2cede206b90)


# 10) Fallas de conectividad y su impacto en OSPF

#### a) Falla en el enlace R2 – R1

Cuando se pierde la conexión entre R2 y R1 (`192.168.1.0/30`), ambos routers marcan la interfaz como `down/down`. OSPF detecta la caída de la vecindad, se eliminan las LSAs entre ambos y se recalcula la topología (SPF). El Área 0 se particiona: R1 y sus hosts (h1–h3) pierden acceso al Área 1, aunque esta última sigue funcionando normalmente.

#### b) Falla en el enlace R2 – R3

R2 pierde la interfaz hacia R3 (`192.168.2.0/30`) y ambos routers bajan su adyacencia OSPF. Se eliminan las rutas correspondientes de sus LSDB y se recalcula la topología (SPF) en toda la red. Aunque R2 ya no tiene conexión con el Área 1, este no era el ABR; R3 cumple ese rol. Ambas áreas siguen funcionando internamente, pero se pierde la conectividad entre ellas.

#### c) Falla en el enlace R2 – S1 (LAN1)

Al perderse la conexión entre R2 y el switch S1, la red `10.0.1.0/24` desaparece localmente. R2 actualiza su Router-LSA y propaga el cambio. R1 elimina la red de su LSDB y tabla de rutas. Los hosts h1–h3 quedan sin gateway, pero el resto de la red no se ve afectado.


# 11. ¿La tabla RIB (Routing Information Base) es lo mismo que la tabla FIB (Forwarding Information Base)? Justificar con capturas del práctico.


No son lo mismo las tablas RIB y FIB.

## RIB (Routing Information Base)

Es la tabla de rutas «lógicas» que construye el router a partir de los protocolos de enrutamiento (OSPF, EIGRP, RIP, rutas estáticas, etc.).

Se ve con el comando:

### show ip route

Incluye:
- Código del protocolo (O, D, C, S…)
- Distancia administrativa
- Métrica
- Redes directamente conectadas y aprendidas
  
## FIB (Forwarding Information Base)

Es la tabla optimizada que usa el hardware (o el motor de reenvío) para mandar paquetes al siguiente salto.

Normalmente se obtiene con:

### show ip cef

Contiene sólo:
- Prefijo de red
- Interfaz de salida
- Next-hop
  
No muestra ni distancias administrativas ni códigos de protocolo.

## Justificación práctica en router 1:

## RIB:

![image](https://github.com/user-attachments/assets/f3f09a10-c875-4039-a2ce-356814babaff)

## FIB:

![image](https://github.com/user-attachments/assets/92cc7181-8963-4a50-a265-419a3c92234c)










