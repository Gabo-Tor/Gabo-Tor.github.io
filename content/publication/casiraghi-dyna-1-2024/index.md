---
title: 'Dyna-1: Desarrollo, Construcción y Pruebas Experimentales de un Robot Cuadrúpedo
  de Diseño Abierto'
authors:
- Tadeo Casiraghi
- gabo
- Roberto Bunge
- Ignacio Mas
date: '2024-06-01'
publishDate: '2024-09-01T22:33:48.157574Z'
publication_types:
- paper-conference
publication: '*XII Jornadas Argentinas de Robótica*'
abstract: Presentamos un nuevo diseño de cuadrúpedo de 12 grados de libertad, centrado
  en reducir el costo y la complejidad de fabricación y maximizar el uso de componentes
  disponibles regionalmente. Para facilitar el ensamblado, las piernas son modulares,
  y los motores están lo mas cerca posible del cuerpo para reducir su inercia. Se
  utilizan motores brushless junto a controladores Odrive que permiten un control
  simple con su interfaz por CAN. El cuerpo contiene los motores de los hombros y
  toda la electrónica necesaria para su funcionamiento. Para controlar el cuadrúpedo
  se implementa el sistema operativo ROS2. Todos los procesos cruciales para el funcionamiento
  del robot se corren en la computadora a bordo, y los demás en una computadora externa.
  Se demuestra experimentalmente que cada pierna es capaz de saltar hasta 30 cm con
  un peso similar a un cuarto del cuerpo. Se ensayó el robot con peso agregado de
  hasta 4 kg y se observa buen rendimiento. Se creó un simulador en Pybullet para
  evaluar algoritmos de autonomía de manera eficiente. El costo total del robot es
  de aproximadamente 3000 USD. Todo el software y los modelos del hardware son de
  diseño abierto y pueden encontrarse en un repositorio público. https://github.com/udesa-ai/dyna1-quadruped
---
