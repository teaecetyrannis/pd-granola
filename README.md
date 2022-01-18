# granola
[read in english](https://github.com/teaecetyrannis/pd-granola/blob/main/README_EN.md)
<br><br>
sintetizador granular estocástico desarrollado en [pure data](https://github.com/pure-data/pure-data)


## instalación
descargar el archivo granola.zip de la [última release](https://github.com/teaecetyrannis/pd-granola/releases), extraer y agregar la carpeta contenedora al path de pure data, luego se puede iniciar desde cualquier parche creando el objeto `[granola~]`
<br><br>también depende de la abstracción [adsr](https://github.com/teaecetyrannis/pd-adsr) y el objeto `[selector~]` de la librería [cyclone](https://github.com/porres/pd-cyclone), por lo que necesariamente deberán instalarse también


## funcionamiento
el sintetizador cuenta con:

1. grain
	- **durat.**: control de duración del grano (*ms* o *samps*)
	- **autotrigger**: activa el encendido automático de los granos en base al rate, si se desactiva sólo será posible encenderlos recibiendo un `[bang]` por la segunda inlet
	- **rate**: control de la frecuencia a la que los granos se encienden (*ms* o *samps*)

2. sample
	  - **load**: se utiliza para buscar el archivo de audio (en formato .wav)
	- **posición del cabezal**
	- **play**: si está encendido, desplaza el cabezal a través del sample a la velocidad indicada (en otras palabras, resume o pausa la reproducción del sample)
	- **reset_on_noteon**: al estar **play** encendido, determina si el cabezal será uno solo para todas las voces (apagado) o si cada vez que ataque una nueva nota esta comenzará a reproducir el sample a partir de la **posición del cabezal** de manera independiente a las demás voces (encendido)
	<br>nótese que al estar encendido mover la **posición del cabezal** no tendrá ningún efecto ya que sólo se está determinando en qué punto del sample iniciará la próxima nota, mientras que si está apagado todas las voces comparten un único cabezal que puede ser alterado moviendo la **posición del cabezal**
	- **speed**: determina la velocidad a la que se moverá/n el/los cabezal/es (*%*)
	<br>se puede utilizar el slider que se encuentra arriba, que va de -300 a 300, para un control más intuitivo en caso de estar usando mouse
	- **note**: permite indicar cuál es la "nota" (*midi*) del sample original para que coincida con las notas ingresadas al sintetizador, pero también permite usos más creativos que éste

3. window
	- **shape**: determina la forma de la ventana que es aplicada a cada grano (*straight, cos, tri, tridown, triup, custom*)
	- **load**: se utiliza para buscar el archivo de audio (en formato .wav) que será utilizado en la forma *custom*, la ventana es siempre de 2048 samples de duración y si el sample cargado es mayor se utilizarán sólo los primeros 2048 samples del mismo
	- **smooth**: si se enciende, una segunda ventana es aplicada a cada grano con breves rampas desde y hacia cero al comienzo y al final, esto sirve para evitar los impulsos generados por las formas de ventana que no empiezan y terminan en un mismo valor

4. envelope
	- ataque (*ms*), decay (*ms*), sustain (*%*) y release (*ms*)
	- controles para cambiar entre monofónico y polifónico, voice-**stealing**, **legato** y comportamiento del pitch **bend** (_global_ aplica el bend a todas las voces de la polifonía y _last_ aplica el bend sólo a la última voz activada)

5. offsets
	el sintetizador cuenta con tres paneles que permiten cambiar los valores de tres parámetros: posición de cabezal (*ms* o *samp*), altura (*midi* o *cents* y duración (*ms* o *samp*) de izquierda a derecha
	- **offset**: cuánto se le suma o resta al valor actual de cada parámetro
	- **chance**: la probabilidad (*0-100%*) de que esta resta o suma se cumpla para cada nuevo grano
	- **precision**: define el margen de posibles resultados (*0-100%*), el resultado podrá ser cualquier número (con igual probabilidad) ubicado entre el valor actual + precision% del offset Y el valor actual + 100% del offset
	<br>por ejemplo: si se lleva a 100 el resultado siempre será el valor actual + el valor de **offset** (precisión absoluta), si se lleva a 50 el resultado podrá ser cualquier número entre el valor actual + 50% del **offset** Y el valor actual + el **offset** (todos con igual probabilidad)

CUIDADO
- tener en cuenta que al cargar archivos de audio la abstracción guardará la ruta de dichos archivos y es allí que los buscará la próxima vez que reciba el mensaje de estado, en caso de hacer uso de él
- al trabajar en modo polifónico y sobre todo usando valores muy pequeños de **rate** el uso del cpu puede incrementar bastante con cada voz, técnicamente logra hasta 16 voces pero en la computadora donde desarrollo (i5-4440) el buffer rebalsa antes de ocuparlas todas


## documentación, cambiar los parámetros dinámicamente y guardar el estado actual del sintetizador
en el subparche `[pd help]` dentro de granola~.pd se encuentra (en inglés) toda esta misma información y además se detallan sus inlets y outlets y cómo setear todos los parámetros desde fuera mediante mensajes, permitiendo un control completamente dinámico del sintetizador sin hacer uso de la interfaz
<br>gracias a esto también es posible recordar enteramente el estado del sintetizador: sólo es necesario un muy extenso mensaje que incluya todos los valores actuales para cada parámetro... por suerte esta función ya está incorporada y basta con conectar un mensaje (no es necesario que esté vacío, ya que su contenido será reemplazado) a la tercera outlet y luego enviar un mensaje `[state(` a la segunda inlet, esto escribirá en el mensaje todos los parámetros y rutas de archivos
	

## créditos
[pure data](https://github.com/pure-data/pure-data) por miller puckette y muchxs más
