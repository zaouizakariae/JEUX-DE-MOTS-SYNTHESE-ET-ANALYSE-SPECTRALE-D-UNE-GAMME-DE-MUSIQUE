# JEUX-DE-MOTS-SYNTHESE-ET-ANALYSE-SPECTRALE-D-UNE-GAMME-DE-MUSIQUE

## Introduction

L'analyse spectrale d'un son repose sur la décomposition de Fourier, elle permet de connaître les fréquences des différentes fonctions sinusoïdales présentes dans un son (appelées harmoniques). Dans ce TP, nous allons travailler sur 2 exercices qui vont nous permettre de se familiariser avec des outils d’analyse spectrale dont Matlab dispose.

## Jeux de mots
- « phrase.wave » est un fichier audio enregistré à l’aide d’un smartphone, en prononçant lentement la phrase :
  -  « Rien ne sert de courir, il faut partir à point »
- Le but de cet exercice est d’échantillonner le signal en 4 morceaux après on change leur emplacements pour obtenir un nouveau signal .
- Pour cela on charge le fichier 'phrase.wav', on le trace dans une figure avec la commande plot() qui prend comme argument le signal à visualiser .Dans ce cas on a pas précisé l’axe du temps puisqu’on veut le visualiser en fonction de ses indices pour pouvoir récupérer l’indice de chaque mot constituant la phrase.
- On peut entendre la phrase en appliquant sound().
- Lorsqu’on multiplie la fréquence d’échantillonnage par un facteur 2 sound(y,2*Fs) on obtient un son compressé, rapide et aigu. En effet, cette modification revient à appliquer un changement d’échelle c.à.d. opérer une compression du spectre initiale d ou une version plus aigu du signal écouté.
- Et si on divise la fréquence d’échantillonnage par 2 sound(y,Fs/2)on dilate le signal d ou une version plus grave et moins rapide du signal écouté .

![capture 152](https://user-images.githubusercontent.com/85891554/151433352-43ed2028-34d2-4929-aa67-092e4c5df291.jpeg)

- Chaque pique de fréquence correspond à un mot de la phrase. On découpe le fichier en morceaux (riennesertde, courir, ilfaut, partirapoint) chacune dans son intervalle de fréquence précis et finalement dans un vecteur newvoice on place le nouveau signal.

```matlab
clear all
close all
clc
phrase = 'phrase.wav';
[x,fs]=audioread(phrase);
 plot(x);
% sound(x,Fs);
%Ft=0.70*fs;
% sound(x,Ft);
n=length(x);
dt=1/fs;
t=[0:n-1]*dt;
% plot(t,x);

rien=x(14000:75500 );
% sound(rien,fs);
courir=x(77885:96176);
faut=x(97569:125772);
partir=x(126000:171008);
o=[rien,partir,faut,courir];
sound(o,fs);
```
# Synthèse et analyse spectrale d’une gamme de musique

## Synthèse d’une gamme de musique

![Capture d’écran 2022-01-27 205018](https://user-images.githubusercontent.com/85891554/151433217-ba5a0bed-a63e-4386-b066-350c6a97d0eb.png)

- Dans cette question nous avons défini la gamme de musique comme étant un vecteur constitué de 8 sons purs. Chaque son pur est défini par : sin(2*pi*f*t) avec f correspondant à la fréquence propre de chaque note musicale et t un vecteur allant de 0 à 1 avec un pas de 1/fe. 

```matlab
fs=8192;
t = 0:1/fs:1
dol=sin(2*pi*262*t);
re= sin(2*pi*294*t);
mi=sin(2*pi*330*t);
fa=sin(2*pi*349*t);
sol=sin(2*pi*392*t);
la=sin(2*pi*440*t);
si=sin(2*pi*494*t);
do2=sin(2*pi*523*t);
gamme=[do1 re mi fa sol la si do2];
%gamme=[mi mi fa sol sol fa mi re do2 do2 re mi mi re re mi mi fa sol sol fa mi re do2 do2 re mi re do2 do2];
%sound(gamme,fs*1.25);
```

- On peut entendre la gamme musicale grâce à la commande : sound(gamme,fs)

## Spectre de la gamme de musique

- L'application Signal Analyzer est un outil interactif pour visualiser, mesurer, analyser et comparer des signaux dans le domaine temporel, dans le domaine fréquentiel et dans le domaine temps-fréquence. L'application permet de travailler avec de nombreux signaux de durées variables en même temps et dans la même vue. Pour lancer le programme on utilise la commande suivante :

```matlab
signalAnalyzer(gamme);
```

![Capture d’écran 2022-01-27 205715](https://user-images.githubusercontent.com/85891554/151434180-078cfca2-1dab-4668-b2d1-d5678dc1be37.png)

On peut visualiser notre signal (gamme) dans le domaine temporel en fonction des indices (plot 1).
En cliquant sur le bouton Spectrum on peut visualiser le Spectrum de notre signal. En effet, le Spectrum nous permet de visualiser les différentes fréquences qui constituent le signal en dB et suivant la loi normale appelées fréquences normalisées. Comme on peut le voir dans le Plot 2, on retrouve les 8 fréquences constituant notre signal de départ. Si on utilise le curseur on peut observer la valeur numérique des fréquences en dB.

- On peut visualiser le spectrogramme de notre signal (plot 3) en cliquant sur le bouton Time-Frequency. En effet, les spectrogrammes sont une représentation bidimensionnelle du spectre de puissance d'un signal lorsque ce signal balaie le temps. Ils donnent une compréhension visuelle du contenu fréquentiel du signal. Pour lire la valeur de la fréquence on regarde la zone la plus foncée dans ce cas la bande jaune et on retrouve la valeur de la fréquence normalisée.

## Approximation du spectre d’un signal sinusoïdal à temps continu par FFT 

- Pour tracer le spectre de fréquence de la gamme musicale crée en échelle linéaire, on utilise la fonction fft qui permet de calculer la transformée de Fourier rapide puis on utilise la fonction fftshift qui permet de rendre le spectre symétrique par rapport à l’axe des ordonnées. On définit l’axe de fréquence f et on utilise plot pour tracer le spectre. On obtient le résultat suivant :

![Capture d’écran 2022-01-27 210505](https://user-images.githubusercontent.com/85891554/151435261-9676cb2b-d618-4c66-9c67-de78f4970558.png)

> on zoom

![Capture d’écran 2022-01-27 210631](https://user-images.githubusercontent.com/85891554/151435470-9c0094d7-164f-4561-891a-1eafeb1467f5.png)

```matlab
N=length(gamme);
tfd=fftshift(fft(gamme))
f=(-N/2:(N/2)-1)*(fs/N);
plot(f,abs(tfd));
```

Pour tracer le spectre de fréquence en dB, on utilise la fonction mag2db() qui utilise la formule suivante : 

S(dB) = 20 * log10( |S(f)| )

```matlab
tfdb=mag2db(abs(tfd));
figure(2)
plot(f,tfdb);
```

On obtient la figure suivante :

![Capture d’écran 2022-01-27 211052](https://user-images.githubusercontent.com/85891554/151436111-e211f203-2444-45bd-8f0e-8d4d0fdfcb68.png)

> On peut observer les 8 fréquences qui constituent la gamme de musique.

![Capture d’écran 2022-01-27 211300](https://user-images.githubusercontent.com/85891554/151436362-11073ca2-cc02-4502-9ffe-5f0986af10a2.png)

# Consclusion

Dans ce TP nous avons pu comprendre comment manipuler un signal audio avec Matlab, en effectuant certaines opérations classiques sur un fichier audio d’une phrase enregistrée via un smartphone. Ainsi que de comprendre la notion des sons purs à travers la synthèse et l’analyse spectrale d’une gamme de musique.
