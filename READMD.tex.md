\documentclass{article}
\usepackage[utf8]{inputenc}

\title{README for constrmag.F}
\author{Pui-Wai Ma\\ Culham Centre for Fusion Energy, Abingdon, Oxfordshire, OX14 3DB\\ United Kingdom}

\date{\today}

\begin{document}

\maketitle
\section{Introduction}
We added a new constrained non-collinear magnetism method to VASP. It can be implemented easily by recompiling VASP with our modified constrmag.F. Please cite the following if you are using this method.
\\
\\
Pui-Wai Ma and S. L. Dudarev, \textit{Constrained density functional for noncollinear magnetism}, Physical Review B 91, 054420 (2015)



\section{Theory}
We define the magnetic moment of an atom  as
\begin{eqnarray}
\mathbf{M}_I=\int_{\Omega_I}\mathbf{m(r)}d^3 r,
\label{eq1}
\end{eqnarray}
where $\mathbf{m(r)}$ is a spatially varying magnetization density, and $\Omega_I$ is a sphere centred at atom $I$. 

Instead of using $\mathbf{M}_I$ directly, we use an alternative definition of the local magnetic moment, namely
\begin{eqnarray}
\mathbf{M}^F_I=\int_{\Omega_I}\mathbf{m(r)}F_I(|\mathbf{r}-\mathbf{r}_I|)d^3 r
\label{eq2}
\end{eqnarray}
where $F_I(|\mathbf{r}-\mathbf{r}_I|)$ is a scalar function decreasing monotonically to zero towards the boundary of the atomic sphere. Such definition of $\mathbf{M}^F_I$ was adopted in VASP for other constrained method. 

The constrained total energy functional has the form:
\begin{eqnarray}
E &=& E_0 + E_p \label{eq3}\\
&=& E_0 + \sum_I \lambda _I\left(\left| \mathbf{M}^F_I \right| -\mathbf{e}_I\cdot\mathbf{M}^F_I\right)\label{eq4}
\end{eqnarray}
where $E_0$ is the DFT energy of the material, $E_p$ is the penalty energy term, $\mathbf{e}_I$ is a unit vector in the desired direction of the local magnetic moment, and $\lambda _I$ is a Lagrange multiplier associated with site $I$. The dimensionality of $\lambda _I$ is the same as of external magnetic field.

The penalty energy term in (\ref{eq4}) introduces an effective extra potential inside each sphere $\Omega_I$ centred at atom $I$, given by
\begin{eqnarray}
V_I(\mathbf{r})=-\mathbf{b}_p(\mathbf{r})\cdot\bm{\sigma} \label{eq5}
\end{eqnarray}
where $\bm{\sigma}$ is the vector of Pauli matrices, and
\begin{eqnarray}
\mathbf{b}_p(\mathbf{r})&=&-\frac{\delta E_p}{\delta \mathbf{m(r)}} \label{eq6} \\
&=& -\lambda _I\left(\frac{\mathbf{M}^F_I}{|\mathbf{M}^F_I|}-\mathbf{e}_I\right)F_I(|\mathbf{r}-\mathbf{r}_I|) \label{eq7}
\end{eqnarray}
is an additional penalty ``field" in the Kohn-Sham equations. Equations \ref{eq4} and \ref{eq7} show that both $E_p$ and $V_I(\mathbf{r})$ terms vanish only if vector $\mathbf{M}^F_I$ points in the same direction as $\mathbf{e}_I$.

From equation (\ref{eq6}) we see that functions $F_I(|\mathbf{r}-\mathbf{r}_I|)$ eliminate discontinuities of the effective potential at the boundaries of atomic spheres $\Omega_I$. We do not need to separate the core and interstitial regions. The part played by the penalty term in this respect is similar to the action of local spatially-varying external magnetic field.

For more details, please refer to our Phys. Rev. B paper.

\section{Controls}
Our constrained method can be activated by using:
\\
\\
I\_CONSTRAINED\_M=4
\\
\\
Other details of setup are the same as using I\_CONSTRAINED\_M=1. 
\\
\\
Additionally, if VASP is compiled with "-Dconstrmagtanh", it replaces the function
\begin{equation}
F_I(|\mathbf{r}-\mathbf{r}_I|)=\sin(x)/x,
\end{equation}
where $x=\pi (|\mathbf{r}-\mathbf{r}_I|)/R_I$ by 
\begin{equation}
F_I(|\mathbf{r}-\mathbf{r}_I|)=-\tanh((x/\pi-1)/c)
\end{equation}
where we set $c=0.01$. It is useful for I\_CONSTRAINED\_M=2.


\end{document}

