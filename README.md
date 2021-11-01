# granola
[read in english](https://github.com/teaecetyrannis/granola/blob/main/README_EN.md)
<br><br>
sintetizador granular estocástico desarrollado en [pure data](https://github.com/pure-data/pure-data) y [camomile](https://github.com/pierreguillot/Camomile)


## instalación
descargar el archivo granola.zip de la [última release](https://github.com/teaecetyrannis/granola/releases/tag/v1.0) y extraerlo en la carpeta donde se encuentren los plugins vst3 (para correrlo en pd basta con descargar el source code y ejecutar el parche granola.pd)

## funcionamiento
el sintetizador cuenta con:

1. grain
	- **durat.**: control de duración del grano (*ms* o *samps*)
	- **rate**: control de la frecuencia a la que los granos se encienden (*ms*, *samps* o **sync**)
	<br>si se activa **sync**, los granos se encenderán sólo cuando el DAW o host esté reproduciendo y su frecuencia/ritmo se determinará por la razón de los dos números a la derecha de *sync* en relación a la figura de negra (por ejemplo: 1/1 equivale a una negra, 2/1 a una blanca, 1/3 a un tresillo de corchea) respondiendo con buena precisión a cambios de tempo

2. sample
	  - **load**: se utiliza para buscar el archivo de audio (en formato .wav)
	- **posición del cabezal**
	- **play**: si está encendido, desplaza el cabezal a través del sample a la velocidad indicada (en otras palabras, resume o pausa la reproducción del sample)
	- **reset_on_noteon**: al estar **play** encendido, determina si el cabezal será uno solo para todas las voces (apagado) o si cada vez que ataque una nueva nota esta comenzará a reproducir el sample a partir de la **posición del cabezal** de manera independiente a las demás voces (encendido)
	<br>nótese que al estar encendido mover la **posición del cabezal** no tendrá ningún efecto ya que sólo se está determinando en qué punto del sample iniciará la próxima nota, mientras que si está apagado todas las voces comparten un único cabezal que puede ser alterado moviendo la **posición del cabezal**
	- **mono/stereo**: determina la cantidad de canales del sample
	si el sample es stereo existe la libertad de elegir mono, lo que duplica el canal izquierdo del sample
	si el sample es mono, se debe elegir la opción mono a menos que se quiera silencio en el canal derecho
	- **speed**: determina la velocidad a la que se moverá/n el/los cabezal/es (*%*)
	<br>se puede utilizar el slider que se encuentra arriba, que va de -300 a 300, para un control más intuitivo en caso de estar usando mouse
	- **origin_note**: permite indicar cuál es la "nota" (*midi*) del sample original para que coincida con las notas ingresadas al sintetizador, pero también permite usos más creativos que éste

3. window
	- **forma**: determina la forma de la ventana que es aplicada a cada grano (*straight, cos, tri, tridown, triup, custom*)
	- **load**: se utiliza para buscar el archivo de audio (en formato .wav) que será utilizado en la forma *custom*, la ventana es siempre de 2048 samples de duración y si el sample cargado es mayor se utilizarán sólo los primeros 2048 samples del mismo
	- **smooth**: si se enciende, una segunda ventana es aplicada a cada grano con breves rampas desde y hacia cero al comienzo y al final, esto sirve para evitar los impulsos generados por las formas de ventana que no empiezan y terminan en un mismo valor

4. envelope
	- ataque (*ms*), decay (*ms*), sustain (*%*) y release (*ms*)
	- controles para cambiar entre monofónico y polifónico, **legato**, voice-**stealing** y comportamiento del pitch **bend** (_global_ aplica el bend a todas las voces de la polifonía y _last_ aplica el bend sólo a la última voz activada)

5. offsets
	el sintetizador cuenta con tres paneles que permiten cambiar los valores de tres parámetros: posición de cabezal (*ms* o *samp*), altura (*midi* o *cents* y duración (*ms* o *samp*) de izquierda a derecha
	- **offset**: cuánto se le suma o resta al valor actual de cada parámetro
	- **chance**: la probabilidad (*0-100%*) de que esta resta o suma se cumpla para cada nuevo grano
	- **precision**: define el margen de posibles resultados (*0-100%*), el resultado podrá ser cualquier número (con igual probabilidad) ubicado entre el valor actual + precision% del offset Y el valor actual + 100% del offset
	<br>por ejemplo: si se lleva a 100 el resultado siempre será el valor actual + el valor de **offset**, si se lleva a 50 el resultado podrá ser cualquier número entre el valor actual + 50% del **offset** Y el valor actual + el **offset** (todos con igual probabilidad)

CUIDADO
- tener en cuenta que al cargar archivos de audio el plugin recordará el directorio de dichos archivos y es allí que los buscará la próxima vez que el proyecto/preset sea cargado
- al trabajar en modo polifónico y sobre todo usando valores muy pequeños de **rate** el uso del cpu puede incrementar bastante con cada voz, técnicamente logra hasta 16 voces pero en la computadora donde desarrollo (i5-4440) el buffer rebalsa antes de ocuparlas todas
- tener en cuenta que hay una serie de objetos [send] que no llevan la variable $0 al comienzo de su nombre, de cualquier manera difícilmente sea una complicación ya que para evitar posibles cruzamientos con otros parches estos objetos llevan 'dMab-' al comienzo de su nombre
	

## créditos
[pure data](https://github.com/pure-data/pure-data) por miller puckette y muchxs más
[camomile](https://github.com/pierreguillot/Camomile) por pierre guillot
