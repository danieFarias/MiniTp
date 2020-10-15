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
  
  
  
  3)La funcion leer fue lo que mas costo de todo el tp, si bien muchas cosas me costaron como entender struct, y poer aunque sea saber que es, no utilizarlo pero por lo menos pensarlo. Volviendo al tema de la funcion leer fue un verdadero desafio ya que habia elegido la funcion fgets para leer las lineas de la receta para luego pasarle a la funcion stortok(char *cadena,delimitaro)*, pasarle un cadena que tenia que contener un buffer que contenga la direccion de memoria donde se encontraba la linea de texto que queria leer sumado a un delimitador que por consejo de los profesores iba a ser "|". No pude lograr leer un archivo y combinarlo con el codigo del tp como pense desde un principio, entonces busque otra funcion , la funcion getline, ya que mi manejo de los punteros y direccion de memoria no son los mejores. La funcion getlilne hace un lectura de lineas, almacenando el texto en un buffer y almacena la direccion del buffer
