# LaTeX Setup
Repository containing .sty files I use for my homework and notes.

## Table of Contents
[homework.sty](#homeworksty)

  - [mytitle](#mytitle)
  - [listoftheorems](#listoftheorems)
  - [hw](#hw)

## homework.sty
This section will detail some of the custom features, environments, and commands in `homework.sty`.
### mytitle
The `homework.sty` file has its own custom command `\mytitle` that creates a title page, along with the list of problems page. The command takes in two arguments `arg1` and `arg2`, where `arg1` is the title of the document, and `arg2` the date.

`\mytitle{arg1}{arg2}` is defined as follows:
```latex
\newcommand{\mytitle}[2]{%
	\title{#1}
	\author{...}
	\date{#2}
	\maketitle
	\newpage
	\listoftheorems
	\newpage
}
```
Remember to change the author from `...` to your own name!

### listoftheorems
The default `\listoftheorems` is redefined to look like the following:
![Default Problems Page](https://github.com/ktm-p/LaTeX-Setup/assets/119767232/cd20f5c4-028d-44ff-8f9c-d4ee8f751d54)

Due to how `.sty` files interact with `\makeatletter` and `makeatother`, you have to paste the following code into your preamble in order to swap the theorem name and number around in the Problems page:
```latex
\renewcommand\thmtformatoptarg[1]{:\enspace#1}
\makeatletter
\def\ll@homework{
	\thmt@thmname~
	\protect\numberline{\csname the\thmt@envname\endcsname}%
	\ifx\@empty
	\thmt@shortoptarg
	\else
	\protect\thmtformatoptarg{\thmt@shortoptarg}
	\fi
}
\makeatother

\makeatletter
\renewcommand*{\numberline}[1]{\hb@xt@3em{#1}}
\makeatother	
```

After doing so, the Problems page will look like this:
![Modified Problems Page](https://github.com/ktm-p/LaTeX-Setup/assets/119767232/e6577631-5fb1-4a79-adee-a9e3064aca4a)


### hw
The `homework.sty` file also has its own homework environment: `hw`. It uses `thmtools` for its appearance, and is numbered within the section.

The environment is as such:
```latex
\begin{hw}{arg1}[arg2]
  This is a homework environment.
\end{hw}
```

`arg1` specifies the homework problem number and is mandatory. If the question has a subpart (i.e. 4.3a), you can enter a second argument in `arg2` with a number corresponding to the position of the letter in the alphabet.

Below is an image of what the homework environment looks like after running the following code:
```latex
\section{This is a section}
\begin{hw}{arg1}[arg2]
  This is a homework environment.
\end{hw}
```
![Example of Homework Environment](https://github.com/ktm-p/LaTeX-Setup/assets/119767232/df1eb014-6e13-4300-825d-d3a228d7e069)

