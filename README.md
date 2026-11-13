# Graph-theory-
\documentclass[11pt]{article}
\usepackage[a4paper,margin=20mm,headheight=14pt]{geometry}
\usepackage[T1]{fontenc}
\usepackage{lmodern}
\usepackage{microtype}
\usepackage[table,dvipsnames]{xcolor}
\usepackage{booktabs,tabularx,array}
\usepackage{enumitem}
\usepackage{titlesec}
\usepackage{fancyhdr}
\usepackage{hyperref}
\usepackage{xurl}
\usepackage{tikz}
\usetikzlibrary{arrows.meta,positioning,calc,fit}

\definecolor{navy}{HTML}{17324D}
\definecolor{teal}{HTML}{087E8B}
\definecolor{river}{HTML}{2878A8}
\definecolor{leaf}{HTML}{3F8F62}
\definecolor{amber}{HTML}{D98B27}
\definecolor{brick}{HTML}{BF4E4E}
\definecolor{pale}{HTML}{EEF4F8}
\definecolor{palegreen}{HTML}{E7F3E8}
\definecolor{paleyellow}{HTML}{FFF4D5}
\definecolor{ink}{HTML}{263238}

\hypersetup{colorlinks=true,linkcolor=black,citecolor=black,urlcolor=black,
 pdftitle={Seeing Graph Theory: A Visual Guide to Graph Types for Twin Hazard},
 pdfauthor={Green Pulse Q5 companion guide}}
\titleformat{\section}{\Large\bfseries}{\thesection}{0.65em}{}
\titleformat{\subsection}{\large\bfseries}{\thesubsection}{0.65em}{}
\setlength{\parindent}{1em}
\setlength{\parskip}{1pt}
\setlist[itemize]{leftmargin=1.35em,itemsep=2pt,topsep=3pt}
\setlist[enumerate]{leftmargin=1.5em,itemsep=2pt,topsep=3pt}
\pagestyle{plain}
\setcounter{tocdepth}{2}

\tikzset{
  vtx/.style={circle,draw=navy,fill=pale,minimum size=8mm,inner sep=1pt,font=\small},
  smallv/.style={circle,draw=navy,fill=pale,minimum size=7mm,inner sep=1pt,font=\scriptsize},
  greenv/.style={circle,draw=leaf!80!black,fill=palegreen,minimum size=8mm,inner sep=1pt,font=\small},
  bluev/.style={circle,draw=river,fill=river!9,minimum size=8mm,inner sep=1pt,font=\small},
  goldv/.style={circle,draw=amber!80!black,fill=paleyellow,minimum size=8mm,inner sep=1pt,font=\small},
  facility/.style={rounded corners=2pt,draw=navy,fill=pale,minimum width=1.3cm,minimum height=7mm,align=center,font=\scriptsize,inner sep=2pt},
  action/.style={rounded corners=2pt,draw=leaf!80!black,fill=palegreen,minimum width=1.3cm,minimum height=7mm,align=center,font=\scriptsize,inner sep=2pt},
  line/.style={draw=navy,line width=0.8pt},
  arrow/.style={-{Stealth[length=2mm]},draw=river,line width=0.95pt},
  good/.style={draw=leaf!80!black,line width=1.4pt},
  bad/.style={draw=brick,line width=1.4pt},
aux/.style={draw=gray!70,dashed,line width=0.85pt}
}

\begin{document}
\thispagestyle{empty}
\begin{flushleft}
{\LARGE\bfseries Seeing Graph Theory}\\[3pt]
{\large A visual guide to graph types for Twin Hazard}\\[5pt]
\end{flushleft}
\vspace{2mm}
\begin{center}
{\normalsize Green Pulse 2026 \quad | \quad Theme 04, Question 5}\\[5pt]
{\small Original city-planning diagrams and case studies for Bengaluru's heat, flooding and green-blue spaces.}
\end{center}
\vspace{5mm}
\tableofcontents
\clearpage
\pagenumbering{arabic}
\setcounter{page}{1}

\section{What is a graph?}
\subsection{Start with the object, then choose the graph}

A graph is commonly written $G=(V,E)$: $V$ is the set of nodes (or vertices), and $E$ is the set of edges (or links). A city model might use drain junctions as nodes and pipes as edges, or road intersections as nodes and walkable street segments as edges. The same city can be represented by many graphs because each graph answers a different question.

\begin{center}
\small
\begin{tabularx}{\linewidth}{@{}>{\bfseries}p{2.8cm}X X@{}}
\toprule
Graph feature & What changes in the picture? & Twin Hazard example\\
\midrule
Direction & Add arrows, or leave links without arrows. & Downstream drain flow versus two-way pedestrian access.\\
Weights & Add a number or attribute to every link. & Travel time, heat discomfort, pipe slope or capacity.\\
Connectivity & Some nodes may or may not have a route between them. & An isolated block or park; a cut-off path to a cooling centre.\\
Cycles & A route may or may not loop back to its start. & A branching catchment versus a looped street route.\\
Node groups & Links may join any nodes, or only unlike groups. & Neighbourhoods connected to cooling sites (bipartite).\\
Layers & Keep several related networks separate and cross-reference them. & Drainage, walking and green-cover systems in one city twin.\\
\bottomrule
\end{tabularx}
\end{center}

These features are \emph{independent choices}, not mutually exclusive labels. A graph can be directed, weighted, connected and acyclic all at once. For instance, an outlet-bound drain model may be directed and weighted; a park-access model may be undirected and weighted. Standard graph libraries likewise distinguish undirected, directed and parallel-edge graph classes \cite{NetworkXClasses}.

\section{Kinds of graphs}
\subsection{Undirected and directed graphs}

\textbf{Undirected graph:} a link has no arrow. It represents a two-way relationship for the model. If a footpath is usable in both directions, the same edge connects the home to the park and the park to the home. If one direction is inaccessible, a single undirected link is an inaccurate simplification.

\textbf{Directed graph:} every link has an arrow. In a drainage model, an arrow can point from an upstream inlet towards a downstream lake or outlet. If $A\to B$ exists, it does not automatically mean that water can flow back from $B$ to $A$.

\begin{figure}[ht]
\centering
\begin{minipage}{0.47\linewidth}
\centering\textbf{A. Undirected access graph}\\[-2pt]
\begin{tikzpicture}[scale=0.88]
\node[vtx] (H) at (0,0) {H};
\node[vtx] (J) at (1.5,0.9) {J};
\node[greenv] (P) at (3,0) {P};
\node[vtx] (M) at (1.5,-0.9) {M};
\draw[line] (H)--(J)--(P)--(M)--(H);
\draw[line] (J)--(M);
\end{tikzpicture}\\[-1pt]
{\scriptsize Two-way walking links: home H, junction J, park P, market M.}
\end{minipage}\hfill
\begin{minipage}{0.47\linewidth}
\centering\textbf{B. Directed drainage graph}\\[-2pt]
\begin{tikzpicture}[scale=0.88]
\node[bluev] (C1) at (0,0.8) {$C_1$};
\node[bluev] (C2) at (0,-0.8) {$C_2$};
\node[vtx] (J) at (1.6,0) {J};
\node[greenv] (L) at (3.2,0) {L};
\node[goldv] (O) at (4.5,0) {O};
\draw[arrow] (C1)--(J);
\draw[arrow] (C2)--(J);
\draw[arrow] (J)--(L);
\draw[arrow] (L)--(O);
\end{tikzpicture}\\[-1pt]
{\scriptsize Assumed flow: catchments $C_1,C_2$ to junction J, lake L, outfall O.}
\end{minipage}
\caption{Undirected links and directed arrows represent different meanings. Both diagrams are schematic, not to scale.}
\label{fig:directed}
\end{figure}

\textbf{When to use which?} Keep arrows when direction affects reachability: water flow, one-way traffic or a one-direction warning cascade. Leave arrows off when the relationship works both ways. Do not turn a directed network into an undirected one merely to make its picture simpler; that can invent routes that do not exist.

\subsection{Unweighted and weighted graphs}

An \textbf{unweighted} graph records whether a link exists, treating links as equal for the question being asked. A \textbf{weighted} graph attaches values such as metres, minutes, generalized heat cost, flow capacity or ecological resistance. In a weighted graph, the meaning and units of the weight must be written down.

\begin{figure}[ht]
\centering
\begin{minipage}{0.47\linewidth}
\centering\textbf{A. Unweighted: count links}\\[-2pt]
\begin{tikzpicture}[scale=0.9]
\node[vtx] (A) at (0,0) {A};
\node[vtx] (B) at (1.6,0.8) {B};
\node[vtx] (C) at (1.6,-0.8) {C};
\node[goldv] (S) at (3.3,0) {S};
\draw[line] (A)--(B)--(S);
\draw[line] (A)--(C)--(S);
\end{tikzpicture}\\[-1pt]
{\scriptsize Both routes A--B--S and A--C--S use two links.}
\end{minipage}\hfill
\begin{minipage}{0.47\linewidth}
\centering\textbf{B. Weighted: compare link costs}\\[-2pt]
\begin{tikzpicture}[scale=0.9]
\node[vtx] (A) at (0,0) {A};
\node[vtx] (B) at (1.6,0.8) {B};
\node[vtx] (C) at (1.6,-0.8) {C};
\node[goldv] (S) at (3.3,0) {S};
\draw[line] (A)--node[midway,above,sloped,fill=white,inner sep=1pt,font=\scriptsize]{1} (B);
\draw[line] (B)--node[midway,above,sloped,fill=white,inner sep=1pt,font=\scriptsize]{8} (S);
\draw[good] (A)--node[midway,below,sloped,fill=white,inner sep=1pt,font=\scriptsize]{3} (C);
\draw[good] (C)--node[midway,below,sloped,fill=white,inner sep=1pt,font=\scriptsize]{2} (S);
\end{tikzpicture}\\[-1pt]
{\scriptsize Green route costs 3+2=5; upper route costs 1+8=9.}
\end{minipage}
\caption{Same node layout, different information. Here weights are invented generalized walking-cost units to a cooling site S, not observed minutes.}
\label{fig:weighted}
\end{figure}

An unweighted shortest-path search would treat both two-link routes as equal. With the illustrative costs, the lower path costs 5 and the upper path costs 9, so the weighted shortest route changes. If the weight is \emph{capacity}, however, a route-selection method that minimizes weight would be the wrong interpretation; capacity and cost are different edge attributes. Graph libraries support numeric edge attributes such as weights, but the analyst must define what they mean \cite{NetworkXTutorial}.

\subsection{Connected, disconnected and complete graphs}

An undirected graph is \textbf{connected} if every pair of its nodes has a route between them. It is \textbf{disconnected} if it has separate components. A component is a maximal group in which every node can reach every other node. For a city, an isolated node might represent a park with no mapped walkable entrance or a block separated from a shelter by an impassable barrier. Before deciding it is genuinely isolated, check whether the data are missing a link.

\begin{figure}[ht]
\centering
\begin{minipage}{0.47\linewidth}
\centering\textbf{Connected}\\[-2pt]
\begin{tikzpicture}[scale=0.85]
\node[vtx] (a) at (0,0) {a}; \node[vtx] (b) at (1.2,0.8) {b};
\node[vtx] (c) at (2.4,0) {c}; \node[vtx] (d) at (1.2,-0.8) {d};
\draw[line] (a)--(b)--(c)--(d)--(a); \draw[line] (a)--(c);
\end{tikzpicture}\\[-1pt]
{\scriptsize Every pair is joined by some route.}
\end{minipage}\hfill
\begin{minipage}{0.47\linewidth}
\centering\textbf{Disconnected: two components}\\[-2pt]
\begin{tikzpicture}[scale=0.85]
\node[vtx] (a) at (0,0) {a}; \node[vtx] (b) at (1.2,0.8) {b};
\node[vtx] (c) at (3.0,0) {c}; \node[vtx] (d) at (4.2,0.8) {d};
\draw[line] (a)--(b); \draw[line] (c)--(d);
\end{tikzpicture}\\[-1pt]
{\scriptsize No route links the left pair to the right pair.}
\end{minipage}
\caption{Connectivity is about whether paths exist, not how close nodes look on a drawing.}
\end{figure}

A \textbf{complete graph} is the special case in which every pair of distinct nodes has a direct edge. The four-node drawing below is $K_4$. Complete graphs are useful as an idealized benchmark for maximum direct connectivity; they are rarely a realistic picture of actual streets or drains.

\begin{center}
\begin{tikzpicture}[scale=0.65]
\node[vtx] (a) at (0,0) {1}; \node[vtx] (b) at (1.4,0) {2};
\node[vtx] (c) at (0.7,1.2) {3}; \node[vtx] (d) at (0.7,0.42) {4};
\draw[line] (a)--(b)--(c)--(a); \draw[line] (d)--(a); \draw[line] (d)--(b); \draw[line] (d)--(c);
\end{tikzpicture}
\quad
\begin{minipage}{0.57\linewidth}
\small\textbf{Complete $K_4$:} six direct links connect every pair of four nodes. In a planning study it can serve as a theoretical reference, not as a target to build literally.
\end{minipage}
\end{center}

For directed graphs, connectivity has two useful readings: \emph{weakly connected} means connected after ignoring arrow directions; \emph{strongly connected} means every node can reach every other by following arrows. In a one-way chain $A\to B\to C$, the graph is weakly connected but not strongly connected.

\subsection{Trees, cycles and directed acyclic graphs}

A \textbf{tree} is a connected, undirected graph with no cycles. Between any two tree nodes there is exactly one simple route. This can approximate a branching collection or a simplified catchment hierarchy. A \textbf{cycle} is a closed route that returns to its starting node. Cycles in a pedestrian network can supply alternative ways around a closed street, although a cycle by itself does not guarantee accessible or safe alternatives.

A \textbf{directed acyclic graph} (DAG) has arrows but no directed cycle. It can show stages of downstream flow or a sequence of dependencies. A DAG need not be a tree: several branches can merge, or one node can have multiple routes feeding downstream. Conversely, a directed graph may contain cycles where circulation, feedback or pump connections exist.

\begin{figure}[ht]
\centering
\begin{minipage}{0.31\linewidth}
\centering\textbf{Tree}\\[-2pt]
\begin{tikzpicture}[scale=0.72]
\node[vtx] (r) at (1,1.5) {R}; \node[vtx] (a) at (0,0.4) {A}; \node[vtx] (b) at (2,0.4) {B};
\node[vtx] (c) at (-0.4,-0.7) {C}; \node[vtx] (d) at (0.5,-0.7) {D};
\draw[line] (r)--(a)--(c); \draw[line] (a)--(d); \draw[line] (r)--(b);
\end{tikzpicture}\\[-1pt]
{\scriptsize One route between any pair.}
\end{minipage}\hfill
\begin{minipage}{0.31\linewidth}
\centering\textbf{Cycle}\\[-2pt]
\begin{tikzpicture}[scale=0.72]
\node[greenv] (a) at (0,0) {A}; \node[greenv] (b) at (1.5,0) {B};
\node[greenv] (c) at (1.5,1.3) {C}; \node[greenv] (d) at (0,1.3) {D};
\draw[good] (a)--(b)--(c)--(d)--(a);
\end{tikzpicture}\\[-1pt]
{\scriptsize Loop gives an alternate way around.}
\end{minipage}\hfill
\begin{minipage}{0.31\linewidth}
\centering\textbf{DAG}\\[-2pt]
\begin{tikzpicture}[scale=0.72]
\node[bluev] (a) at (0,1.2) {$C_1$}; \node[bluev] (b) at (0,0) {$C_2$};
\node[vtx] (j) at (1.5,0.6) {J}; \node[greenv] (l) at (3,0.6) {L};
\node[goldv] (o) at (4.4,0.6) {O};
\draw[arrow] (a)--(j); \draw[arrow] (b)--(j); \draw[arrow] (j)--(l); \draw[arrow] (l)--(o);
\end{tikzpicture}\\[-1pt]
{\scriptsize Arrows move downstream; no directed loop.}
\end{minipage}
\caption{A tree, a cyclic access network and a directed acyclic flow example. Each is a different structural property.}
\label{fig:treecycleDAG}
\end{figure}

\textbf{Do not assume every real drain network is a tree or a DAG.} The correct type comes from the actual surveyed connections and flow rules. Loops, pumps, gates, backwater or multiple outlets can change the model. One study of eight Dresden sewer subnetworks explicitly represented them as weighted directed acyclic graphs and compared topology-based measures with hydraulic simulation; its results applied to the studied operating conditions, not directly to flood events in Bengaluru \cite{Reyes2020}.

\subsection{Simple graphs and multigraphs}

A \textbf{simple graph} has at most one edge between any pair of nodes (and no self-loop in the usual beginner definition). A \textbf{multigraph} allows parallel edges between the same pair. A \textbf{directed multigraph} can keep multiple one-way links from the same start node to the same end node.

\begin{figure}[ht]
\centering
\begin{minipage}{0.46\linewidth}
\centering\textbf{Simple: one mapped link}\\[-2pt]
\begin{tikzpicture}[scale=0.92]
\node[vtx] (u) at (0,0) {U}; \node[greenv] (v) at (2.8,0) {V};
\draw[line] (u)--(v) node[midway,above,font=\scriptsize]{one abstract edge};
\end{tikzpicture}
\end{minipage}\hfill
\begin{minipage}{0.49\linewidth}
\centering\textbf{Multigraph: two separate culverts}\\[-2pt]
\begin{tikzpicture}[scale=0.92]
\node[vtx] (u) at (0,0) {U}; \node[greenv] (v) at (2.8,0) {V};
\draw[arrow] (u) to[bend left=22] node[midway,above,font=\scriptsize]{pipe 1} (v);
\draw[arrow] (u) to[bend right=22] node[midway,below,font=\scriptsize]{pipe 2} (v);
\end{tikzpicture}
\end{minipage}
\caption{If two different physical pipes link the same junctions, a multigraph preserves their separate identities. Collapsing them into one edge can hide redundancy and maintenance information.}
\label{fig:multi}
\end{figure}

The right-hand picture is a simplified example: distinct culverts between junctions U and V may have different diameters, blockage states or owners. A multigraph preserves those parallel assets as separate links. If the question is only broad reachability, the graph may be simplified; if the question is failure of one pipe, the separate edges matter. NetworkX documents separate graph classes for directed/undirected and parallel-edge cases, and notes that some algorithms require extra care on multigraphs \cite{NetworkXClasses,NetworkXTutorial}.

\subsection{Bipartite and multilayer graphs}

A \textbf{bipartite graph} divides nodes into two groups, with edges only between groups. For heat-service planning, one side can be residential blocks and the other side cooling locations. An edge means that the route is feasible under a stated rule, such as a maximum walking cost. The graph can show which blocks have no reachable option or which cooling location serves many blocks. The standard mathematical form is $B=(U,V,E)$ with no within-group edges \cite{NetworkXBipartite}.

\begin{figure}[ht]
\centering
\begin{tikzpicture}[x=1cm,y=1cm]
\node[facility] (r1) at (0,1.3) {Home block R1};
\node[facility] (r2) at (0,0.0) {Home block R2};
\node[facility] (r3) at (0,-1.3) {Home block R3};
\node[action] (c1) at (5,1.5) {Cooling hub C1};
\node[action] (c2) at (5,0.0) {Library C2};
\node[action] (c3) at (5,-1.5) {Clinic C3};
\draw[line] (r1)--(c1); \draw[line] (r1)--(c2);
\draw[line] (r2)--(c2); \draw[line] (r3)--(c2); \draw[line] (r3)--(c3);
\end{tikzpicture}
\caption{A fictional bipartite coverage graph: left nodes are home blocks, right nodes are cooling services, and links indicate access within the selected threshold. R2 has no feasible link in this example.}
\label{fig:bipartite}
\end{figure}

A \textbf{multilayer graph} keeps networks of different kinds in parallel and adds explicit cross-layer links where their nodes refer to the same place or a real dependency. For example, one layer may contain drainage junctions, another walkable street segments, and another parks or shaded places. A cross-layer link can mark that a street node is near a lake edge, not that a street and lake are physically the same object.

\begin{figure}[ht]
\centering
\begin{tikzpicture}[x=1cm,y=1cm]
\node[font=\small\bfseries,text=river] at (-0.9,1.5) {Drain};
\node[font=\small\bfseries,text=navy] at (-0.9,0) {Walk};
\node[font=\small\bfseries,text=leaf!70!black] at (-0.9,-1.5) {Green};
\node[bluev] (d1) at (0.5,1.5) {$D_1$}; \node[bluev] (d2) at (2.5,1.5) {$D_2$}; \node[greenv] (d3) at (4.5,1.5) {Lake};
\draw[arrow] (d1)--(d2); \draw[arrow] (d2)--(d3);
\node[vtx] (w1) at (0.5,0) {$W_1$}; \node[vtx] (w2) at (2.5,0) {$W_2$}; \node[vtx] (w3) at (4.5,0) {$W_3$};
\draw[line] (w1)--(w2)--(w3);
\node[greenv] (g1) at (0.5,-1.5) {Park}; \node[greenv] (g2) at (2.5,-1.5) {Trees}; \node[greenv] (g3) at (4.5,-1.5) {Lake};
\draw[good,dashed] (g1)--(g2)--(g3);
\draw[aux] (d1)--(w1); \draw[aux] (d2)--(w2); \draw[aux] (d3)--(w3);
\draw[aux] (w1)--(g1); \draw[aux] (w3)--(g3);
\end{tikzpicture}
\caption{Three layers with dashed cross-layer associations. Horizontal links have layer-specific meanings; vertical dashed links record spatial overlap or dependency.}
\label{fig:multilayer}
\end{figure}

\section{Research and real-world lessons}

\textbf{Bengaluru: connected lakes and drains.} The ATREE flood strategy describes the city's traditional lake cascade, with excess water moving through primary stormwater drains (Rajakaluves) to downstream lakes, and recommends integrated lake-level management, drainage maintenance and blue-green measures \cite{ATREE2024}. An IISc report also identifies the loss of lake interconnectivity among flood-related concerns \cite{IISc2017}. For Twin Hazard, this is why a directed drainage graph should preserve upstream/downstream links rather than just mark water bodies as nearby dots. The reports do not mean a conceptual graph alone predicts inundation.

\textbf{Dresden: a measured drainage-network study.} Reyes-Silva and colleagues modelled eight subnetworks of Dresden's sewer system as weighted directed acyclic graphs, using pipe properties and testing graph measures against hydraulic simulations. The study found that appropriate shortest-path weights could help describe transport, while the usefulness of particular centrality measures differed between branched and meshed systems \cite{Reyes2020}. It demonstrates that graph type and edge weight should follow the physical question; it is not a Bengaluru flood forecast.

\textbf{Stormwater resilience: cycles and alternate paths.} Hesarkazzazi and colleagues examined centralized and decentralized stormwater networks, using graph centrality to help place redundant routes. Their modelled case reported resilience gains of up to 8\% under functional failure when redundant paths were added \cite{Hesarkazzazi2022}. The number is specific to their network and assumptions. For a city twin, the transferable idea is to compare a base network with a version that preserves independently usable alternatives.

\textbf{Tehran: green-space connectivity and fairness.} Alavi and colleagues used graph theory and least-cost analysis to examine urban green-space connectivity and ecosystem-service distribution. Their study identified unequal access among districts and possible corridor opportunities in Tehran \cite{Alavi2024}. For Bengaluru, the method suggests modelling both the green patches and who can reach them, while gathering Bengaluru-specific park, street-barrier and community data rather than copying Tehran's corridor pattern.

\section{Quick reference: choose a graph type}

\small
\begin{tabularx}{\linewidth}{@{}>{\bfseries}p{4.2cm}X X@{}}
\toprule
Ask this first & Use this graph property & Example decision\\
\midrule
Does direction matter? & Directed or undirected & Drain flow arrows; two-way footpath links.\\
Do edge costs or capacities differ? & Weighted or unweighted & Heat-adjusted walking cost; pipe attributes.\\
Can parallel links occur? & Simple graph or multigraph & Keep separate culverts when one-pipe failure matters.\\
Can every node reach the others? & Connectedness and components & Find a block with no feasible cooling service.\\
Are there loops or feedback? & Tree, cyclic graph or DAG & Branching outlet route versus alternate street loop.\\
Are there two node groups? & Bipartite & Home blocks linked to reachable cooling facilities.\\
Do network layers interact? & Multilayer graph & Connect drainage, access and green-space layers.\\
\bottomrule
\end{tabularx}
\normalsize

\textbf{A reliable first prototype:} pick one question, map one layer with a few verified nodes and edges, state whether it is directed, state what every weight means, and test one scenario. Then explain what the graph cannot tell you. For flood depth or pipe performance, validate the topology and use hydrologic/hydraulic analysis; for access, field-check barriers, opening hours, shade and universal accessibility. A missing edge in a dataset may be missing information, not a missing real-world connection.

\section{Key idea}

Graph types are visual choices that encode assumptions. Arrows say direction matters; weights say links are not equivalent; disconnected components expose missing reachability; cycles show alternative routes; bipartite and multilayer graphs connect different classes of things without pretending they are the same system. Choose the simplest graph that answers the planning question, then check its assumptions against Bengaluru's real streets, drains, lakes and people.

\renewcommand{\refname}{Sources and further reading}
\addcontentsline{toc}{section}{Sources and further reading}
\begin{thebibliography}{99}
\small
\setlength{\itemsep}{3pt}

\bibitem{GreenPulse}
Green Pulse 2026. \emph{Problem Statement Booklet}, Theme 04: Digital Twin for Sustainable Cities, Question 5. User-provided event brief, 13 November 2026.

\bibitem{ATREE2024}
P. Jamwal, P. Hymavathi, A. Nair, D. Samhitha and V. Suresh. \emph{Building Resilience to Urban Floods: Strategy and Action Plan for Bengaluru}. Ashoka Trust for Research in Ecology and the Environment, March 2024. \url{https://www.atree.org/wp-content/uploads/2025/07/Building-resilience-to-urban-floods-Strategy-and-Action-plan-for-Bengaluru.pdf}

\bibitem{IISc2017}
T. V. Ramachandra, V. S. Bharath and H. Aithal. \emph{Frequent Floods in Bangalore: Causes and Remedial Measures}. ENVIS Technical Report 123, Indian Institute of Science, August 2017. \url{https://wgbis.ces.iisc.ac.in/energy/water/paper/ETR123/contents.html}

\bibitem{Reyes2020}
J. D. Reyes-Silva, J. Zischg, C. Klinkhamer, P. S. C. Rao, R. Sitzenfrei and P. Krebs. ``Centrality and Shortest Path Length Measures for the Functional Analysis of Urban Drainage Networks.'' \emph{Applied Network Science}, 5, article 1, 2020. \url{https://doi.org/10.1007/s41109-019-0247-8}

\bibitem{Hesarkazzazi2022}
S. Hesarkazzazi, A. E. Bakhshipour, M. Hajibabaei, U. Dittmer, A. Haghighi and R. Sitzenfrei. ``Battle of Centralized and Decentralized Urban Stormwater Networks: From Redundancy Perspective.'' \emph{Water Research}, 222, 118910, 2022. \url{https://doi.org/10.1016/j.watres.2022.118910}

\bibitem{Alavi2024}
S. A. Alavi, S. Esfandi, A. R. Khavarian-Garmsir, S. Tayebi, A. Shamsipour and A. Sharifi. ``Assessing the Connectivity of Urban Green Spaces for Enhanced Environmental Justice and Ecosystem Service Flow: A Study of Tehran Using Graph Theory and Least-Cost Analysis.'' \emph{Urban Science}, 8(1), 14, 2024. \url{https://doi.org/10.3390/urbansci8010014}

\bibitem{Simone2022}
A. Simone, A. Cesaro, G. Del Giudice, C. Di Cristo and O. Fecarotta. ``Potentialities of Complex Network Theory Tools for Urban Drainage Networks Analysis.'' \emph{Water Resources Research}, 58, e2022WR032277, 2022. \url{https://doi.org/10.1029/2022WR032277}

\bibitem{NetworkXClasses}
NetworkX Developers. \emph{Graph Types: Graph, DiGraph, MultiGraph and MultiDiGraph}. NetworkX documentation. \url{https://networkx.org/documentation/stable/reference/classes/index.html}

\bibitem{NetworkXTutorial}
NetworkX Developers. \emph{Tutorial: Graph Attributes, Directed Graphs and Multigraphs}. NetworkX documentation. \url{https://networkx.org/documentation/stable/tutorial.html}

\bibitem{NetworkXBipartite}
NetworkX Developers. \emph{Bipartite Graphs}. NetworkX documentation. \url{https://networkx.org/documentation/stable/reference/algorithms/bipartite.html}
\end{thebibliography}

\end{document}
