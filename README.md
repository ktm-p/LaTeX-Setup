# LaTeX Setup
Repository containing .sty files I use for my homework and notes.

## Table of Contents
[macros.sty](#macrossty)

[homework.sty](#homeworksty)
  - [mytitle](#mytitle)
  - [listoftheorems](#listoftheorems)
  - [hw](#hw)
## macros.sty
The `macros.sty` file contains a list of commands and macros that helps make it faster to TeX things up. For example, there are bracketing macros (`\pr`, `\br`, `\brc`, and `\ang` for different brackets), macros for common sets (such as `\RR` for `\mathbb{R}`), and macros so you can avoid doing `\mathrm{arg}`.

There are also some custom commands such as `\aug`, which creates the line for augmented matrices. A working example is shown below, with the code:
```latex
\begin{equation*}
	\begin{bmatrix}
		1 & 0 & \aug & a \\
		0 & 1 & \aug & b
	\end{bmatrix}
\end{equation*}
```
![Augmented Matrix](https://github.com/ktm-p/LaTeX-Setup/assets/119767232/fe98c146-81c4-4599-81fc-794345a235fc)


## homework.sty
This section will detail some of the custom features, environments, and commands in `homework.sty`. This is intended to be used with an `article` document class.

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

Due to how `.sty` files interact with `\makeatletter` and `\makeatother`, you have to paste the following code into your preamble in order to swap the theorem name and number around in the Problems page:
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
The `homework.sty` file also has its own homework environment: `hw`. It uses `thmtools` for its appearance, and is numbered within the section. The code is as follows:
```latex
\newcounter{dscounter}
\NewDocumentEnvironment{hw}{m O{0} O{0}}{
	\IfNoValueT{#2}{\renewcommand*{\thehomework}{\thesubsection}\setcounter{subsection}{#1}\begin{homework}}
	\IfNoValueF{#2}{\IfNoValueTF{#3}
		{\renewcommand*{\thehomework}{\thesubsection\alph{homework}}\setcounter{subsection}{#1}\setcounter{homework}{\inteval{#2-1}}\begin{homework}}
		{\renewcommand*{\thehomework}{\thesubsection\alph{dscounter}\roman{homework}}\setcounter{subsection}{#1}\setcounter{dscounter}{#2}\setcounter{homework}{\inteval{#3-1}}\begin{homework}}
		}
	}
	{\end{homework}}
```

To use the environment, we can do the following:
```latex
\begin{hw}{arg1}[arg2][arg3]
  This is a homework environment.
\end{hw}
```

`arg1` specifies the homework problem number and is mandatory. If the question has a subpart (i.e. 4.3a), you can enter a second argument in `arg2` with a number corresponding to the position of the letter in the alphabet. Furthermore, you can define a sub-subpart (i.e. 4.3aii) by entering third argument in `arg3` with a number corresponding to the desired roman numeral.

If `arg2` and `arg3` are not specified, the default value is 0 (which will make the subpart and sub-subpart not appear).

Below is a demonstration of the different homework numberings:
```latex
\section{This is a section}
\begin{hw}{1}
  This is a homework environment without any subparts or sub-subparts.
\end{hw}

\begin{hw}{1}[2]
  This is a homework environment with a subpart but no sub-subpart.
\end{hw}

\begin{hw}{1}[2][3]
  This is a homework environment with a subpart and sub-subpart.
\end{hw}
```

![Example of Homework Environments](https://github.com/ktm-p/LaTeX-Setup/assets/119767232/5968e5f2-ba6c-4952-b63c-1bea2b8645d5)
