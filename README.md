# Snake.io
Universidad URJC  
Práctica: Juegos En Red
____

### 1. Nombre y breve descripción del Juego:
El juego a presentar es *"Snake.io"* en el cual dos o más jugadores se retarán por reclamar la mayor área del tablero posible en un tiempo determinado. Es un juego de temática similar a *Qix* y *Splix.io*, donde se mezclan la acción y estrategia.

### 2. Reglas/funcionalidades del juego:
  * El tiempo máximo de cada partida es de 2 minutos.
  * 2-4 jugadores.
  * Cada jugador tendrá un color específico que le represente de forma aleatoria.
  * Controles por teclado con movimientos exclusivamente horizontales y verticales.
  * Cuando el jugador abandona su área para aumentarla, aparece una línea tras él que representa su recorrido.
  * Para aumentar el área, el jugador deberá cerrar un recinto sobre ella.
  * El área del jugador es zona segura para él mismo.
  * Un jugador puede "eliminar" a otro si el primero colisiona contra la línea de recorrido del segundo.
  * Si un jugador es "eliminado" se reinicia su área.
  * Existen potenciadores para los jugadores (mayor velocidad, etc).
  * Al final de cada partida se muestra la puntuación (o porcentaje) de cada jugador en función del área que haya conseguido.

### 3. Integrantes:
**Nombre:** Luis Barreto Peralta  
**Correo Universidad:** l.barretop@alumnos.urjc.es  
**Cuenta en GitHub:** lbarretop

**Nombre:** Laura Hernández Román  
**Correo Universidad:** l.hernandezroma@alumnos.urjc.es  
**Cuenta en GitHub:** Laura-HR 

/////////////////////////
### Estructura del juego
El juego tiene 4 estados:
- Menú de inicio. 
- Menú donde se esperan a los jugadores (será más importante cuando se implemente el multijugador en diferentes ordenadores). 
- Juego. 
- Menú de puntuaciones. (desde el menú de puntuaciones se puede ir al menú de inicio or al menú donde se esperan a los jugadores).

Ahora, lo necesario a implementar es:
- Varios jugadores: 
 - Capturan territorio (sale del territorio propio, se hace una figura delimitada por la línea del jugador y se vuelve al territorio. Se complete lo que está dentro de la figura). 
 - Tienen que desplazarse con teclado (horizontal y vertical). 
 - Pueden "matarse entre ellos (cuando chocan contra una línbe de jugador or contra el borde del tablero). 
 - Cuando un jugador "muere" se borra todo su área. 
 - Puntos (porcentaje de captura). 
 
- Tablero:
 - Representan territorio de jugadores. 
 - Representan la línea de captura de los jugadores. 
 - Representa los bordes.

- Fin del juego
 - Tiempo límite (2 minutos). 
 
Ahora este representado en clases y métodos que el juego tiene que tener:
```
Clase- Tablero. 
Array de casillas de: 
- Array de int de territorio/casillas ✓
-1 == borde. 
0 == en blanco, casilla de nadie. 
1 == jugador 1. 
2 == jugador 2.  

- Array de int de linea ✓
0 == de nadie. 
1 == captura de jugador 1. 
2 == captura de jugador 2.  

Métodos de clase Tablero: 
Método de pintar casilla(numJug, x, y){ //numJug (nº de jug), x (pos x de la casilla a pintar), y (pos y de la casilla a pintar) ✓  
 el color depende del array de territorio.   
}  

Método de inicializar Partida{ ✓
   Ejecutar antes de empezar la partida.  
   Se pone todo a 0 (en blanco) y -1 (los bordes del tablero) por defecto.  
}  

Función pintar alrededor(x, y, numJug){ ✓
   Cambiar valor array territorio/casilla a numJug alreadedor de x e y.  
   tiene que quedar un cuadrado de 3x3.  
}  

Función borrarDeUnJugador(numJug){ ✓
   Poner 0 todo territorio y linea de numJug.  
}  

Función contar casillas(numJug){
   Recorrer todo el array de casillas y contar las casillas que pertenecen a un jugador. Devuelve contador de casillas.  
} 

Función pintar línea(numJug, x, y){ ✓
   El color depende del array linea.  
} 

Clase Jugador- Se creará un array juagdores.
Posición x. 
Posición y. 
Puntuación.  
Flag hay que pintar.  

Métodos de la clase jugador:  
Función mover jugador(tecla){ ✓
   En función de la tecla se aumenta o disminuye la posición del jugador correspondiente.  
}
//////////PROBLEMAS SOBRE MOVER JUGADOR:
//////////NO SE PERMITE EL MOVIMIENTO SIMULTÁNEO DE JUGADORES
///////// LOS CONTROLES NO FUNCIONAN PARA TODOS LOS NAVEGADORES (funcionan bien en Firefox)

Función colisión(numJug, x, y){ ✓
   Comprobar que el jugador no está en casilla -1. Si sí, el jugador "muere". Llamar a función Reiniciar jugador(de este jugador).  
   Comprobar que el jugador no está en línea(cualquier valor). Si sí, reiniciar el jugador del cual ha chocado.  
   ej:  
   colision(numJug, x, y){  
     if(array_casillas[x][y]== -1){  
     reiniciarJugador(n);  
     }  
     if(array_linea[x][y]!=0){  
       reiniciarJugador(array_linea[x][y]);  
     }  
   }
}  

Función inicializar Jugador(numJug, x, y){ ✓
   Poner jugador en x, y.  
   Llamar a pintar alrededor de tablero.  
}  

Función ReiniciarJugador(numJug){ ✓
   Cuando "mueres".  
   Reiniciar flag.  
   Calcular aleatoriamente una x e y (que no esté al lado del borde ni sobre un jugador).    
   Llamar a borrarDeUnJugador de la clase tablero.  
   Llamar a inicializar jugador con los valores x e y.  
   Llamar a pintar alrededor de la clase tablero.  
}  

Función updatePuntos(){  
   Llamar a la función contar casillas de la clase tablero.  
   Calcular el porcentaje.  
}  

Función Capturar(n){ //SOLO BOUNDING BOX
   Calcular BoundingBox (encontrar los extremos de línea y territorio) y ampliar 1.  
   Encontrar lo de fuera.  
   Pintar lo de dentro.  
   Borrar la línea.  
}  

Función comprobar si se está fuera y rellenar nueva área(){ ✓
 Detectar que no está el jugador en el territorio. Empezar a pintar línea.  
 Si estoy fuera  
  if(territorio != jugador){  
    flag a true.  
    pintar linea[x][y] a jugador  
  }  
  else{  
     if(flag == true){  
       llamar a función Capturar  
       flag a false.//Para que no calcule el capturar todo el rato cuando que el jugador se encuentra en su territorio.  
     }  
   } 
}  

Bucle de control{ 
   Borrar todo las gráficas.  
   Pintar las gráfica nuevas.  
   Comprobar el tiempo.  
   Comprobar el estado de los jugadores (colisiones, el método de comprobar si el jugador está fuera de su territorio, actualizar las puntuaciones).  
   Comprobar inputs (ej: movimiento de jugadores).  
}  
```
