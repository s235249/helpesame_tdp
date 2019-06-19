# Guida all'esame

## Ricorsione

---

- Dichiarare parametri da salvare eventualmente nel model (liste, interi, stringhe etc...) _esempio: se mi volessi salvare il punteggio massimo in una ricorsione dichiaro un int punteggiomax_;
- Creo una funzione che richiama la ricorsione stabilendo certi parametri, come range o massimi, è una void o eventuali liste parziali, usate per fare i "calcoli" della ricorsione;
- Creo l'algoritmo ricorsivo vero e proprio, all'interno di questa funzione devono essere presenti in ordine:
  - Controllo di validità della soluzione (usando funzioni definite private ad esempio per non appesantire la funzione)
  - Controllo se ho ottenuto una soluzione ottima ed in caso la aggiorno
  - Itero l'algoritmo aggiornando i parametri (livello, eventuali liste etc)

> (Fare riferimento al progetto "lab07" per un esempio)

## Grafi

---

- Creo l'oggetto grafo

  ```java
  this.grafo = new SimpleDirectedGraph<>(DefaultWeightedEdge.class);
  ```

  I parametri "SimpleDirectedGraph" e "DefaultWeightedEdge" sono regolabili in base al bisogno, il grafico può essere anche solo "SimpleGraph" e l'arco "DefaultEdge"

- Aggiungo i vertici al grafico

  ```java
  //1. Richiamo il DAO per avere la lista dei vertici
  //2. Salvo i vertici in una ID map che conservo nel model
  Graphs.addAllVertices(this.grafo, this.listavertici);
  ```

  Un modo intelligiente per aggiungere i vertici e creare una loro "idMap" (una hashmap con chiave primaria un id che li rappresenta) per poterli ricavare poi più facilmente

- Aggiungo gli archi

  ```java
  for (Fermata partenza : this.grafo.vertexSet()) {
    List<Fermata> arrivi = dao.stazioniArrivo(partenza, fermateIdMap);
    for (Fermata arrivo : arrivi)
      this.grafo.addEdge(partenza, arrivo);
  }
  ```

  Questo è solo un esempio a partire da una tabella in cui gli archi sono esplicitati da una tabella specifica, dipende dal database

- FACOLTATIVO aggiungere i pesi agli archi

  ```java
  List<ConnessioneVelocita> archipesati = dao.getConnessioneVelocita(); //Creo una lista con oggetti che associano un arco al suo peso, può essere fatta in vari modi
  for (ConnessioneVelocita cp : archipesati) {
    Fermata partenza = fermateIdMap.get(cp.getStazP());
    Fermata arrivo = fermateIdMap.get(cp.getStazA());
    double distanza = LatLngTool.distance(partenza.getCoords(), arrivo.getCoords(), LengthUnit.KILOMETER);
    double peso = distanza / cp.getVelocita() * 3600;
    grafo.setEdgeWeight(partenza, arrivo, peso);
    }
  ```

- Dopodiché rispondo alle domande poste dall'esame, in "metroparis" ci sono alcuni esempi per vedere i nodi raggiungibili, il cammino minimo con Dijkstra

> Fare riferimento al progetto "metroparis" per un esempio

## Simulazioni

---

- Creo nel model una funzione "simula" che passa parametri non prestabiliti da adoperare nella simulazione, questa avrà poi una sua classe
- Oltre alla classe "Simulatore" creo anche la classe "Evento", nell'evento dovrò salvare tutti i dati che riguardano un evento specifico (come ora di inizio e di fine, numero di persone partecipanti, etc...), nel simulatore invece devo dichiarare:

  - I dati che mi serviranno per determinare lo stato del sistema;
  - I parametri della simulazione;
  - I valori in output
  - La coda

    ```java
    private PriorityQueue<Evento> queue;
    ```

dopodiché procedo con il creare due funzioni, una "init" che inizializza lo stato del sistema ed aggiunge gli eventi alla coda, l'altro "run" che passa in rassegna la coda

```java
Evento e;
  while ((e = queue.poll()) != null) {
    switch (e.getTipo()) {
```

analizzando i diversi tipi di evento ad aggiungendone alcuni di nuovi.
_Risulta molto utile usare un "enum" nella classe evento per determinare la tipologia di eventi possibili, riconosciuti poi dal "run" nel momento in cui vengono presi in esame_

> (Fare riferimento al progetto "2018-09-12")
