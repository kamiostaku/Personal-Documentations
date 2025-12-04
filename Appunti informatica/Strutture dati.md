# Strutture Dati
Le **strutture dati** sono modi in cui conservare informazioni, sono utili perché permettono di definire il modo in cui accediamo ai dati conservati nella memoria. Dunque conviene fare una buona scelta di quale struttura utilizzare dato che può significativamente incidere sulla velocità del programma.

Le strutture dati si dividono principalmente in:
- **Primitive**: Tipi di dati elementari predefiniti in un linguaggio di programmazione. Es. int, float, void*
- **Astratte**: Strutture di dati costruite con l'ausilio di strutture primitive. Es. Grafi, Alberi, Liste concatenate, Liste, array ecc...

Si possono inoltre dividere in base al tipo di contenuti:
- **Omogenee**: Strutture che contengono solo un tipo di dato che può essere un'oggetto, una struttura dati o un tipo. Es. Array, Liste, Code, Stack, Dizionari ecc...
- **Non omogenee**: Strutture che contengono tipi diversi di dati. Es. Struct ecc...

Un'ultima distinzione che si potrebbe fare è sulla loro dimensione:
- **Statiche**: Hanno una dimensione fissa che non cambierà mai durante l'esecuzione. Es. Array
- **Dinamiche**: Hanno dimensioni che possono variare in base al numero di alimenti che vengon aggiunti. Es. Liste, Code, Stack ecc...


<br><br>

## Array
Un **array** (o vettore) sono una **struttura dati astratta**, **omogenea** e **statica**. Hanno una **complessità di accesso di O(1)** visto che il loro punto forte è l'accesso indicizzato. Questo punto forte però è uno **svantaggio nel caso in cui si ricerchi un'elemento** in quanto nel peggior scenario dovresti arrivare all'ultimo elemento dunque **O(n)**. Per poterne cambiare la dimensione è necessario fare una copia del vettore e copiare tutti i dati dal primo in un secondo. Di seguito trovate un esempio di resize e uso di un vettore in C#.

```C#
        static void Main(string[] args)
        {
            int size = 5;

            //Ci sono diversi metodi di inizializzare dei vettori. Uso int come tipo solo per semplicità si potrebbe usare qualsiasi tipo.

            int[] vettoreA = new int[size]; // Vengono allocati N celle inzializzate a 0 per gli int, '' per i char, "" per le stringhe e null gli oggetti

            int[] vettoreB = new int[] { 1, 2, 3 }; // Vengono allocate N celle per il numero di elementi che gli verranno inseriti


            Console.WriteLine(vettoreB[2]); // Stampera la seconda posizione del vettore dunque il numero 3


            //Questo for istanzierà ogni elemento del vettoreA al proprio indice+1, si potrebbe anche usare foreach ed evitare di usare l'indice.
            for(int i = 0; i < vettoreA.Length; i++)
            {
                vettoreA[i] = i+1;
            }

            //Еsempio resize
            //Faccio size*2 perchè è il modo più semplice per aumentare lo spazio.
            int[] vettoreC = new int[size*2];

            //Questo for effettua la copia dei dati nel vettoreC
            for(int i = 0; i < size; i++)
            {
                vettoreC[i] = vettoreA[i];
            }

            //Come potete notare se vogliamo immagazzinare 10 dati in un vettore con inizialmente 5 dati siamo costretti a usare un vettore con un'altra size e ciò rende i vettori statici.
        }
```

<br><br>

## Lists
Le **list (o liste)** rappresentano un’alternativa più flessibile agli array, poiché permettono di aggiungere o rimuovere elementi senza dover stabilire una dimensione in anticipo.

Le liste hanno 3 principali implementazioni:
- **Array dinamico**: funziona come un array ma con ridimensionamento automatico quando lo spazio non basta più. In questo caso l’accesso agli elementi tramite indice resta diretto.

- **Lista concatenata**: ogni elemento contiene il dato e un riferimento al successivo. Gli elementi non sono contigui in memoria e per accedere a uno specifico nodo bisogna scorrere quelli precedenti.

- **Lista doppiamente concatenata**: simile alla precedente, ma ogni elemento ha anche un riferimento al nodo precedente. Questo semplifica alcune operazioni di inserimento e rimozione.

Le liste offrono vari metodi pronti all’uso, tra cui:
- **Add(elemento)**: per inserire un elemento in coda. Ha complessita O(1) ammortizzato.
- **Insert(indice, elemento)**: per aggiungerne uno in una posizione specifica. Ha complessita O(N).
- **AddRange(struttura dati)**: aggiunge tutti gli elementi di un’altra collection. Ha complessita O(N+М).
- **Remove(elemento)**: per eliminare la prima occorrenza di un elemento. Ha complessita O(n).
- **Clear()**: per svuotare completamente la lista. Ha complessita O(n).

Di seguito un'esempio di codice con le liste in C#:

```C#
        static void Main(string[] args)
        {
            int size = 5;

            //Uso int come esempio ma si potrebbe usare qualsiasi tipo come per i vettori.

            //Diversi modi di inizializzare le liste:
            List<int> listA = new List<int>(); //Crea una lista con capacità 0

            List<int> listB = new List<int>(size); //Crea una lista con capacità N

            List<int>[] vettListA = new List<int>[size]; //Crea un vettore contenene N liste che vanno istanziate

            //Metodo per istanziare un vettore di liste:
            for(int i = 0; i < size; i++)
            {
                vettListA[i] = new List<int>(size);
            }

            List<int>[] vettListB = new List<int>[] { new List<int>(), new List<int>() }; //Crea un vettore con 2 liste istanziate

            List<List<int>> vettListC = new List<List<int>>(); //Si può creare anche una lista di liste.
        }
```

---

### Liste in C#

Le liste **in C#** sono **implementate** tramite **array dinamico** che può aumentare e ridurre la propria capacity. Offrono un'accesso indicizzato con O(1).
In C# hanno 4 campi/proprietà principali:
- **_items**: è il vettore contenente i dati.
- **_size**: Indica il numero di elementi aggiunti nel vettore, di conseguenza anche dove andare ad inserire il prossimo elemento.
- **Capacity**: Indica il numero di elementi aggiungibili.
- **Indexer**: Permette l'accesso diretto tramite indice.

Di seguito analizzeremo un paio di metodi:

#### Add(elemento)
```C#
    public void Add(T item)
    {
        if (_size == Capacity) // Punto 1
        {
            Grow(); // Punto 1.1
        }

        _items[_size] = item; // Punto 2
        _size++; // Punto 3
    }
```
1. Controlliamo se abbiamo ancora spazio per contenere l'elemento che vogliamo aggiungere.
   1. Se non abbiamo abbastanza spazio usiamo Grow() che raddoppia la capacity del vettore.
2. Aggiungiamo l'elemento nella posizione _size
3. Aumentiamo _size in modo da puntare alla prossima cella disponibile 

#### Insert(indice, elemento)
```C#
    public void Insert(int index, T item)
    {
        if (index > _size) // Punto 1
            throw new ArgumentOutOfRangeException("index");

        if (_size == Capacity) // Punto 2
        {
            Grow(); // Punto 2.1
        }

        if (index < _size) // Punto 3
        {
            Array.Copy(_items, index, _items, index + 1, _size - index); //Punto 3.1
        }

        _items[index] = item; // Punto 4
        _size++; // Punto 5
    }
```
1. Controllo che l'indice inserito sia contenuto nello spazio occupato del vettore
2. Controlliamo se abbiamo ancora spazio per contenere l'elemento che vogliamo inserire .
   1. Se non abbiamo abbastanza spazio usiamo Grow() che raddoppia la capacity del vettore.
3. Controlliamo se l'indice è compreso all'interno dello spazio occupato.
   1. Spostiamo la lista di una cella in modo da liberare l'indice in cui vogliamo inserire l'elemento senza perdere quello precedetene.
4. Aggiungiamo nella posizione dell'indice l'elemento.
5. Incrementiamo _size in modo da puntare alla prossima cella disponibile.

#### RemoveAt(indice)
```C#
    public void RemoveAt(int index)
    {
        if (index >= _size) //Punto 1
            throw new ArgumentOutOfRangeException("index");

        _size--; //Punto 2
        if (index < _size) //Punto 3
        {
            Array.Copy(_items, index + 1, _items, index, _size - index); //Punto 3.1
        }

        _items[_size] = default(T); //Punto 4
    }
```
1. Controlliamo che l'indice selezionato sia utilizzato all'interno di size
2. Decrementiamo size
3. Controlliamo che l'elemento sia contenuto in size
   1. Copiamo il vettore su se stesso saltando la posizione di index così da sovreasscriverlo
4. Ripuliamo l'ultima posizione (_size punta sempre a una posizione vuota).

#### TrimExcess()
Cito questo metodo senza analizzarne il codice per fornire un'esempio di riduzione del vettore in quanto questo metodo riduce la Capacity a _size.

---

<br><br>

## Queue
La **Queue (o coda)** è una struttura dati **astratta**, **omogenea** e solitamente **dinamica** che segue l'ordine **FIFO**(First In First Out): il primo elemento inserito è il primo ad essere rimosso. Per facilitarne la gestione si usa una **head** - la posizione dell'elemento da rimuovere con un'operazione di **dequeue** O(1) - e una **tail** - la posizione dell'elemento aggiunto con una operazione di **enqueue** O(1) ammortizzato - .\
Le implementazioni sono svariate, ma tra le più comuni ci sono quella con le linked list e quella che utilizza un vettore circolare. In questo caso gli indici di head e tail avanzano tramite aritmetica modulare, permettendo di riutilizzare lo spazio libero. Quando la tail raggiunge la fine del vettore e si tenta di inserire un nuovo elemento, prima di effettuare un’eventuale resize si verifica, tramite il calcolo modulare, se esistono celle libere nelle posizioni iniziali del vettore.

Un'esempio di codice con le code in C#:
```C#
    static void Main(string[] args)
    {
        int size = 5;
        //Uso int come esempio int ma si potrebbe usare qualsiasi tipo come per le liste.

        //Diversi modi di inizializzare le code:
        Queue<int> queueA = new Queue<int>(); //Crea una coda con capacità 0

        Queue<int> queueB = new Queue<int>(size); //Crea una coda con capacità N

        Queue<int>[] vettQueueA = new Queue<int>[size]; //Crea un vettore contenente N code che vanno istanziate
        
        //Metodo per istanziare un vettore di code:
        for(int i = 0; i < size; i++)
        {
            vettQueueA[i] = new Queue<int>(size);
        }
        
        Queue<int>[] vettQueueB = new Queue<int>[] { new Queue<int>(), new Queue<int>() }; //Crea un vettore con 2 code istanziate

        Queue<Queue<int>> queueQueueC = new Queue<Queue<int>>(); //Si può creare anche una coda di code.
    }
```

---

### Implementazione in C#
Le **code** come le liste sono implementate tramite un array dinamico. Ha 5 campi/proprietà principali che sono:
- **_аrray** = Il vettore che contiene i dati.
- **_head** = L'indice dell'elemento da rimuovere.
- **_tail** = L'indice di dove aggiungere il prossimo elemento.
- **Count** = Il numero di elementi attuali contenuti nel vettore.
- **Capacity** = La capienza totale del vettore.

All'istanza di un coda avverrà questo:
```C#
    static void Main(string[] args)
    {
        Queue<int> queueA = new Queue<int>(); 
        /*  
         *  Crea una coda con:
         *  Capacity = 0, 
         *  Count = 0,
         *  head = 0,
         *  tail = 0
         */

        queueA.Enqueue(5); // Aggiunge un elemento alla coda

        /*  
         *  Il metodo Enqueue (dunque aggiunge un'elemento) controlla se Capacity è uguale a Count. In questo caso sono entrambi a 0 e di conseguenza
         *  dopo aver fatto un calcolo setta la Capacity a un numero default di 4 e aumenta la tail di 1.
         */
    }
```
Di seguito analizzeremo un paio di metodi:
#### Enqueue(elemento)
```C#
    public void Enqueue(T item)
    {
        if (Count == Capacity) // Punto 1
        {
            Grow(); // Punto 1.1
        }

        _array[tail] = item; // Punto 2
        tail = (tail + 1) % Capacity; // Punto 3
        Count++; // Punto 4
    }
```
1. Controlla se gli elementi presenti nel vettore(Count) sono tanti quanta la capacità massima del vettore
   1. In caso fa crescere la coda che raddoppia la Capacity e ricopia gli elementi
2. Inseriamo l'elemento nella coda
3. Cambiamo il valore della coda tramite un calcolo modulare, ovverosia aumentiamo la tail di 1 - visto che abbiamo aumentato di 1 elemento - e facciamo il modulo del risultato. Questa operazione è fatta in quanto il modulo permette di "riavvolgere" il vettore. Es. se abbiamo una coda con capacity = 4, Count = 2 е tail = 3 facendo (3+1)%4 torniamo alla posizione 0 del vettore e è questa operazione a rendere il vettore circolare. **Consiglio**: Rivedersi il calcolo del resto in quanto il modulo si basa su quello.
4. Incrementiamo Count in quanto abbiamo aggiunto un'elemento alla coda.

#### Dequeue(elemento)
```C#
    public T Dequeue()
    {
        if (Count == 0) //Punto 1
            throw new InvalidOperationException("Coda vuota"); // Punto 1.1
            
        T value = _array[head]; // Punto 2
        head = (head + 1) % Capacity; // Punto 3
        Count--; // Punto 4

        return value; // Punto 5
    }
```
1. Prima di rimuovere l'elemento controllo se effettivamente ci sono degli elementi da rimuovere.
   1. Se non ci sono elementi lancio un'eccezione
2. Creo un nuovo elemento che varrà quanto l'elemento da rimuovere
3. Aumento la head con la stessa logica del calcolo modulare (rivedere punto 3 di Enqueue)
4. Decremento Count in quanto abbiamo rimosso un'elemento alla coda.
5. Restituisco l'elemento rimosso

#### Resize()
```C#
    public void Resize()
    {
        int num = _array.Length * 2; // Punto 1
        if (num < _array.Length + 4) // Punto 2
        {
            num = _array.Length + 4; // Punto 2.1
        }

        SetCapacity(num); // Punto 3
    }
```
1. Raddoppio la Capacity del vettore
2. Controllo che sia maggiore 4
   1. In caso contrario assegno Capacity+4 a num
3. Assegno num alla capacity che si occuperà di fare la resize vera e propria e inizializzerà head,tail e Count a 0 in quanto la copia avverrà partendo da head e non da 0

Еsempio:
```C#
    static void Main(string[] args)
    {
        Queue<char> coda = new Queue<char>() {'C','D','A','B'}; 

        //Si ipotizzi head = 2, tail = 2, Count = 4 e Capacity = 4

        coda.Enqueue('E');

        //Diventerebbe: ['A','B','C','D','E',_,_,_] con head = 0, tail = 5, Count = 5 e Capacity = 8
    }
```
> [!NOTE]
>  Il metodo resize di base non esiste ed è implementato direttamente con Enqueue() cito il codice per puri scopi didattici

---
<br><br>

## Stack
Lo **Stack (o Pila)** è una **struttura dati astratta**, **omogenea** e solitamente **dinamica**. La sua caratteristica principale è che segue l'ordine **LIFO** (Last In First Out). L'implementazione di questa caratteristica porta all'esistenza della **"head"**, ovverosia l'indice del prossimo elemento da rimuovere. Ha come metodi principali la **Pop  ( O(1) )** , la rimozione di un elemento, e la **Push ( O(1) ammortizzato )**, l'inserimento di un nuovo elemento.

Un'esempio di codice con le pile in C#:
```C#
static void Main(string[] args)
    {
        int size = 5;
        //Uso int come esempio ma si potrebbe usare qualsiasi tipo come per le liste.

        //Diversi modi di inizializzare gli stack:
        Stack<int> stackA = new Stack<int>(); //Crea uno stack con capacità 0

        Stack<int> stackB = new Stack<int>(size); //Crea uno stack con capacità N

        Stack<int>[] vettStackA = new Stack<int>[size]; //Crea un vettore contenente N stack che vanno istanziati
        
        //Metodo per istanziare un vettore di stack:
        for(int i = 0; i < size; i++)
        {
            vettStackA[i] = new Stack<int>(size);
        }
        
        Stack<int>[] vettStackB = new Stack<int>[] { new Stack<int>(), new Stack<int>() }; //Crea un vettore con 2 stack istanziati

        Stack<Stack<int>> stackStackC = new Stack<Stack<int>>(); //Si può creare anche uno stack di stack.
    }
```
---
### Implementazione in C#
In C# l'implementazione avviene tramite array dinamico e ha come campi/proprietà:
- **_array** = L'array in cui i dati verranno immagazzinati.
- **_size** = Indice di elementi presenti e di conseguenza dove inserire il prossimo, ha ruolo di head.
- **Capacity** = Dimensione totale del vettore.
- **Top eleemnt** = Elemento a cui si può accedere tramite Peek().
  
Di seguito analizzeremo un paio di metodi:

#### Push(elemento)
```C#
    public void Push(T item)
    {
        if (_size == _array.Length) //Punto 1
        {
            Grow(); // Punto 1.1
        }

        _array[_size] = item; // Punto 2
        _size++; // Punto 3
    }
```
1. Controllo che ci sia ancora spazio nel vettore.
   1. Se non c'è ne fosse più aumento la dimensione.
2. Aggiungo l'elemento in posizione _size.
3. Incremento _size così che possa puntare alla prossima cella libera.

#### Pop()
```C#
    public T Pop()
    {
    if (_size == 0) // Punto 1
        throw new InvalidOperationException("Pila vuota"); // Punto 1.1

    _size--;  // Punto 2
    T value = _array[_size]; // Punto 3
    _array[_size] = default(T); // Punto 4

    return value; // Punto 5
}
```
1. Controllo che ci siano effettivi elementi da rimuovere
   1. Se non c'è ne fossero più lancio un'eccezzione
2. Diminuisico _size di uno
3. Mi salvo l'ultimo valore presente nella stack
4. Pulisco la posizione per prepararla in caso dovessi aggiungere un'altro elemento
5. Restituisco il valore precedentemente rimosso

#### Resize()
```C#
    public void Resize()
    {
        T[] array = new T[(_array.Length == 0) ? 4 : (2 * _array.Length)]; // Punto 1
        Array.Copy(_array, 0, array, 0, _size); // Punto 2
        _array = array; // Punto 3
    }
```
1. Creo un nuovo vettore controllando che se il vettore ha lunghezza di 0 valga 4 altrimenti raddoppio la lunghezza del vettore
2. Copio la coda precedente in quella nuova
3. Imposto il vettore al nuovo.

Еsempio:
```C#
    static void Main(string[] args)
    {
        Stack<char> pila = new Stack<char>() {'A','B','C'}; 

        //Si ipotizzi _size = 3, Capacity = 3

        pila.Push('D');

        //Diventerebbe: ['A','B','C','D',_,_,_] con _size = 4, Capacity = 6
        //Il vettore mantiene l'ordine e size punta sempre alla prima cella vuota
    }
```
> [!NOTE]
>  Il metodo resize di base non esiste ed è implementato direttamente con Push() cito il codice per puri scopi didattici

---

<br><br>

## Dictionary
Il **Dictionary (o dizionario)** è una **struttura dati astratta**, **Omogenea (nonostante sia chiave, valore persiste nei suoi due tipi)** e solitamente **Dinamica**. La sua caratteristica principale è l'associazione chiave-valore. Proprio come in un array normale permette l'accesso tramite indice, in questo caso chiamato chiave, senza però limitarne il tipo in quanto la chiave può essere una stringa, oggetto o qualsiasi altro tipo di dato. Ognuna di queste deve però essere univoca nel dizionario e per garantire ciò si usa una **tabella Hash** (guardare la nota) contenente l'hash della chiave che permette di generare un valore univoco associato alla chiave. Nel caso più chiavi generino lo stesso hash C# rende il valore una lista dell'elemento precedente. Queste caratteristiche permetto un accesso con complessità O(1).

>[!NOTE]
> Una tabella hash è una struttura dati usata per mettere in corrispondenza una data chiave con un dato valore. Viene usata per l'implementazione di strutture dati astratte associative come le mappe o i set. 

Metodi principali in C#:
- **Add(chiave, valore)**: Aggiunge una coppia chiave - valore.
- **dictionary[chiave]** = valore: Aggiunge o aggiorna un valore.
- **Remove(chiave, valore)**: Rimuove la coppia chiave - valore
- **Clear()**: Rimuove tutte le coppie dal dizionario
- **ContainsKey(chiave)**: Verifica se una chiave esiste (O(1)).
- **ContainsValue(valore)**: Verifica se un valore esiste (O(n)).

Un'esempio di codice cone i Dictionary in C#:
```C#
    static void Main(string[] args)
    {
        int size = 5;
        //Uso <int, string> come esempio ma si possono usare qualsiasi tipo per chiave e valore.
        //Diversi modi di inizializzare i Dictionary:
        Dictionary<int, string> dictionaryA = new Dictionary<int, string>(); //Crea un Dictionary vuoto
        Dictionary<int, string> dictionaryB = new Dictionary<int, string>(size); //Crea un Dictionary con capacità iniziale N (ottimizzazione)
        Dictionary<int, string>[] vettDictionaryA = new Dictionary<int, string>[size]; //Crea un vettore contenente N Dictionary che vanno istanziati
        
        //Metodo per istanziare un vettore di Dictionary:
        for(int i = 0; i < size; i++)
        {
            vettDictionaryA[i] = new Dictionary<int, string>();
        }
        
        Dictionary<int, string>[] vettDictionaryB = new Dictionary<int, string>[] { new Dictionary<int, string>(), new Dictionary<int, string>() }; //Crea un vettore con 2 Dictionary istanziati
        Dictionary<int, Dictionary<int, string>> dictionaryDictionaryC = new Dictionary<int, Dictionary<int, string>>(); //Si può creare anche un Dictionary di Dictionary
        
        //Puoi anche inizializzare con elementi:
        Dictionary<int, string> dictionaryD = new Dictionary<int, string>()
        {
            { 1, "uno" },
            { 2, "due" },
            { 3, "tre" },
            { 4, "quattro" },
            { 5, "cinque" }
        };
        
        //Oppure con la sintassi degli indicizzatori (C# 6.0+):
        Dictionary<int, string> dictionaryE = new Dictionary<int, string>()
        {
            [1] = "uno",
            [2] = "due",
            [3] = "tre"
        };
        
        //NOTA: A differenza di HashSet, se provi ad aggiungere una chiave duplicata viene lanciata un'eccezione
        //quindi non puoi inizializzare da un array con chiavi duplicate come con HashSet
    }
```

---

<br><br>

## HashSet
Un HashSet è una **struttura dati astratta**, **Omogenea** e solitamente **Dinamica**. Non ha un'ordine specifico, è priva di duplicati e supporta operazioni insiemistiche.

Metodi principali in C#:
- **Add(elemento)** = Aggiunge un'elemento alla hashmap. Ha complessità O(1) ammortizzato.
- **Remove(elemento)** = Rimuove un'elemento dalla hashmap. Ha complessità O(1).
- **Contains(elemento)** = Controlla se un'elemento è presente. Ha complessità O(1).
- **Clear()** = Rimuove ogni elemento dal HashSet. Ha complessità O(N).
- **UnionWith(struttura dati)** = Unisce una struttura dati al HashSet. Ha complessità O(N).
- **IntersectWith(struttura dati)** = Contiene solo gli elementi in comune in se e nella struttura passata come parametro. Ha complessità O(N + М).

Un'esempio di codice con lo HashSet in C#:
```C#
    static void Main(string[] args)
    {
        int size = 5;
        //Uso int come esempio ma si potrebbe usare qualsiasi tipo come per le liste.

        //Diversi modi di inizializzare gli HashSet:
        HashSet<int> hashSetA = new HashSet<int>(); //Crea un HashSet vuoto

        HashSet<int> hashSetB = new HashSet<int>(size); //Crea un HashSet con capacità iniziale N (ottimizzazione)

        HashSet<int>[] vettHashSetA = new HashSet<int>[size]; //Crea un vettore contenente N HashSet che vanno istanziati
        
        //Metodo per istanziare un vettore di HashSet:
        for(int i = 0; i < size; i++)
        {
            vettHashSetA[i] = new HashSet<int>();
        }
        
        HashSet<int>[] vettHashSetB = new HashSet<int>[] { new HashSet<int>(), new HashSet<int>() }; //Crea un vettore con 2 HashSet istanziati

        HashSet<HashSet<int>> hashSetHashSetC = new HashSet<HashSet<int>>(); //Si può creare anche un HashSet di HashSet

        //Puoi anche inizializzare con elementi:
        HashSet<int> hashSetD = new HashSet<int>() { 1, 2, 3, 4, 5 };
        
        //O da una collezione esistente:
        int[] numeri = { 1, 2, 3, 3, 4, 5 }; //I duplicati vengono automaticamente rimossi
        HashSet<int> hashSetE = new HashSet<int>(numeri); //Conterrà: 1, 2, 3, 4, 5
    }
```
