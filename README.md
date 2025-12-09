# Proyecto: Mi canción favorita

![chandler](https://github.com/user-attachments/assets/2427df21-5bdd-424b-8c9a-3d879d9f9b4d)

## Anteriormente en otras tareas:

En la *Tarea 1* ya pude ver cuales eran los artistas que más se repetían en mi playlist de canciones favoritas, dando como resultado el siguiente gráfico.

<img width="2700" height="1800" alt="artistas_mas_repetidos_playlist" src="https://github.com/user-attachments/assets/f5834cb0-bd10-4e93-93d1-b0ceb90fd266" />

Pero, todavía estaba muy lejos de tener al menos un indicio de cual de todas esas canciones es precisamente mi favorita. En la *Tarea 2* me acerqué, dando cuenta cual de todas estas canciones era la más escuchada. Así, con datos de mi historial de escucha obtuve el siguiente gráfico:

<img width="2400" height="1800" alt="canciones_mas_escuchadas_playlist" src="https://github.com/user-attachments/assets/e3c1c78a-d16e-4f9c-b8c4-6fa25557c810" />

Y también incluí uno de mis artistas más reproducidos de la misma playlist, dando este resultado:

<img width="2700" height="1800" alt="artistas_mas_reproducidos_playlist" src="https://github.com/user-attachments/assets/0b78c552-c642-4ba5-9697-5caa6bbc70ad" />

# Tarea 3:

Para la *Tarea 3* decidí hacer un análisis más profundo de mis gustos musicales, para así poder determinar con mayor certeza cual es mi canción favorita. Para esto se me ocurrió (la profe nos pidió) usar Análisis de Componentes Principales en esta tarea. Gracias a Spotify y sus datos pude obtener las características acústicas de cada canción de mi playlist, estas son:

1. Valence: mide "positividad" que transmite la canción. Va de 0.0 a 1.0. + = alegría / - = tristeza
2. Energy: representa una medida de intesidad y actividad. Va de 0.0 a 1.0. Se basa en el volumen y timbre.
3. Danceability: que tan apta es para bailar la canción. Va de 0.0 a 1.0. Combina tempo, estabilidad del ritmo, etc.
4. Tempo: la velocidad estimada de la canción. Medida en BPM (beats por minuto).
5. Acousticness: medida de confianza si la canción es acústica. Va de 0.0 a 1.0.
6. Loudness: volumen promedio de la pista. Medida en decibeles.
7. Instrumentalness: predice si una pista no contiene voces. Va de 0.0 a 1.0. Más cercano a 0 = canciones con mucha letra

Estos datos se supone que los podría obtener de la librería "Spotifyr" pero por alguna razón no pude. Así que tuve que recurrir a una página llamada "Exportify" para obtener en CSV estas variables de la misma playlist.

# Estandarización:

Antes de realizar el análisis tuve que estandarizar algunas variables para que las variables más grandes no distorsionaran el análisis. Este fue el código que usé:

```{r}
df_playlist_est <- df_playlist |> 
  select(Danceability, Energy, Valence, Tempo, Acousticness, Instrumentalness, Loudness) #selección de variables

res_pca <- prcomp(df_playlist_est, scale. = TRUE)

fviz_eig(res_pca, addlabels = TRUE) # Esto me muestra que con 2 componentes principales ya explican la varianza.

```
<img width="750" height="625" alt="scree_plot_1" src="https://github.com/user-attachments/assets/f87524cf-9a02-4755-8cc7-71e273b41325" />

Ante esto, el gráfico de sedimentación mostró que las dos primeras barras (dim 1 y 2) capturan casi un 60% de la varianza de los datos. Esto hace que se justifique su interpretación. Reducirlo a dos ejes sería válido y suficiente para entender los patrones de escucha.

# Que encontré:

<img width="750" height="625" alt="biplot_1" src="https://github.com/user-attachments/assets/2cf5204d-24b3-44df-8840-5071df871597" />

Este biplot muestra los vectores, que indican la dirección en la que aumenta cada variable. Hacía la derecha van Energy/Loudness, a la izquierda Acousticness y hacía arriba Valance/Danceability. Aquí ya podemos ver un cluster dominante, que sería el cuadrante 4, de alta energía y tempo rápido, pero con un contrapeso en el cuadrante contrario, donde predominan canciones acústicas y de baja energía.

<img width="750" height="625" alt="biplot_top20" src="https://github.com/user-attachments/assets/bdf343d4-c727-4e3b-9c2a-0f4276734702" />

Al verlo más en profundidad, podemos ver a ciertas canciones representan mejor a ciertos cuadrantes. Así, podemos ver como artistas como A Day To Remember y Paramore se encuentran más en el cuadrante 4, la canción más representativa de este cuadrante sería "All I Want" de A Day To Remember. Mientras que el cuadrante 2 y 3 predominan artistas como Sam Smith y Noah Kahan, siendo "Fear of Water" de este mismo la canción representativa de este cuadrante.

Existe un caso particular que es una canción ubicada en el cuadrante 1 de alto Valence y bailable, este outlier es nada más y nada menos que "On Melancholy Hill" de Gorillaz.

Así, estas serían las 20 canciones más representativas de cada cuadrante:

<img width="957" height="723" alt="image" src="https://github.com/user-attachments/assets/586cf1b6-1c32-45f0-8a24-f9d0331b5f03" />

Finalmente, dejo una tabla dinámica para ver en que cuadrante se encuentra cada canción (se encuentra en la carpeta de output, pero dejo un preview):

<img width="1873" height="855" alt="image" src="https://github.com/user-attachments/assets/c4aca36a-ba97-4423-a2c5-d2e4aa709050" />

# Conclusiones finales

Antes de determinar cual es mi canción favorita definitiva, la verdad es que terminé por decantarme por las 3 descritas antes. "All I Want" muestra a la perfección mi gusto por las canciones de alta energía, lo que se relaciona con mi gusto a géneros como el pop-punk y el metalcore. "Fear of Water" también representa mi gusto por la música más chill y la verdad más depresiva, pero no puedo negar mi gusto por una buena balada. Y la mención honrosa a mi outlier "On Melancholy Hill" que en realidad sería como un equilibrio entre todos estos gustos. 

Por lo que, en conclusión, luego de todo este análisis, quiero determinar a "All I Want" de A Day To Remember como mi canción favorita de toda la vida. Los datos así lo muestran y existe algo que los datos no pueden medir y es lo significativa que ha sido esta canción en mi vida, es básicamente mi mantra. Cierro con la letra que algún día me voy a tatuar "Keep your hopes up high and your head down low" <3
