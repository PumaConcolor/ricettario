# Breadth-First Search (BFS)

## Descrizione

Il **BFS** è uno degli algoritmi più semplici per la ricerca su un grafo, e un
archetipo per molti algoritmi sui grafi orientati e non.

Dato un grafo della forma $G=(V,E)$, e un **vertice sorgente** $s$, il BFS
esplora sistematicamente gli archi $E$ di $G$ per trovare ogni vertice
raggiungibile a partire da $s$. Inoltre esso computa la distanza tra $s$ ed ogni
vertice raggiungibile da esso, e produce un **Breadth-First Tree** di radice
$s$ che contiene tutti i vertici raggiungibili. Per ogni vertice $v$ nel BFT, il
percorso da $s$ a $v$ è un "percorso più breve", ovvero quello che contiene il
minor numero di archi.

Il BFS si chiama così perché espande uniformemente la frontiera dei vertici
scoperti, ovvero scopre tutti i vertici a distanza $k$ da $s$ prima di scoprire
quelli a distanza $k+1$.

Per tenere traccia dei progressi, il BFS "colora" ogni vertice di bianco, grigio
o nero: tutti i vertici sono colorati inizialmente di bianco, vengono colorati
di grigio quando parzialmente esplorati e, infine, colorati di nero quando
completamente esplorati. Per esplorare i vertici adiacenti si utilizza la
matrice o la lista di adiacenza associata al grafo.

Il BFS usa una serie di variabili di supporto, tra cui $color[u]$ e $\pi(u)$
(predecessore). La distanza tra $s$ e il vertice $u$ inoltre è salvata in
$d[u]$. Per gestire i vertici grigi inoltre, l'algoritmo usa una coda FIFO $Q$.

## Algoritmo

```pascal
procedure BFS(G, s)
	color[s] := GRAY
	d[s] := 0
	π[s] := NIL
	for vertex u in V[G]-{s} do
		color[u] := WHITE
		d[u] := ∞
		π[u] := NIL
	end
	ENQUEUE(Q, s)
	Q = {}
	while Q != {} do
		u := DEQUEUE(Q)
		for vertex v in ADJ(u) do
			if color[v] = WHITE then
				color[v] := GRAY
				d[v] := d[u] + 1
				π[v] := u
				ENQUEUE(Q, v)
			end
		end
		color[u] := BLACK
	end
end

```

## Funzionamento

Simulazione del funzionamento di un algoritmo BFS su un grafo non orientato, a
partire dal vertice $s$.

![image-20200207181925585](img\BFSandDFS_BFS_Funzionamento_1.png)

# Depth-First Search (DFS)

## Descrizione

La strategia impiegata dall'algoritmo DFS è di andare a cercare nel grafo in
profondità quando possibile. Nella ricerca DFS, gli archi vengono esplorati
partendo dall'ultimo vertice $v$ che ha ancora archi inesplorati. Quando tutti
gli archi sono stati esplorati, l'algoritmo "torna indietro" per esplorare gli
archi uscenti da $v$. Il processo continua finché tutti i vertici raggiungibili
a partire dal vertice di partenza $s$. Se ci sono altri vertici non esplorati,
uno di essi è selezionato come nuova sorgente e la ricerca viene ripetuta a
partire da quel vertice. Il processo termina quando non rimangono vertici
inesplorati.

Come nella ricerca BFS, quando un vertice $v$ viene scoperto durante la
scansione della lista di adiacenza di un vertice $u$ già scoperto, esso
viene registrato come $\pi[v]=u$.

Al contrario della ricerca BFS, il cui sotto-grafo dei predecessori forma un
albero, il sotto-grafo dei predecessori prodotto dalla ricerca DFS può essere
composto da vari alberi, poiché la ricerca può essere ripetuta partendo da
varie sorgenti.

Oltre a creare foreste, la ricerca DFS imposta anche una **timestamp** per
ogni vertice: ogni vertice ha due timestamp, il tempo di **discovery** $d[v]$
e il tempo di **fine** della ricerca sulla lista di adiacenza $f[v]$. Per ogni
vertice $v$, quindi, vale la relazione $d[u]\lt f[u]$. Le timestamp sono
utilizzate in vari algoritmi sui grafi e sono generalmente utili per capire
il funzionamento della ricerca DFS.

Il vertice $u$ è bianco prima del tempo $d[u]$, grigio tra $d[u]$ e $f[u]$ e
nero dopo $f[u]$.

## Algoritmo

```pascal
procedure DFS(G)
	for vertex v in V[G] do
		color[u] := WHITE
		π[u] := NIL
	end
	time := 0
	for vertex u in V[G] do
		if color[u] = WHITE then
			DFS-VISIT(u)
		end
	end
end
```

```pascal
procedure DFS-VISIT(u)
	color[u] := GRAY	// Il vertice bianco u e' stato scoperto
	time := time + 1
	d[u] := time
	for vertex v in ADJ(u) do	// Esploro tutti gli archi (u, v)
		if color[v] = WHITE then
			π[v] := u
			DFS-VISIT[v]
		end
	end
	color[u] := BLACK	// La visita di u e' terminata
	f[u] := time
	time := time + 1
end
```

## Funzionamento

![image-20200208012110018](img\BFSandDFS_DFS_Funzionamento_1.png)