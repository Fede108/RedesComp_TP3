# Trabajo práctico 3: Evaluación de performance en redes y ruteo interno dinámico Open Shortest Path First (OSPF)

### Integrantes del Grupo:

Federico Cechich

Juan Manuel Ferrero

Luciano trachta

Profesor: SANTIAGO MARTIN HENN


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






