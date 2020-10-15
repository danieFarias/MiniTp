# Sistemas Operativos y Redes

## Trabajo Práctico de hilos y semáforos
obstaculos encontrados
1)utilizacion para descarga y cargar archihvos en github
2)sincronizacion de los semaforos en sus dos tipos init y mutex
3)leer desde un txt y escribir la salilda por pantalla en un txt
4)Imprimir por pantalla al equipos ganador y escribirlo en el txt

1)tanto para a finalizacion y entrega del tp, entiendase como el subir los archivos txt y escribir el informa, tuve problemas al desconocer el uso de la herramienta git, tuve que escribir el informe dos veces ya que si no se guardan los cambios despues de un tiempo se borra lo escrito.Similares problemas basicos tuve cuanod quise bajar el archivo luego de loogearme, estuve un tiempo tratando de copiar el codigo del tp que baje por winzip, sin embargo no sabia que debia confirma mi correo electronico.Dichos errores basicos, los cometi por el desconcimiento sobre la herreamienta git y la falta de tiempo.
2)Utilice dos tipos de semaforos unit y mutex, tuve problemas con la sintaxis de mutex y donde declararlos, ya que se habia sugerido en el video de la clase que dichos semaforos debian considerarselos como globales, los declare abajo de las librerias, juntos con otras varibles que considere debian ser globales para la resolucion del tp.
Utilce los semaforos init para sincronizar las tareas y los mutex para proteger las zonas criticas, en dos casos como fueron salar() y parrilla(), se utilizo la combinacion de ambos semaforos.
esquema del pseudo codigo
                  

                                   p(s_salar)                                           p(s_parrilla)
                p(s_mezclar)       mutex_lock(&salar)       p(s_armar)                  mutex_lock(&parrilla)       mutex_lock(&hornear)
cortar          mezclar()          salar()                  armar()                     cocinar()                   hornear()
v(s_mezclar)    v(s_salar)         mutex_unlock(&salar)     v(s_parrilla)               v(s_cocinarT)               v(s_hornearT)
                                   v(s_armar)                                           mutex_unlock(&parrilla)     mutex_unlock(&hornear)
                                   
                            p(s_cortarLyTT)       
                            p(s_hornearT)
                            p(s_cocinarT)
                            mutex_lock(&ganador)
 cortarLyT()               armarHam()
 v(s_cortarLyTT)            mutex_unlock(&ganador)
 
 
 dentro de todas las posibilidades de secuencias:
 salar -LyT(cortar lechuga y tomate)
       -parrilla
       -hornear
  LyT  -salar
       -hornear
  hornear -LyT
          -salar
  y las bifurcaciones de los terceros y cuarto pasos, se ejemplifica la secuencia salar-parrilla-hornear-LyT y LyT-salar-parrilla-hornear.Cabe aclarar que la hamburguesa queda lista cuando los semaforos,s_cocinarT,s_hornearT,s_cortarLyTT estan en 1
  
  secuencia:salar-parrilla-hornear-LyT
                  a b c d e f g h i j k l
  s_mezclar       0 1 0 0 0 0 0 0 0 0 0 0     
  s_salar         0 0 1 0 0 0 0 0 0 0 0 0
  s_armar         0 0 0 0 1 0 0 0 0 0 0 0
  s_parrilla      0 0 0 0 0 1 0 0 0 0 0 0 
  s_cocinarT      0 0 0 0 0 0 0 1 1 1 1 1
  s_hornearT      0 0 0 0 0 0 0 0 0 0 1 1
  s_cortarLyTT    0 0 0 0 0 0 0 0 0 0 0 1
  mutex_parrilla  - - - - - - b d d d d d 
  mutex_hornear   - - - - - - - - b d d d
  mutex_salar     - - - b d d d d d d d d
  
  
    secuencia:LyT-salar-parrilla-hornear
                  a b c d e f g h i j 
  s_mezclar       0 1 0 0 0 0 0 0 0 0     
  s_salar         0 0 0 1 0 0 0 0 0 0 
  s_armar         0 0 0 0 0 1 0 0 0 0 
  s_parrilla      0 0 0 0 0 0 1 0 0 0  
  s_cocinarT      0 0 0 0 0 0 0 1 1 1 
  s_hornearT      0 0 0 0 0 0 0 0 0 1 
  s_cortarLyTT    1 1 1 1 1 1 1 1 1 1 
  mutex_parrilla  - - - - - - b d d d  
  mutex_hornear   - - - - - - - - b d 
  mutex_salar     - - - b d d d d d d 
  
  
  
  3)La funcion leer fue lo que mas costo de todo el tp, si bien muchas cosas me costaron como entender struct, y poer aunque sea saber que es, no utilizarlo pero por lo menos pensarlo. Volviendo al tema de la funcion leer fue un verdadero desafio ya que habia elegido la funcion fgets para leer las lineas de la receta para luego pasarle a la funcion stortok(char *cadena,delimitaro)*, pasarle un cadena que tenia que contener un buffer que contenga la direccion de memoria donde se encontraba la linea de texto que queria leer sumado a un delimitador que por consejo de los profesores iba a ser "|". No pude lograr leer un archivo y combinarlo con el codigo del tp como pense desde un principio, entonces busque otra funcion , la funcion getline, ya que mi manejo de los punteros y direccion de memoria no son los mejores. La funcion getlilne hace un lectura de lineas, almacenando el texto en un buffer y almacena la direccion del buffer en un char , en el caso de mi codigo palabra. Con respecto al buffer, sino es lo suficientemente grande como para que entre una linea, getline hace el buffer mas grande usando realloc actualizando la nueva direccion, *lineptr(*) y el tamaño *n(*). inicializando lineptr a NULL y n a cero getline lo hace todo.La condicion de while !=-1 es por el final del fichero.Las fuentes utilizadas para esta funcion fueron, riptutorial.com, www.it.uc3.m.
  Se habia aclarado en clase que se trataba de un ciclo anidado y que aquellas palabras hardcodeadas debian levantarse desde el archivo, lo que mas costo fue encontrar la funcion adecuada que trabajara con punteros,lo del codigo anidado, fue cuestion de paciencia e interpretar a los printf puestos de ayuda para la visualizacion.
   Por otro lado con fprintf(FILE *stream(*), va sin asteriscos en el parentesis,const char *format(*), lo mismo sin asteriso) pude tomar lo que se imprimia en la funcion imprimir accion , los printf, y copiar su contenido tal cual estaba, de esa forma se escribieron en el archivo txt de salid.Una aclaracion importante es que la forma en que se copia el txt de salida coloca a la ultima accion al principio del txt asi como esta en el codigo.Quise probar la idea de cambiar la struct de parametros para que a hilo principal en la funcion ejecutar receta, la principal pudiera, en dicho struct pasarle un archivo un FIlE, pero no pude lograrlo, asi que elegi dicha formar comoestas codificada, cree un archivo en el lugar donde declare mis varibales globales y cerre dicho archivo en el main.
   4)Fue lo ultimo que logre hacer, el problema principla que tuve es que cuando los string "corta" y demas acciones estaban hardcodeadas, solo con colocar un boolean en mi ultima funcion armarHam podia imprimir por pantalla el equipo que gano. Pero una vez que pude leer la receta , esta opcion ya no estaba, de manera tal que cree una variable de tipo entero y guarde el numero del equip que llego primero a la ultima funcion armarHam.La variable para guardar el numero del equipo es Equipo y el numero del equipo viene dato por mydata->Equipo_param.Seguido a guardar el numero del equipo, lo imprimi por pantalla, a continuacion abri el archivo ak que es donde escribo las salidas y con fprintf, escribo en el txt Salida.
  
