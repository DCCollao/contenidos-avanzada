# Parte 1 Taller: _Testing_

Para hacer más corta la parte de _testing_ y además aprovechar de revisar cosas relacionadas a la tarea, decidimos hacer un poco de testing sobre la solución subida de la AC07. Primero un intento de testing propio con excepciones y luego con `unittest`.

## Introducción

Primero hablar sobre _testing_, de que trata, y como se visualiza a nivel de código y flujo de desarrollo.

## Intento 1: Manual con excepciones: `test.py`

Aquí la idea es hacer más visual como se vería un módulo para probar código. Este importa algo específico a probar, y llamamos de a poco a sus métodos y revisamos que los efectos esperados se traduzcan. 

Usamos el uso de levantamiento de excepciones para que nuestro programa se detenga completamente cuando revise y encuentre algo innesperado. La idea es que al correr el programa, si termina sin interrupciones, es porque está todo bien. Se encapsulan en los métodos de una clase los distintos chequeos a realizar y en `"__main__"` se realizan las llamadas de métodos.

`back_end.py` lo cambié desde el que originalmente subimos. 

Me di cuenta preparando esto que se caía si no se conectaba efectivamente con front-end. Eran dos las razones: (1) nunca se inicializaban las señales para emitir hacia el front-end si ni se llamaba el método `setear_senales`, tirando errores de `AtributeError`, y (2) aunque se inicialicen a `None`, por ejemplo, se caía al intentar emitir, ya que eran `None`.

Igual lo encontré un problema interesante para mostrar, debido a la necesidad de separar back-end y front-end, pero habían tantas secciones con problemas que hubiera sido una lata arreglarlo en vivo. Así que dejé solo algunos errores. Para arreglar todo, generalicé la emisión de señales a un método `emit` que recibe la señal y argumentos, y solo si la señal está definida, entonces emite.

Los problemas que dejé:
- `self.senal_ganador` no se inicializa como atributo. (se arreglará en el intento 2)
- En `ingresar_nombres`, dejé una de las emisiones como estaban antes, y tirará un error `AttributeError: 'NoneType'`.

Así, la idea es revisar de a poco las cosas que se van revisando:
- Primero se revisa que los atributos iniciales del juego estén bien.
- Se intenta jugar cartas aunque no se haya comenzado a jugar aún.
- Se prueba la revisión de nombres. (aquí aparece el `NoneType` "sin querer"). Aquí se puede probar comentando en `back_end.pY` tal vez alguno de los casos y ver como se levantan errores.
- Luego comenzar el juego, y e intentar tirar unas cartas, revisando que los turnos se respeten.

## Intento 2: Con `unittest`: `test_2.py`

Mostrar que esto ya hay librerías que permiten organizar estas pruebas. Heredando de `TestCase` se crean mediante métodos distintos flujos de pruebas a revisar. Luego se ejecutan las pruebas y un reporte aparece. Recordar:

- Los métodos que son tests deben empezar su nombre con `test` para ser detectado como pruebas.
- `setUp` permite generar datos base comunes para todos los `tests`. Ojo, que es como si se ejecutará y sobre-escriba el estado antes de cada prueba. El orden en que se hacen los test es por orden alfabetico de métodos.
- Mostrar el reporte: cada . es un éxito, E es un error que surge y cada F una falla.

Aquí aparece el error mencionado antes de que no se inicializa `self.senal_ganador`. Este no aparece antes porque nunca se llega a la situación de ganar en el primer archivo. Es cosa de inicializarlo a `None` en el constructor.

