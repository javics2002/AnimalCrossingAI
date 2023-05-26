# Animal Crossing AI
Proyecto final para la asignatura Inteligencia Artificial para Videojuegos en el grado de Desarrollo de Videojuegos de la UCM

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/cefefcc6-858a-4de0-82df-507139787069)

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
- Tolerancia al sueño: cuando el sueño supere esta cantidad, el vecino tendrá sueño
- Tolerancia al hambre: cuando el hambre supere esta cantidad, el vecino tendrá hambre
- Tolerancia a la limpieza: cuando la suciedad supere esta cantidad, el vecino tendrá que ducharse
- Tolerancia a la sed: cuando la sed supere esta cantidad, el vecino tendrá sueño
- Tolerancia al aburrimiento: cuando el aburrimiento supere esta cantidad, el vecino querrá jugar
- Tolerancia al frío: cuando la temperatura baje de esta cantidad, el vecino se refugiará
- Tolerancia al calor: cuando la temperatura supere esta cantidad, el vecino se dará un baño en la playa
- Salario diario: dinero que gana cada día le vecino
- Apego al dinero: a mayor sea este valor, menos cosas tratará de comprar
- Extroversión: a mayor sea este valor, más se divertirá y querrá interactuar con el jugador, y preferirá el exterior al interior
- Evita lluvia: si está activado, el vecino se refugiará en interiores cuando llueva.
- Gustos: lista de tipos de waypoints favoritos
- Disgustos: lista de tipos de waypoints evitados

En el gráfico muestro como cada uno de los vecinos tiene unos valores distintos en estas variables para así ver como afectan en su comportamiento final.

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/814d838b-db52-4bbb-aa3e-9e5fad4e167e)

Cada uno tiene más variables, como el salario que gana y las listas de gustos y disgustos, o su reacción a la temperatura, que no enseño, pero se pueden ver en su blueprint, por ejemplo:

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/e6857427-7880-4a04-be24-8cfa3a95f3bc)
![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/8b038183-5765-4014-9607-41593ae40f03)

Por último, contarán una rutina con la que cumplirán siempre que no les interrumpa ninguna necesidad básica.

En el mundo existirán puntos para cada satisfacer todas las necesidades de nuestros habitantes.

Las características del desarrollo son:

A.	El pueblo cuenta con una malla de navegación y puntos de interés en ella que sacian las necesidades de los vecinos.

B.	Las necesidades de los vecinos aumentan con el tiempo a distintas velocidades y consideran que tienen que atacarlas cuando superen un límite variable que, en conjunto, hará que emerjan comportamientos y personalidades distintos.

C.	Los vecinos deciden qué acción es más importante y buscan el punto de interés más cercano de ese tipo, van hacia él y al llegar, se sacian a ritmo distinto.

D.	Cada vecino tiene una rutina distinta que ejecutar cuando no tiene necesitades que saciar.

E.	Los vecinos tendrán en cuenta al jugador para pedirle dinero para comprar comida y bebida o divertirse, si así lo deciden basado en sus parámetros de personalidad.

F.	Existen interrupciones basadas en el tiempo: si llueve o la temperatura es muy alta o baja, los vecinos se refugiarán en interiores.


## Desarrollo

Para el desarrollo de el proyecto he escogido Unreal Engine, principalmente porque cuenta con muchas funciones de inteligencia artificial por defecto, y quería investigarlas, y luego, Epic Games regala muchos assets de pago para usar exclusivamente en Unreal Engine que en Unity tendría que descargar de entre los gratuitos, si existen siquiera. He pensado que podría aprovechar estas ventajas, a pesar de que no tengo experiencia previa con él, al contrario que con Unity.

Las características de Unreal Engine que he usado son el Behaviour Tree, los waypoints y el AI Controller.

VisAI es un paquete del Marketplace que trae clases y comportamientos de inteligencia artificial. No sé aún de todo lo que es capaz, pero me ha proporcionado plantillas de comportamientos con variables útiles en la blackboard, funciones para seleccionar objetivos y perseguirlos. Me refiero a que en vez de que mis tareas del Behaviour Tree hereden de BTTaskBlueprintBase (Unreal Engine) heredan de BTTActionBase (VisAI), que a su vez hereda de BTTaskBlueprintBase. Es decir, son lo mismo, pero con funcionalidad adicional. Lo mismo se aplica a los VisAIWaypoint, en vez de Target Point; y VisAIController, en vez de AIController. En concreto, he utilizado el blackboard VisAI que tiene referencias a los targets. Yo coloco en estas variables el objetivo seleccionado y puedo usar sus funciones para ir al target.

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/6e4d9055-232a-44b7-84c9-c41a8c48ebde)

Por ejemplo, en la siguiente captura vemos que si un vecino tiene hambre, va a buscar el punto de comida más cercano (más adelante tendrá más cosas en cuenta), y se va a dirigir a él con la tareas de VisAI BBT_AIMoveToWaypoint, y cuando llegue, comerá, y su hambre disminuirá.

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/6f47b59c-7a75-4cda-8b77-e02234529407)

Con esto quiero separar bien lo qué parte he hecho yo y cuál es de VisAI. Quiero resaltar que el trabajo que he cogido de VisAI me ha ahorrado colocar ciertas referencias, elegir objetivos e ir a ellos, pero no es tampoco nada que no hubiera podido hacer sin él. Me ha servido como ejemplo para ver cómo funciona esta herramienta y poder entenderla rápidamente. Con esto explicado, el resto de funcionalidades que explicaré están integradas en Unreal Engine por defecto.

### Característica A

El objeto NavMeshBoundsVolume calcula automáticamente la malla de navegación del mapa dentro del cubo. Está colocado de manera que se deje el agua fuera del volumen navegable. En el mapa he colocado puntos de interés que satisfacen una de las necesidades de los aldeanos. Cada punto de interés tiene la siguiente información:

- Tipo: Característica que sacia. Puede ser un punto para dormir, comer, beber, ducharse, ir al baño o divertirse
- Ocupado: Un punto solo puede usarlo un aldeano a la vez. Si el punto está ocupado, otro vecino no lo podrá elegir hasta que el primero termine.
- Coste: Algunos puntos cuestan dinero, por ejemplo, la diversión de las tiendas, la comida del restaurante, la bebida de las máquinas… solo podrán usarlas los aldeanos que tengan el dinero suficiente y crean que es conveniente gastarlo basado en su distancia a otros puntos gratuitos y su variable “Apego al dinero”
- Lugar: interior o exterior. Se usa para que los vecinos puedan refugiarse en caso de tiempo adverso.
- Gusto: Qué es el punto. Lo uso para ajustar los gustos y disgustos de cada aldeano en sus listas, y así definir su rutina. Los gustos que hay son comida sana, comida rápida, museo, gimnasio, lago, playa, tiendas, granja, camping y, para los que no encajen en ninguna, neutral.

Se ven de esta forma:

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/5348c072-37cb-4fc0-8b1a-597d69b8a8e7)

### Característica B

Cada vecino hereda del blueprint (una clase) BP_Villager. Cuenta con las variables mencionadas en el diseño además de las referencias necesarias para mostrar los datos por la interfaz y conocer la hora del día y tiempo.

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/5ba2853e-d198-4724-93b3-03d4e9f2383b)

Cada frame, cada vecino actualiza sus variables, incrementándolas en una medida distinta para cada necesidad, y dependiendo de si está durmiendo o no. Después, cada vecino comprueba si tiene que ganar dinero. Ganan su sueldo cada día. Finalmente, se actualiza la interfaz. Cada necesidad es una barra de progreso, y como uso valores en el rango \[0, 1], el valor se establece directamente. La función UpdateProgressBar además, elige si poner el color por defecto o el rojo, en caso de que el valor supere el límite que ese vecino tiene y marcando así que ese vecino va a priorizar saciar esa necesidad. Los blueprints son:

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/b8e7996c-b1a1-4c15-820f-b611d5377513)
![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/5a0c3c76-e86c-46bc-b8cf-afb6f39a8d9e)
![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/0d704e7a-7963-45d8-8624-67a667d9a7a1)
![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/937621ad-12df-41b1-ab78-ff291e196fe5)
![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/9cf55f95-cde8-4e46-95bf-dec570344707)

### Característica C

Para decidir qué hacer, utilizo un árbol de comportamiento. Para asignar un árbol de comportamiento a un “Pawn” (un personaje) debemos crear un AIController y asignárselo al blueprint.

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/e42e1d6c-4403-4300-86c4-9b60ed1a6d55)

Mi AIController se llama AI_Villager hereda de VisAIController y es el que corre el árbol de comportamiento.

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/41a26901-23d3-4773-bb23-af4b9b4a8a2a)

El árbol de comportamiento usa tareas (tasks) y condicionales (decorators) de la misma forma que el de Unity, con la diferencia fundamental de los [árboles de comportamiento de Unreal](https://docs.unrealengine.com/5.0/en-US/behavior-tree-in-unreal-engine---overview/) son dirigidos por eventos, lo que significa que no se ejecutan cada frame activamente, sino que sólo se actualiza cuando alguna variable cambia, lo que aumenta el rendimiento bastante.

Aunque no se vea nada, el árbol de comportamiento completo luce así. Así, podremos situar las capturas en detalle en el flujo global.

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/8fcaebb2-2028-46f5-a8b5-1259b89bc0a7)

Para saber qué tarea atacar primero he ordenado condicionales en el árbol por prioridad: primero ir al baño, luego comer, beber, dormir, ducharse y por último divertirse; de forma que, si una nueva necesidad más prioritaria surge, se interrumpen las menos prioritarias.

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/d6c64548-c71c-409c-b2af-b7b269f2cc37)

Las condicionales son decorators que comprueban si el valor de la necesidad supera su límite. Por ejemplo, este es el BTD_Drowzy. El resto de decorators son equivalentes.

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/4128505d-f81c-404a-a18d-efe7b3212796)

Para atacar esa tarea, los vecinos tienen que buscar el punto de interés más conveniente, ir hacia él y saciar su necesidad. Por ejemplo, estas son las tareas que se ejecutarán cuando el vecino tenga sueño.

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/93c83b98-1530-4e85-8fae-e046ba4bead8)

En más profundidad, la tarea BTT_FindBestMapPoint recorre los puntos de interés del escenario y, si está interesado en un punto, lo compara con los que ya ha encontrado, para guardar el más cercano y el gratuito más cercano. Al terminar, escoge entre el más cercano y el gratuito más cercano dependiendo de la distancia que los separe y su “apego al dinero”.

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/91589ed3-9d55-4360-b38b-833feb4566c9)
![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/0d80fe03-f998-47ba-8fb5-a2a7f8a7bfd1)

Para saber si está interesado en un waypoint, tiene que cumplirse que sea del tipo deseado, que no esté ocupado, que pueda pagarlo y que sea el de menor coste, siendo el coste la distancia, pero modificada para beneficiar a los gustos, perjudicar los disgustos, y preferencia de interior y exterior según el parámetro “extroversión”.

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/ecb5259c-b178-455b-a9fd-c97615a08279)

La tarea BTT_Bathe reduce la suciedad con el tiempo. Cuando se reduce a 0 o se aborta, deja libre el waypoint y cobra su coste. Otras tareas para saciar necesidades son equivalentes.

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/e08b8da3-5723-4c2e-8c4b-655aa54d865e)
![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/b87c9b8a-85df-441c-b754-6364caf4974c)

### Característica D

Para crear una rutina, es decir, una serie de acciones que los vecinos realizan cuando no tienen ninguna necesidad que saciar, he escogido darles “gustos”. Cuando se queden libres, elegirán uno de sus gustos al azar, buscarán el punto de ese gusto más cercano y se pasearán a él, sin tener en cuenta si está ocupado porque no van a saciar ninguna necesidad. Al llegar, esperarán un tiempo antes de elegir otro punto. Todos los vecinos tienen al menos 2 gustos para que se puedan pasear entre ellos.
En el árbol de comportamiento, esta parte se ve así:

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/b3c27216-509f-4296-9085-0bbd036a1e63)

BTT_FindRoutineMapPoint es una variación de BTT_FindBestMapPoint en la que en vez del tipo (dormir, comer…) se buscan waypoints uno de gustos (comida sana, tiendas, museo…) del aldeano aleatoriamente (si no tiene gustos, se escoge cualquier gusto) y no se tiene en cuenta ni el precio ni si está ocupado o no.

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/65779d25-f4c9-49e0-8a74-21d8bb4be6b0)
![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/4211e646-2d8c-4aff-9b4b-b0a25abc1d55)

### Característica E

La interacción de los vecinos con el jugador tiene 2 beneficios para ellos: el jugador puede darles el dinero que se compren comida y bebida y se divierten hablando con él. La distancia a la que buscan al jugador, y la cantidad de aburrimiento que sacian hablando con él depende de su parámetro “extroversión”, siendo 0 evitar al jugador y no divertirte nada con él, y 1 buscarlo en el rango máximo y saciar toda la barra de aburrimiento al hablar con él. Podemos notarlo en la implementación del decoratorn BTD_PlayerAround.

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/7cdb76e7-66c0-4567-a57a-06c7e18e4fff)

Para la implementación se ha modificado el árbol de comportamiento. Al tener hambre o sed, los vecinos comprueban si el jugador está cerca como he explicado en el párrafo anterior. En caso afirmativo, se dirigirán hacia él y recibirán el dinero correspondiente lo que cueste el waypoint más cercano, de manera que les sale gratis. Este dinero puede ser 0, pero, aun así, sacian de paso su aburrimiento, por lo que siempre es beneficioso para ellos buscarlo. Al aburrirse, si el jugador está cerca, en vez de ir a jugar irán a hablar con él hasta que se sacie su aburrimiento. Aquí uso el nodo simple parallel, que me permite continuar siguiendo al jugador mientras el vecino no haya terminado de hablar con el jugador.

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/cf70ff16-8e1c-491d-a0b9-6a0c3c12c99d)
![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/0bea6d9c-654c-4463-a69e-e2cab3b1efff)

### Característica F

El tiempo influye en el comportamiento de los vecinos, interrumpiéndolos por completo. Los vecinos tienen distintas reacciones frente al tiempo. Les puede importar que llueva o no, y sentirán frío o calor con distintas temperaturas. En caso de que el tiempo les interrumpa, un icono se mostrará en su parte de la interfaz para mostrarlo. He hecho que el juego comience con la fecha del ordenador, por lo que he balanceado los límites a la variación de temperaturas actuales (mayo-junio) para ver las reacciones al frío y al calor ahora, aunque no haga mucho frío.
Si al vecino le importa la lluvia y llueve, se refugiará, buscando el punto de interés interior más cercano. Si el vecino tiene frío, también. Si el vecino tiene calor, irá a la playa.
Con la estructura de waypoints, no puedo saber si el camino entre 2 interiores pasa por el exterior, por lo que es posible que los vecinos refugiados cambien de edificio para saciar alguna necesidad que no está disponible en su edificio. En caso de que quieran usar un punto exterior, se quedarán esperando a que termine de llover o haga más calor.
Para implementar esta funcionalidad en el árbol de comportamiento se han añadido estos nodos como los más prioritarios.

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/bec44625-6573-4140-bba8-a29448106315)

Estos decorators son distintos a los demás. Se llaman composite decorators y permiten hacer operaciones lógicas con otros decorators simples mediante un grafo. Los títulos que he puesto explican que hace cada uno, y la descripción explica que operaciones hace. Por ejemplo, el grafo de “¿Estoy fuera y está lloviendo o hace frío?” luce así:

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/4cd54afa-2fa0-42d6-a189-471f55448572)

Las tareas BTT_FindClosestLocation y BTT_FindClosestTaste son también variaciones de BTT_FindBestMapPoint, pero buscando sólo el mas cercano, sin tener en cuenta costes o si está ocupado, y buscando por lugar (interior o exterior) o gusto.

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/f3526981-f867-481e-be7d-fb983b5e8fc2)
![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/c443969f-fcf4-4abe-9c1c-a734c962bf63)
![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/a858dc3d-a9ad-46d1-b3f1-86d74c6223df)

## Resultados

Los resultados en general son satisfactorios, el comportamiento es el esperado. Los vecinos sacian sus necesidades correctamente y ejecutan su rutina sin problema. El balanceo no es realista, pero lo he ajustado a lo que necesito para probar todas las características.

He encontrado algunos “bloqueos”. Cuando llueve y quieres ir a un objetivo exterior, como estás poniendo el target al exterior, y la lluvia es lo que mayor prioridad tiene, se pisa constantemente en todos los frames. El resultado es bueno, el aldeano espera en el interior, simplemente peta la consola de mensajes porque oscila muy rápido. Otro es que haga calor y llueva a la vez, de nuevo se pisa constantemente y el aldeano ha decidido quedarse en la playa. Podemos comprarlo como correcto, si el pobre tiene calor. El último ha sido uno que no esperaba. He visto que la tarea BTT_FindBestMapPoint puede fallar, y he encontrado que en esa ocasión todos los vecinos tenían sed. Como marco los puntos como ocupados desde que el aldeano decide dirigirse a ellos, los puntos se ocupan por bastante rato, lo que tarden en llegar. Es decir, es posible que todos los puntos de sed se encuentren ocupados a la vez. El resultado es que el aldeano que se bloquea se queda quieto intentando encontrar una bebida cada frame porque es lo que quiere.

Al final no es que sean bugs, pero estos 3 bloqueos han llenado la pantalla de mensajes y he tenido que optar por imprimir menos información, ya que tampoco son incorrectos, los pobres no pueden hacer otra cosa.

Por otra parte, la navegación de Unreal no tiene en cuenta el resto de los agentes y su esquive parece escaso. Parece que se puede solucionar con un crowd manager, pero no me ha funcionado.

El rendimiento no es muy bueno, no llega a los 25 FPS en mi portátil, pero esto es mayormente por los gráficos de alta calidad y sospecho que por las olas del mar. Por suerte podemos ver el tiempo que consume el árbol de comportamiento, que es algo mas de 6 ms cada frame. Esto a mí no me dice mucho, porque tampoco esque tenga experiencia optimizando, pero creo que es bastante si se quisiera para un juego de verdad. Como este es mi primer contacto con Unreal, quizá el hacer los finds para todos los waypoints en cada condicional es demasiada carga, y se podría hacer de otra forma. Pero para un primer intento lo veo asumible.

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/73706bd0-f321-427b-8c26-2c1c0a12393f)

## Conclusion

La sensación que me dan estos vecinos es más robótica, pero esto es debido a que he hecho que el tiempo avance muy deprisa. En Animal Crossing, el tiempo avanza como al ritmo real. Con esto en cuenta, sí que creo que agrega una capa de profundidad a los vecinos, que tienen preferencias, y eventualmente tienen que ausentarse de sus puestos de trabajo para ir al baño o comer. También vemos que tienen una vida fuera, ya que, en el juego original, los dependientes de las tiendas se quedan dentro cuando cierran y nunca se los ve ir a su casa, viven en su tienda.

En cuanto mi opinión sobre la viabilidad de este sistema en un videojuego real, creo que perdería frente a un sistema de horarios predefinidos debido a la incertidumbre, y puede que también por el rendimiento. Sin embargo, no me parecería descabellado porque los resultados son buenos y hemos visto que los árboles de comportamiento de Unreal consumen pocos recursos. Si tuviera que añadir una mejora, sería una pequeña previsión a futuro para evitar que, tras unos segundos de ir a dormir, por ejemplo, tengas que dar media vuelta porque tienes hambre.

Para terminar, haber escogido Unreal Engine ha producido un resultado muy vistoso gracias a su motor de renderizado y los assets de pago, por lo que voy a mostrar algunas capturas.

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/82097447-d6c5-4ca9-987a-7727606ec863)
![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/f2b8a82b-7d89-4642-8f63-85f2100ae2da)
![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/b61fa6b7-e81c-4752-893e-a6880e0dc710)
![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/3e29d547-3772-4705-9d3a-c9502cb63f5b)
![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/5c671b3a-d39b-4b42-a8ea-bad863ee8375)
![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/f82d21ed-cb13-47e3-bc44-37434b450527)


## Contenido de terceros

![image](https://github.com/javics2002/IAV23-CanoSalcedo/assets/49459590/75bd0d16-5a0c-4372-8a47-01fbc7e7e85f)
- [VisAI – Community – Modern AI Framework](https://www.unrealengine.com/marketplace/en-US/product/visai-an-advanced-modular-ai-system)
- [Dynamic Volumetric Sky](https://www.unrealengine.com/marketplace/en-US/product/dynamic-volumetric-sky?sessionInvalidated=true)
- [Bank Building / Interior (Modular)](https://www.unrealengine.com/marketplace/en-US/product/bank-building-interior)
- [Big Office](https://www.unrealengine.com/marketplace/en-US/product/big-office)
- [Triplex House Villa](https://www.unrealengine.com/marketplace/en-US/product/big-triplex-house-villa)
- [Cartoon Water Shader](https://www.unrealengine.com/marketplace/en-US/product/cartoon-water-shader)
- [Clothing and Shoe Stores](https://www.unrealengine.com/marketplace/en-US/product/clothing-and-shoe-stores)
- [Greenwood Fantasy Village](https://www.unrealengine.com/marketplace/en-US/product/resubmission-for-release-greenwood-fantasy-village)
- [Hospitality Pack](https://www.unrealengine.com/marketplace/en-US/product/hospitality-pack)
- [Loft Office (Modular)](https://www.unrealengine.com/marketplace/en-US/item/ca380075a5c04081aa3f4fe69a732fd1)
- [The Model Resource: Animal Crossing: New Horizons](https://www.models-resource.com/nintendo_switch/animalcrossingnewhorizons/)
- [Animal Crossing: New Horizons Models & Textures](https://www.reddit.com/r/ac_newhorizons/comments/qtmv3x/all_models_textures_ripped_for_acnh_200_daepng/)
- [Quixel Bridge](https://quixel.com/)
- [Mixamo](https://www.mixamo.com/#/)
- [PowerPoint](https://www.microsoft.com/es-es/microsoft-365/powerpoint)
