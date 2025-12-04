# Programmazione Orientata agli Oggetti
La **programmazione orientata agli oggetti (o Object Oriented Programming o ancora OOP)** è un paradigma (guardare la nota) di programmazione.
Nell'OOP una distinzione fondamentale sono:
- **Oggetti**: Sono **istanze** concrete **di una classe**. Un'oggetto può essere la persona Marcello che è un'istanza della classe Persona. Ogni oggetto avrà le proprie caratteristiche e potrà compiere le azioni definite dalla propria classe.
- **Classi**: Sono il **progetto/modello** o in inglese blueprint che **definisce** le **proprietà/campi e le azioni che quel tipo oggetto avrà**.

>[!NOTE]
>Paradigma: modello di riferimento, esempio o sistema di credenze in questo caso applicato alla scrittura di codice.

Еsempio di classe e oggetto in C#:

```C#
    class Persona // Creazione di una classe
    {
        // Proprietà che sono comuni tra tutte le istanze della classe Persona ma ciascun oggetto le avrà inizializzate con un certo valore.
        public int Anni;
        public string NomeCompleto, Sesso;

        //Nota: trascurate il modificatore public visto che verrà affrontata successivamente.

        // Metodi, che per analogia sono le azioni di un'oggetto, in comune tra ogni oggetto del tipo Persona.
        public void Presentazione()
        {
            Console.WriteLine($"Ciao sono {NomeCompleto}");
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            Persona Marcello = new Persona(); // Creo l'istanza di Persona che sarà un'oggetto di Persona e lo definisco come Marcello.

            // Inizializzo le proprietà di Marcello.
            Marcello.NomeCompleto = "Marcello Rossi";
            Marcello.Anni = 67;
            Marcello.Sesso = "Maschio";

            //Faccio fare all'oggetto la sua azione.
            Marcello.Presentazione();

            //Output: "Ciao sono Marcello Rossi".
        }
    }
```

---

<br><br>

## Vantaggi Principali
La OOP presta 4 principali vantaggi:
1. **modularità**: Il codice è organizzato in blocchi logici, ciò permette di avere codice più comprensibile e più facilmente modificabile
2. **Collaborazione**: La suddivisione in blocchi logici permette inoltre di affidare a varie parti del team lo sviluppo di una funzione senza intaccare il lavoro altrui.
3. **Riusabilità**: Dati i diversi metodi e classi create si punta alla creazione di codice utile universalmente, qualcosa come magari un calcola somma che possa essere riutilizzato da più parti del programma.
4. **manutenibilità**: La divisione in blocchi ci forza a lavorare a aree di codice più piccoli e, quindi, in caso di bug avendo fatto modifiche in uno spazio ristretto di codice rende più facile la correzzione del codice. Di conseguenza si può dire che il codice è più facile da mantenere.

<br><br>

## I tre pilastri principali

### Incapsulamento 
**L'incapsulamento** nell'OOP consiste nel "nascondere" i dettagli interni di una classe e di gestirne l'accesso. Questo è molto importante in quanto per esempio in un programma bancario non vogliamo che il contenuto del conto venga modificato dall'utente ma solo da parti specifiche del programma.

Per poter nascondere questi dettagli usiamo i così detti modificatori di accesso che definiscono i permessi di accesso a parti del codice:
|         Posizione del chiamante        | public | private |
|:--------------------------------------:|--------|---------|
| All'interno della classe               |    ✔️️   |     ✔️️    |
| Classe derivata (stesso assembly)      |    ✔️️   |     ✔️️    |
| Classe non derivata (stesso assembly)  |    ✔️️   |     ✔️️    |
| Classe derivata (assembly diverso)     |    ✔️️   |     ❌    |
| Classe non derivata (assembly diverso) |    ✔️️   |     ❌    |

#### Incapsulamento in ausilio di public e private
Dunque come possiamo constatare da questa tabella: **Public** è il modificatore di accesso **meno restrittivo** e rende una porzione di codice disponibile **ovunque**. **Private** invece rende il codice **utilizzabile** solo **all'interno della classe**. Un campo della classe verrà settato a private se non specificato

>[!NOTE]
> Il concetto di classe derivata e padre verrà affrontato nella sezione sull'ereditarietà.\
> Un **campo** è una **variabile private** all'interno di una classe, mentre una **proprietà** è una **variabile** che viene resa **disponibile anche all'esterno della classe**.

```C#
    class Conto
    {
        private int saldo; // Saldo è private perché voglio che venga modificato solo all'interno della classe
        private string proprietario; // Il proprietario è privato e utilizza il metodo Proprietario, che può essere chiamato getter, per ricevere il valore ma il suo contenuto non deve mai variare

        public string Proprietario()
        {
            return proprietario;
        }

        public void Prelievo(int val)
        {
            if(val > saldo)
            {
                throw new ArgumentException("Il valore supera i soldi che possiedi");
            }

            saldo -= val;
        }

        public void Deposito(int val)
        {
            if(val <= 0)
            {
                throw new ArgumentException("Non puoi aggiungere un valore negativo o pari a zero");
            }

            saldo += val;
        }
    }
```
In questo esempio di codice **applichiamo l'incapsulamento per garantire** che **l'utente passi** solo **valori validi** e non rompere il codice.

#### getter e setter
Il modo per poter prendere o modificare il valore di un campo/proprietà sono i **getter e setter**. In java inizialmente vengono impostati nel modo in cui sono impostati sopra ma C# ha introdotto dei metodi per abbreviare il modo in cui è scritto e renderlo più chiaro. Andiamo dunque a vedere come migliorarlo:
```C#
    class Conto
    {
        private int saldo;
        private string proprietario;

        public int Saldo
        {
            get => saldo; // Può essere sostituito da get { return saldo; } oppure direttamente public int Saldo => saldo;
        }

        public string Proprietario => proprietario; // Starà al main a questo punto a stampare proprietario

        public void Prelievo(int val)
        {
            if(val > saldo)
            {
                throw new ArgumentException("Il valore supera i soldi che possiedi");
            }

            saldo -= val;
        }

        public void Deposito(int val)
        {
            if(val <= 0)
            {
                throw new ArgumentException("Non puoi aggiungere un valore negativo o pari a zero");
            }

            saldo += val;
        }
    }
```

#### Costruttori e Overload
L'utilizzo dell'incapsulamento fa sorgere un problema imporatante. Una volta creata un'istanza della classe non abbiamo alcun modo di inializzare i campi. Per risolvere questo problema arrivano in nostro soccorso i **costruttori**.
Un costruttore è una funzione, solitamente col nome della propria classe, che verrà **richiamata solo nel momento in cui si crea un nuovo oggetto**. Spesso i costruttori sono soggetti ad **overload** ovverosia la pratica nel creare più funzioni con un numero o tipi diversi di parametri.

Di seguito troverete l'esempio di prima con l'implementazione di un costruttore e dei loro overload:
```C#
    class Conto
    {
        private int saldo;
        private string proprietario;

        public int Saldo => saldo;
        public string Proprietario => proprietario;

        public Conto(string proprietario)
        {
            // In questo caso il this reindizzerà ai campi o proprietà l'assegnazione dunque, nonostante ci sia il shadowing delle variabili, possiamo accedervi
            this.saldo = 0;
            this.proprietario = proprietario;
        }

        // Mentre se inseriamo : this(parametri) il programma richiamerà il costruttore creato precedentemente passandogli la proprietà proprietario e lo inizielizzerà seguendo il formato del costruttore precendente. Questo permette di evitare ripetizioni. 
        public Conto(string proprietario, int saldo) : this(proprietario)
        {
            if(saldo < 0)
            {
                throw new ArgumentException("Il saldo non può essere negativo");
            }
            this.saldo = saldo;
        }

        public void Prelievo(int val)
        {
            if(val > saldo)
            {
                throw new ArgumentException("Il valore supera i soldi che possiedi");
            }

            saldo -= val;
        }

        public void Deposito(int val)
        {
            if(val <= 0)
            {
                throw new ArgumentException("Non puoi aggiungere un valore negativo o pari a zero");
            }

            saldo += val;
        }
    }
```
>[!NOTE]
>L'overload rimane uguale anche per i metodi normale tranne per l'utilizzo di : this che non può essere fatto con i metodi.

#### Static

Un'altro modificatore è **static**. Questo **permette di creare delle variabili che verranno condivise tra tutte le istanze di una classe**. Un esempio pratico per cui utilizzarlo è per tenere conto di un id.
Static **si può anche applicare ai costruttori dove avrà lo scopo di inizializzare tutti i campi/proprietà statici**. Questo costruttore, a differenza di quelli tipici che vengono eseguiti volta che crea un'istanza di una classe, **verrà eseguito solamente alla prima istanza di una classe**.\
Mentre un metodo statico è utile perché può essere richiamato senza aver la necessita di creare un'istanza della classe. Va però prestata attenzione al fatto che all'interno di esso non si potranno richiamare dei campi o prorpietà in quanto non saranno definiti a meno che non siano costanti inizializzate di base.

Di seguito aggiungo degli utilizzi di static rimanendo nel nostro conto:
```C#
    class Conto
    {
        private int saldo;
        private string proprietario;
        static int id;
        const char valuta = '€';

        public int Saldo => saldo;
        public string Proprietario => proprietario;

        //Costruttore statico in quanto deve inizializzare id (anche se in questo codice non verrà mai usato)
        static Conto()
        {
            id = 1;
        }

        public Conto(string proprietario)
        {
            this.saldo = 0;
            this.proprietario = proprietario;
        }

        public Conto(string proprietario, int saldo) : this(proprietario)
        {
            this.saldo = saldo;
        }

        public void Prelievo(int val)
        {
            if (val > saldo)
            {
                throw new ArgumentException("Il valore supera i soldi che possiedi");
            }

            saldo -= val;
        }

        public void Deposito(int val)
        {
            if (val <= 0)
            {
                throw new ArgumentException("Non puoi aggiungere un valore negativo o pari a zero");
            }

            saldo += val;
        }

        public static void UnitaDiMisura()
        {
            //Metodo statico che accede a un valore sempre settato, in caso metteste magari saldo darebbe errore.
            Console.WriteLine($"L'unità di misura utilizzata in questo conto è: {valuta}");
        }
    }
```

#### Differenza tra namespace e assembly
La differenza tra il namespace e l'assembly si basa sul fatto che:
- **Namespace**: Il namespace è un modo per organizzare logicamente il codice, raggruppando classi correlate. Puoi avere svariati namespace nello stesso assembly.
- **Assembly**: L'assembly è il confine fisico del codice compilato. In C# è rappresentato dai file .dll.

Questa distinzione mi porta ad introdurre il modificatore d'accesso internal. Di seguito la tabella aggiornata.
|         Posizione del chiamante        | public | internal | private |
|:--------------------------------------:|--------|----------|---------|
| All'interno della classe               |    ✔️️   |     ✔️️    |    ✔️️    |
| Classe derivata (stesso assembly)      |    ✔️️   |     ✔️️    |    ❌    |
| Classe non derivata (stesso assembly)  |    ✔️️   |     ✔️️    |    ❌    |
| Classe derivata (assembly diverso)     |    ✔️️   |     ❌    |    ❌    |
| Classe non derivata (assembly diverso) |    ✔️️   |     ❌    |    ❌    |
---

### Ereditarietà
L'**ereditarietà** è quel concetto dell'OOP che **permette ad una classe detta figlio, di ereditare tutti i campi/proprietà e metodi di una classe detta padre a patto che questi non siano private**.
Le classi padri possono essere chiamate anche classe **base** e le figlie **classe derivate**.

Questo ci permette di fare un'approfondimento sui costruttori e sul loro overload:
```C#
    class Figura
    {
        private int _base;
        private string nomeFigura;

        //Accessibile da tutte le classi padre e figlie
        protected int Base { get; set; }
        protected int NomeFigura { get; set; }

        public Figura(int lati, string nomeFigura)
        {
            if (lati < 3)
            {
                throw new ArgumentException("Non esistono figure con meno di due lati");
            }

            this._base = lati;
            this.nomeFigura = nomeFigura;
        }

        //Definisce che la funzione può essere sovvrascritta. Dunque rende la funzione sovvrascrivibile
        public virtual int CalcolaArea()
        {
            return Base * Base;
        }
    }
    // Rettangolo (classe figlia) eredita Figura (classe padre)
    class Rettangolo : Figura
    {
        // Può lo stesso implementare proprietà/campi o metodi propri
        private int height;

        // Con :base() richiamamiamo il costruttore padre con i suoi n parametri
        public Rettangolo(int basi,int altezza, string nomeFigura) : base(basi, nomeFigura)
        { 
            height = altezza;
        }

        // Sovvrascrive il comportamento del metodo padre CalcolaArea()
        public override int CalcolaArea()
        {
            return height * Base;  
        }
    }
```
#### Protected table and Is-a/Has-a

|         Posizione del chiamante        | public | internal | private | protected | protected internal |
|:--------------------------------------:|:------:|:--------:|:-------:|:---------:|:------------------:|
| All'interno della classe               |   ✔️   |    ✔️     |    ✔️    |     ✔️     |         ✔️          |
| Classe derivata (stesso assembly)      |   ✔️   |    ✔️     |    ❌    |     ✔️     |         ✔️          |
| Classe non derivata (stesso assembly)  |   ✔️   |    ✔️     |    ❌    |     ❌     |         ✔️          |
| Classe derivata (assembly diverso)     |   ✔️   |    ❌     |    ❌    |     ✔️     |         ✔️          |
| Classe non derivata (assembly diverso) |   ✔️   |    ❌     |    ❌    |     ❌     |         ❌          |

L'ereditarietà porta anche alla **relazione is-a**, ovverosia Rettangolo è una figura mentre una figura non è un rettangolo. Dunque **Rettangolo is a Figura**. La relazione **Has-a** invece si applica nel momento in cui **una classe utilizza un'altra classe** al suo interno e non ne eredita alcun comportamento. Un esempio potrebbe essere Figura has a List se sostituissimo _base con una lista di int.

#### Down-casting e Up-casting e Is

Ci sono due tipi di cast:
- **"Down-casting**: cast da padre a figlio
- **"Up-casting**:  cast da figlio a padre

in C# per fare un cast:

```C#
    class Padre
    {
        // Non implementata
    }

    class Figlio
    {
        // Non implementata
    }

    internal class Program
    {
        Padre p = new Padre();
        Figlio f = new Figlio();

        //Down casting insicuro:
        //Diretto
        Figlio downCastInsicuro = (Figlio)p;

        //Down casting sicuro:
        //Indiretto
        Figlio downCastSicuro = p as Figlio;
        
        //Up casting insicuro:
        //Diretto
        Padre upCastInsicuro = (Padre)f;

        //Up casting sicuro:        
        //Indiretto
        Padre upCastSicuro = f as Padre;

        //Is si usa per fare le comparison tra tipi di oggetto

        if(p is Padre)
        {
            // codice...
        }
    }
```

### Polimorfismo

>[!IMPORTANT]
>Gli appunti sono da sistemare in quanto il polimorfismo non è completo e va spostato anche override in questa sezione