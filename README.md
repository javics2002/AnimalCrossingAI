# IAV23-CanoSalcedo
Proyecto final para la asignatura Inteligencia Artificial para Videojuegos en el grado de Desarrollo de Videojuegos de la UCM

## Propuesta
Se busca crear un prototipo de videojuego de simulación de vida inspirado en Animal Crossing en el que el jugador y un pequeño número de NPCs viven tranquilamente en un pequeño pueblo.

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/49aebc01-20bc-4c57-89be-f67c55fd2fca)

Desde pequeño he jugado a Animal Crossing, y creo que es el juego al que más horas le he echado, así que también sé que uno de sus puntos flojos son sus NPCs. En los primeros juegos al menos parecía que según su amistad con el jugador, podían ser más amables o más bordes, pero esque con las últimas entregas ya ni siquiera pueden enfadarse con el jugador, quitándole aún más profundidad a estos personajes que viven junto a nosotros. Al final, creo que el único propósito que tienen en el juego es que sean “monos” pero no aportan ningún beneficio relevante al jugador y solo están paseándose. Jugadores como yo ya conocemos todas las frases que nos pueden decir y acabamos saltando todos los diálogos para que nos dejen en paz.

Con este proyecto no pretendo hacer Animal Crossing pero mejor, sino más bien un experimento implementando NPCs un poco más profundos, con una rutina y necesidades que satisfacer y comprobando si esta experiencia promete ser más interesante para el jugador, o, por el contrario, “da igual” que se comporten de forma más realista. Eso por la parte de comportamiento, pero por la parte de diálogo, está claro que no puedo escribir la gran cantidad de texto que implica conseguir un efecto de que los NPCs tengan cosas distintas que decir cada vez y me quedaría aún peor que el juego original. Me conformaré con que te hablen de lo que han decidido hacer, por ejemplo, si un NPC tiene hambre pero es vago, en vez de ir a comprar comida le pedirá al jugador si tiene una manzana. Otra alternativa que quiero investigar es la implementación de IAs externas a los videojuegos, y con esto me refiero efectivamente a ChatGPT. Si fuera posible integrarlo en el proyecto y describirle la personalidad, los gustos y el objetivo actual del NPC, podría generar el diálogo que necesito en tiempo de ejecución. Sin embargo, aunque esto fuera posible, creo que se notaría, que no pasaría el test de Turing, por expresarlo de alguna manera.

## Contexto
Animal Crossing es un juego de simulación de vida, en el que el jugador se muda a un nuevo pueblo con el único objetivo de disfrutar la tranquilidad, recoger frutas, cazar bichos… y de pagar tu casa constantemente. 

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/4f56bc21-e228-43ec-89e9-c61c77bafd64)

Existen 2 tipos de NPCs en estos juegos: los especiales y tus vecinos. Los NPCs especiales son personajes únicos con un rol concreto en el juego que todo el mundo tiene en su pueblo, como Tom Nook, el encargado de la tienda, o Canela, la secretaria del jugador. Los vecinos por otra parte, son habitantes sin ningún rol especial, y cada uno tiene aleatoriamente unos pocos viviendo en el pueblo, ya que hay más de 300. 

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/08609bdf-d9f0-4061-9678-a769da69d253)

Lo que hace el juego para diseñar todos estos vecinos es clasificarlos en especies, y retexturizar el modelo de esa especie, darle una personalidad y ya tienes un nuevo vecino. Hay muchas especies: perros, gatos, tigres, canguros, pingüinos, conejos… y de cada especie existen aproximadamente 10 variaciones. Lo importante para nosotros es la parte de la personalidad. La personalidad de un vecino de Animal Crossing define qué conjunto de diálogos usa el personaje para comunicarse. De forma que 2 vecinos distintos con la misma personalidad hablarán exactamente igual y serán indistinguibles desde el punto de vista de los diálogos. Las personalidades que encontramos en Animal Crossing se encuentran separadas por género y son las siguientes:

Personalidad | Género | Descripción
--- | --- | --- 
Normal | Femenino | Agradables, despreocupadas, limpias. Se llevan mal con los gruñones.
Dulce | Femenino | Atentas con el jugador, francas. Irrita a las presumidas y a los gruñones.
Presumida | Femenino | Les encanta la moda, cuidar su imagen y son egocéntricas. No se llevan bien con el resto de vecinos.
Alegre | Femenino | Cantan, bailan y se creen super stars del pop. Irrita a las presumidas y a los gruñones.
Atlético | Masculino | Obsesionados con el deporte.
Esnob | Masculino | Educados y hablan de sí mismos.
Gruñón | Masculino | Con mala leche, pero se abren con la amistad.
Perezoso | Masculino | Les encanta dormir y comer chuches. Se llevan mal con los atléticos

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/8c3b5119-a566-4590-af75-9afbb91221c1)

## Diseño
En el prototipo, los vecinos tendrán una serie de parámetros que representarán sus necesidades, y contaremos con distintas acciones asociadas con cada una que las satisfarán, y cada NPC decidirá cuál es la más importante en cada momento y cómo lidiar con ella

Necesidad | Aumento | Descenso
--- | --- | --- 
Sueño | Lento | Dormir
Hambre | Medio | Comer
Limpieza | Muy lento | Ducharse
Sed | Rápido | Beber
Aburrimiento | Muy rápido | Jugar
Frío | Factor externo | Abrigarse o refugiarse
Calor | Factor externo | Refrescarse o refugiarse
Lluvia | Factor externo | Usar paraguas o refugiarse
Ir al baño | Rápido | Ir al baño
		
Además, contarán con más parámetros que definirán su personalidad, y, por tanto, afectarán a las decisiones que toman:
- Tolerancia al sueño	
- Tolerancia al hambre	
- Tolerancia a la limpieza	
- Tolerancia a la sed	
- Tolerancia al aburrimiento	
- Tolerancia al frío	
- Tolerancia al calor	
- Salario diario	
- Apego al dinero	
- Extroversión	
- Gustos	
- Disgustos

Por último, contarán una rutina con la que cumplirán siempre que no les interrumpa ninguna necesidad básica.
En el mundo existirán puntos para cada satisfacer todas las necesidades de nuestros habitantes.

## Desarrollo
## Resultados
## Conclusion
