# $${\color{slateblue}FUNDAMENTOS \space DE \space PROGRAMACIÓN}$$

<p align="center">
  <img src="https://www.gifcen.com/wp-content/uploads/2023/07/neon-gif-6.gif" />
</p>

$${\color{slateblue}Abdiel \space Josue \space Pacheco \space Robles}$$
$${\color{slateblue}Ingeniería \space en \space Tecnologías \space de \space la \space Información \space e \space Innovación \space Digital}$$
$${\color{slateblue}Enero \space - \space Abril \space // \space Primer \space Cuatrimestre}$$
$${\color{slateblue}Universidad \space Politécnica \space Del \space Estado \space De \space Nayarit}$$

$${\color{slateblue}Los \space fundamentos \space de \space la \space programación \space nos \space ayudan \space a \space acostumbrarnos \space a \space la \space buena \space praxis}$$
$${\color{slateblue}y \space conocer \space aquello \space que \space es \space necesario \space para \space lidiar \space con \space materias \space más \space avanzadas.}$$

$${\color{slateblue}⚠El \space símbolo \space de \space eslabón \space (🔗) \space debe \space llevarle \space a \space la \space respectiva \space carpeta⚠}$$

<p align="center">
  <img src="https://i.pinimg.com/736x/e8/d9/21/e8d921a629b0695f85e8d701055fdd22.jpg" />
</p>

% Random city
% Author: Pascal Seppecher
\documentclass[tikz,border=10pt]{standalone}
\usetikzlibrary{backgrounds}
\usepackage{ifthen}
% The blue print color
\definecolor{blueprintcolor}{RGB}{20,20,100}

% The shadow color
\colorlet{shadow}{blueprintcolor!50!white}

% The light color
\colorlet{light}{white!90!blueprintcolor}

% The background color
\colorlet{background}{blueprintcolor}

% The basic size of a block
\newcommand\defaultside{0.6}

% The height of a storey
\newcommand\floorheight{0.08}

% The minimum number of stories
\newcommand\storeymin{20}

% The maximum number of stories
\newcommand\storeymax{40}

% The width of a window
\newcommand\window{0.05}

% The angles of x,y-axes
\newcommand\xaxis{210}
\newcommand\yaxis{-30}

% Facade style random list
\pgfmathdeclarerandomlist{facade}{{\hlines}{\vlines}{\grid}{\grid}{\grid}}

% Vertical thickness
\newcommand\vthickness{thin}

% Horizontal thickness
\newcommand\hthickness{thin}

% Selects at random the thickness of the horizontal lines
\newcommand\setvthickness{
  \pgfmathrandominteger{\alea}{0}{3}
  \ifthenelse{\alea=0}{\renewcommand\vthickness{thick}}{}
  \ifthenelse{\alea=1}{\renewcommand\vthickness{thin}}{}
  \ifthenelse{\alea=2}{\renewcommand\vthickness{very thin}}{}
  \ifthenelse{\alea=3}{\renewcommand\vthickness{ultra thin}}{}
}

% Selects at random the thickness of the vertical lines
\newcommand\seththickness{
  \pgfmathrandominteger{\alea}{0}{3}
  \ifthenelse{\alea=0}{\renewcommand\hthickness{thick}}{}
  \ifthenelse{\alea=1}{\renewcommand\hthickness{thin}}{}
  \ifthenelse{\alea=2}{\renewcommand\hthickness{very thin}}{}
  \ifthenelse{\alea=3}{\renewcommand\hthickness{ultra thin}}{}
}

% Draws vertical lines on each side of the block
\newcommand\vlines[2]{
  \pgfmathsetmacro\size{#1 * \floorheight}
  \pgfmathsetmacro\max{#2/\window}
  \foreach \col in {1,...,\max}
  {
    \pgfmathsetmacro\xx{-\col * \window}    
    \draw[\vthickness,draw=shadow, shift={(\yaxis:\xx)}] (0,0)--(0,\size);
    \draw[\vthickness,draw=light, shift={(\xaxis:\xx)}] (0,0)--(0,\size);
  }  
}

% Draws horizontal lines on each side of the block
\newcommand\hlines[2]{
  \foreach \floor in {0,...,#1}
  {
    \pgfmathsetmacro\z{\floor * \floorheight}    
    \draw[\hthickness,draw=shadow, shift={(90:\z)}] (150:#2)--(0,0);
    \draw[\hthickness,draw=light, shift={(90:\z)}] (0,0) -- (30:#2);
  }
}

% Draws horizontal and vertical lines on each side of the block
\newcommand\grid[2]{
  \vlines{#1}{#2}
  \hlines{#1}  {#2}
}

% Draws a block at the specified position
\newcommand\block[2]{
  % Computes the height of the block
  \pgfmathsetmacro\height{#2 * \floorheight}
    
  % Erases the background
  \fill[fill=background]
    (0,0) -- (150:#1) -- ++(0,\height) -- (0,\height) -- (0,0);
  \fill[fill=background]
    (0,0) -- (30:#1) -- ++(0,\height) -- (0,\height) -- (0,0);
  
  % Draws the facades
  \facade{\stories}{#1}
  
  % Frames the facades
  \draw[draw=shadow] (0,0) -- (150:#1) -- ++(0,\height) -- (0,\height) -- (0,0);
  \draw[draw=light] (0,0) -- (30:#1) -- ++(0,\height) -- (0,\height) -- (0,0);
  
  % Draws the terrace
  \fill[fill=background, draw=light,shift={(90:\height)}]
    (0,0) -- (30:#1) -- (0,#1) --(150:#1)--(0,0);

  %
  \pgfmathrandominteger{\alea}{0}{3}
  \ifthenelse{\alea=0}{
  % Sometimes, adds more stores (= skyscraper)
    \begin{scope}[shift={(0,\height)},shift={(0,0.05)}]
      \pgfmathsetmacro\pside{#1-0.1}
      \pgfmathrandominteger{\stories}{2}{\storeymax}
      \block{\pside}{\stories}
    \end{scope}  
  }{}
  
  \ifthenelse{\alea=1}{
  % Sometimes, draws a pyramid roof
  \pyramid{\height}{#1}  
  }{}
}

% Draws a basic block
\newcommand\basicblock[3]{
  % Selects a random facade style
  \pgfmathrandomitem{\facade}{facade}
  \setvthickness{}
  \seththickness{}
  
  % Selects a random number of stores
  \pgfmathrandominteger{\stories}{\storeymin}{\storeymax}
  
  \begin{scope}[shift={(\xaxis:#1)},shift={(\yaxis:#2)}]
  \block{#3}{\stories}
  \end{scope}
}

% Draws a pyramid roof
\newcommand\pyramid[2]{
  % Computes the side of the pyramid
  \pgfmathsetmacro\pside{#2-0.1}

  % Selects the height of the pyramid at random
  \pgfmathparse{random()}
  \pgfmathsetmacro\top{0.5+\pgfmathresult/3}

  % Draws the pyramid
  \begin{scope}[fill=background, draw=light,shift={(90:#1)},shift={(90:0.05)}]
    \fill[draw=light] (0,0) -- (30:\pside) -- (0,\top) -- (150:\pside)--(0,0);
    \draw (0,0) -- (0,\top);
  \end{scope}
}

% Draws a random city of the specified dimensions
\newcommand\city[2]{
  \foreach \x in {1,...,#1}
    \foreach \y in {1,...,#2}
      {\basicblock{\x}{\y}{\defaultside}}
}

\begin{document} 
% Draws a hundred blocks random city
\begin{tikzpicture}[show background rectangle,
  background rectangle/.style={fill=background}]
    \city{10}{10}
\end{tikzpicture}
\end{document}

# $${\color{slateblue}Unidad \space 1}$$ [ 🔗 ](https://github.com/W1nso1/FundProgr_UPEN_AP/tree/main/U1%20)
##
$${\color{slateblue}Esta \space unidad \space sirvió \space para \space conocer \space y \space aplicar \space lo \space que \space es \space un \space algoritmo,\space además \space de \space utilizar \space matemática}$$
$${\color{slateblue}básica \space para \space lidiar \space con \space problemas \space de \space relativa \space sencillez, \space aquí \space el \space programa \space PSeInt \space dejó \space de \space ser.}$$
$${\color{slateblue}desconocido \space para \space nosotros.}$$
  

# $${\color{slateblue}Unidad \space 2}$$ [ 🔗 ](https://github.com/W1nso1/FundProgr_UPEN_AP/tree/main/U2)
##
$${\color{slateblue}Aquí \space tratamos \space los \space temas \space de \space FOR, \space IF \space Y \space WHILE \space como \space parte \space de \space nuestro \space aprendizaje \space dentro \space de \space la \space carrera \space de \space ITIID,}$$
$${\color{slateblue}aunque \space sean \space asuntos \space que \space puedan \space parecer \space nimiedades \space es \space importante \space practicar \space su \space alcance \space y \space limitantes.}$$
  

# $${\color{slateblue}Unidad \space 3}$$ [ 🔗 ](https://github.com/W1nso1/FundProgr_UPEN_AP/tree/main/U3)
##
$${\color{slateblue}Esta \space unidad \space estuvo \space basada \space en \space el \space adquirir \space conocimientos \space del \space uso \space de \space GitHub \space debido \space a \space su \space increible \space utilidad.}$$
$${\color{slateblue}Es \space esta \space unidad \space donde \space éste \space repositorio \space vería \space su \space nacimiento.}$$
  
<p align="center">
  <img src="https://images-wixmp-ed30a86b8c4ca887773594c2.wixmp.com/f/cf2836cb-5893-4a6c-b156-5a89d94fc721/db0748x-9fcb8587-a61b-4a2e-b440-5d0de36e7a26.gif?token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ1cm46YXBwOjdlMGQxODg5ODIyNjQzNzNhNWYwZDQxNWVhMGQyNmUwIiwiaXNzIjoidXJuOmFwcDo3ZTBkMTg4OTgyMjY0MzczYTVmMGQ0MTVlYTBkMjZlMCIsIm9iaiI6W1t7InBhdGgiOiJcL2ZcL2NmMjgzNmNiLTU4OTMtNGE2Yy1iMTU2LTVhODlkOTRmYzcyMVwvZGIwNzQ4eC05ZmNiODU4Ny1hNjFiLTRhMmUtYjQ0MC01ZDBkZTM2ZTdhMjYuZ2lmIn1dXSwiYXVkIjpbInVybjpzZXJ2aWNlOmZpbGUuZG93bmxvYWQiXX0.Mv1MCMhq82G6jmVgNp-A7S9sKfmy1ZzaTM5IeXu-SkY" />
</p>

$${\color{slateblue} \textbf Personalmente \space creo \space que \space la \space materia \space fue \space llevadera, \space las \space actividades \space siguieron \space un \space orden \space lógico.}$$
$${\color{slateblue}Entonces \space mi  \space único \space deseo \space habría \space sido \space tener \space muchas \space más \space horas \space para \space abarcar \space más \space temas \space y \space expandir \space el \space repositorio.}$$

<p align="center">
  <img src="https://i.pinimg.com/736x/66/5c/67/665c67b67818fedeae9f0c7eb0c9882b.jpg" />
</p>
