# Trabajo práctico 3: Evaluación de performance en redes y ruteo interno dinámico Open Shortest Path First (OSPF)

### Integrantes del Grupo:

Federico Cechich

Juan Manuel Ferrero

Luciano trachta

Profesor: SANTIAGO MARTIN HENN


## 1. Introducción Teórica a OSPF

## Introducción teórica a OSPF, clases de redes y algoritmos de shortest path

**OSPF (Open Shortest Path First)** es un protocolo de enrutamiento interior (IGP) basado en el estado de enlace. A diferencia de los protocolos de vector de distancia, OSPF permite que cada router tenga una visión completa de la topología de red dentro de su área, lo que le permite calcular rutas más eficientes. Para ello, cada router intercambia información de estado de enlace con sus vecinos y construye una base de datos topológica compartida.

Las **clases de redes** (Clase A, B y C) fueron utilizadas originalmente para dividir el espacio de direcciones IPv4 en bloques según el tamaño de las redes. Aunque el enrutamiento moderno se basa en CIDR (Classless Inter-Domain Routing), entender estas clases sigue siendo útil para comprender cómo se estructuran las redes y cómo OSPF puede organizar grandes topologías mediante áreas jerárquicas.

El algoritmo de **camino más corto** utilizado por OSPF es el **algoritmo de Dijkstra**, el cual permite calcular la ruta más corta desde el router local (nodo raíz) hacia todos los demás routers en la red. Este cálculo se basa en un grafo ponderado, donde los vértices representan routers y las aristas representan enlaces con un costo asignado.

### Teoría de grafos aplicada a OSPF

OSPF modela la red como un **grafo dirigido y ponderado**:

- **Vértices (nodos)**: representan routers.
- **Aristas (enlaces)**: representan las conexiones físicas o virtuales entre routers.
- **Pesos**: representan el costo de utilizar un enlace, que puede depender del ancho de banda, la latencia u otros factores.

Cada router construye este grafo mediante la recopilación de LSAs (Link-State Advertisements) y aplica el algoritmo de Dijkstra para generar su tabla de enrutamiento óptima. Así, la teoría de grafos proporciona la base matemática sobre la que se sustenta el funcionamiento interno de OSPF.

