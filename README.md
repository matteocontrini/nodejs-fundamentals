# Fondamentali di Node.js [![This work is licensed under a Creative Commons Attribution-ShareAlike 4.0 International License.](https://i.creativecommons.org/l/by-sa/4.0/88x31.png)](https://creativecommons.org/licenses/by-sa/4.0/)

## Cos'è?

**Node.js** è un runtime per JavaScript, cioè un ambiente che permette di eseguire del codice JavaScript.

L'equivalente di Node.js per Java è il JRE (Java Runtime Environment), che in fase di sviluppo è affiancato dal JDK (Java Development Kit), cioè una suite di tool di compilazione per permettere lo sviluppo di un'applicazione.

Per chi conosce .NET Core, vale la stessa analogia. L'SDK si usa in fase di sviluppo, mentre il .NET Core Runtime in fase di esecuzione del programma.

## Documentazione di JavaScript

La documentazione più autorevole per trovare informazioni su JavaScript è quella di Mozilla, che si chiama MDN. Dalla [homepage di MDN](https://developer.mozilla.org/en-US/), si può accedere alla sezione dedicata a JavaScript dal menù "Technologies".

[Non usare W3Schools](https://skillsandmore.org/non-usare-w3schools/), grazie.

## Compilatore?

JavaScript non è un linguaggio compilato, ma prevalentemente interpretato. Ciò significa che per eseguire un programma scritto in JavaScript ci vuole il codice sorgente del programma.

Si dice *prevalentemente* interpretato perché nelle implementazioni più recenti il codice sorgente del programma viene in realtà compilato, ma *just in time* (JIT). Significa in pratica che se hai un ciclo che esegue un'istruzione un milione di volte, l'interprete/compilatore anziché "interpretare" l'istruzione un milione di volte, la compila, in modo da aumentare le prestazioni (la compilazione JIT è leggermente più lenta dell'interpretazione, ma il tempo speso viene rapidamente e largamente recuperato).

Per eseguire del codice JavaScript c'è quindi un solo passaggio, e cioè si invoca l'interprete sul codice sorgente, e lui lo esegue, subito.

In Java, invece, un file di codice viene compilato in un file con estensione .class, che contiene il bytecode (cioè un linguaggio intermedio), e poi in un momento separato viene eseguito (usando la JVM). Per i dettagli => Abeni.

## Versioni di JavaScript

Negli ultimi anni, lo sviluppo di JavaScript ha avuto uno "sprint", anche grazie a Node.js, per cui sono spuntate in un paio di anni due versioni che hanno cambiato non di poco il modo in cui si scrive codice JavaScript.

La cosa più importante da sapere è che lo standard di JavaScript rilasciato nel 2015 e chiamato **ECMAScript 2015** (o ES6 a volte) ha introdotto delle funzionalità importanti nel linguaggio, per cui quando si scrive del codice in ES2015 bisogna fare attenzione al fatto che poi l'interprete deve supportare questa versione di JavaScript.

Se usi l'ultima versione LTS di Node.js non c'è da preoccuparsi, perché supporta a pieno ES2015.

## Le variabili in JavaScript

C'è una cosa importante da dire sulle variabili in JavaScript.

Precedentemente ad ES2015, c'era solo un modo per dichiarare ed inizializzare una variabile, e cioè usando la parola chiave `var`:

```js
var gino = true;
```

Di recente è stata introdotta anche la parola chiave `let`, che ha un'importante differenza, e cioè lo scope di validità della variabile creata.

```js
function ciao() {
	if (true) {
	    var gatto = 1;
	    let cane = 2;
	}
	
	console.log(cane);
	console.log(gatto);
}
```

Che cosa succede qua? La variabile `gatto`, creata all'interno del blocco dell'`if`, ha in realtà scope di validità equivalente alla funzione `ciao`. Significa che la variabile `gatto` è disponibile *anche fuori dall'if*.

La variabile `cane`, invece, essendo dichiarata con `let` è disponibile soltanto nel blocco in cui è stata dichiarata, e quindi nel corpo dell'`if` (che inizia e finisce con le parentesi graffe).

Nel mondo reale, se non stai scrivendo codice JavaScript per il browser (che ha problemi di compatibilità), usa **sempre**, **SEMPRE** `let`. È molto più facile da comprendere (è simile a come funzionano le cose in altri linguaggi), e riduce enormemente la possibilità di fare errori o di introdurre bug difficili da trovare. Un altro vantaggio è il fatto che non si può dichiarare due volte una variabile usando `let`:

```js
var gatto;
var gatto; // funziona

let cane;
let cane; // non funziona, grazie al cane
```

## I tipi

JavaScript non è fortemente tipizzato, e i tipi non sono statici. Che vuol dire?

In pratica, che il tipo di una variabile può cambiare, e che quando si accede a una variabile non c'è nessun controllo sul suo tipo. Il linguaggio ci prova (a tempo di esecuzione!), e al massimo fallisce con un'eccezione.

```js
let v = 1;
v = 'ciao'; // il tipo cambia
v = [1, 2, 3]; // due volte
v = null; // qualsiasi variabile può assumere valore null

// Ma tenete d'occhio le scale! cit.
```

Questa "feature" è sia un grande vantaggio che un grande disastro. Dà una grande libertà nel modo in cui si usano le variabili, e permette in molti casi di accorciare il codice, ma è spesso fonte di bug. Una lista di casini che seguono da questa "feature" si trova [qua](https://github.com/denysdovhan/wtfjs) (ma sono cose abbastanza avanzate, leggerle all'inizio è un po' una follia).

## I numeri

Una particolarità di JavaScript è che tutti i tipi numerici sono raggruppati in un unico tipo chiamato `Number`. La rappresentazione interna (in memoria) cambia in base a se il numero è un intero, un short (8 bit), o un numero in virgola mobile, o boh, ma all'esterno è tutto un `Number`.

Come si fa la conversione (parsing) di una stringa in numero?

Ci sono tanti modi. La tabella che segue li riassume. L'intestazione delle colonne indica il codice eseguito, e le varie righe sono l'output dell'istruzione.

![Conversione numeri](https://i.stack.imgur.com/dzin3.png)

![](https://media.makeameme.org/created/wtf-did-i-5b5d17.jpg)

Se la cosa ti spiazza, usa il metodo più semplice, che ha tra l'altro poche conseguenze indesiderate. Esempio:

```js
let input = '123';
let numero = +input;
```

Detto in parole, metti un "+" davanti alla variabile contenente la stringa da convertire, e sarà convertita in numero. Se è già un numero, resta un numero. Se non è un numero, vediamo fra poco come si fa a capirlo.

## Stringhe

Le stringhe *funzionano*. Supportano in automatico UTF-8, a differenza di altri linguaggi che ricordano certi simpatici rettili striscianti.

Un consiglio: quasi tutti gli sviluppatori JavaScript utilizzano gli apici singoli anziché quelli doppi, a differenza di Java.

Ciò significa che anche se entrambe queste forme sono corrette:

```js
let a = "testo";
let b = 'testo';
```

Quella preferita è quasi sempre la seconda.

## Le classi?

In Java ci sono le classi e l'intero linguaggio è orientato agli oggetti. In JavaScript, è un casino.

Prima di ES2015, le funzioni potevano essere assimilate a classi. Si poteva (e si può) scrivere:

```js
function Classe() {}

Classe.prototype.metodo = function() {
    console.log('ciao');
};

var istanza = new Classe();
```

Questo pezzo di codice crea una funzione, modifica il *prototipo* della funzione aggiungendo un "metodo", e poi esegue `new Classe()` per creare un'istanza della classe. È tutto molto strano, fa' finta di non averlo mai visto.

Nelle versioni recenti di JavaScript, il tutto è più familiare:

```js
class Classe {
    metodo() {
        console.log('è colpa di mattia');
    }
}

let istanza = new Classe();
```

Detto questo, le classi non le troverai *quasi* mai in JavaScript o Node.js. Ci sono altri modi più consolidati di strutturare il codice, e dato che il linguaggio non ti obbliga ad avere a che fare con le classi, puoi tranquillamente continuare a credere che non esistano, un po' come fanno alcuni con le famiglie arcobaleno.

Una cosa da da sapere è che quando hai a che fare con le classi ci devi mettere un `new` davanti.  Ad esempio, ti capiterà di scrivere `new Date();`, senza sapere se dietro quel new c'è una funzione o una classe vera e propria. Ma nulla di nuovo, no?

## Object literal

Al posto delle classi, un modo moolto diffuso per creare tipi personalizzati è utilizzare la notazione *object literal*. Questa permette di creare degli oggetti "al volo", e il risultato è molto simile a un dizionario, o hashmap.

```js
let roba = {
    chiave: 'valore',
    fausto: false,
    numero: 123
};
```

A differenza delle classi, gli oggetti "anonimi" li troverai molto spesso, e ti troverai anche a scriverne. Ogni volta che ti serve creare un tipo "composto", usa la notazione *object literal*. È gratis.

Dal codice sopra si può osservare di nuovo la flessibilità di JavaScript. Il numero di proprietà/campi, così come il loro tipo, è completamente libero, e come detto prima può cambiare durante l'esecuzione.

Ad esempio, possiamo accedere a una proprietà di un oggetto e modificarla liberamente:

```js
roba.numero = 'eheh';
```

Una sintassi equivalente è questa:

```js
roba['numero'] = 'eheh';
```

Fanno la stessa identica cosa, puoi scegliere quella che ti piace di più.

In JavaScript, gli oggetti sono liberamente alterabili. Ad esempio, **all'oggetto sopra si può aggiungere una proprietà, e non c'è modo di impedirlo**.

```js
roba.nuovaprop = 'aaa';
```

Questo vale anche per le classi. Anche se i campi/proprietà della classe sono ben definiti nel codice, nulla vieta di crearne di nuovi. Si può addirittura aggiungere un nuovo campo su un oggetto che non abbiamo creato noi. Ad esempio:

```js
let d = new Date();
d.prop = 12; // si può fare
console.log(d.prop);
```

La flessibilità della notazione object literal permette di fare cose come queste:

```js
let obj = {
    data: {
        valore: 1,
        gino: {
            aaa: 'ok'
        }
    },
    ok: true
};
```

E cioè l'annidamento degli oggetti è consentito senza limite di profondità. Esistono anche i riferimenti circolari, se li hai presenti.

## Array literal

La notazione "literal" è disponibile anche per gli array/liste (che sono la stessa cosa!):

```js
let arr = [1, 2, 3];
let arr2 = [true, 0, 'ciao'];
```

Anche in questo caso il controllo dei tipi non esiste, cioè ogni elemento dell'array può avere qualsiasi tipo, e può anche cambiare durante l'esecuzione. Per creare un array vuoto si fa così:

```js
let arr = [];
```

Ci sono altri modi di farlo (come `new Array()`), ma non sono consigliati.

Per aggiungere un elemento a un array:

```js
arr.push(1);
```

Gli oggetti possono essere elementi di un array, ed è molto comune trovarli. Ad esempio:

```js
let arr = [];
arr.push({ val: 1 });
arr.push({ val: 2 });
```

## Truthiness e  falsiness

Come sai, in JavaScript esiste la *coercizione dei tipi*, quella cosa strana che permette ad esempio di confrontare una stringa con un intero, con conversione implicita. Le regole di conversione sono molto complesse, ma c'è una cosa fondamentale da sapere, e cioè **quando un valore viene valutato come truthy o falsy** ("abbastanza vero" e "abbastanza falso", diciamo).

Un valore è detto falsy se è uno dei seguenti:

- null
- undefined
- 0
- '' (stringa vuota)
- NaN
- false

Un valore è truthy se non è falsy.

Sembra un casino, ma in realtà è uno strumento molto potente. In sostanza permette di fare controlli rapidi sul contenuto di una variabile, per vedere approssimativamente **se "contiene qualcosa"**. Ad esempio, supponiamo di avere una variabile `input` con un valore preso in input dall'utente, che pensiamo sia una stringa ma non sappiamo in realtà cosa abbia dentro. Possiamo scrivere:

```js
// input è dichiarato da qualche parte sopra
if (input) {
    // la variabile contiene qualcosa,
    // che *non è* null, undefined, 0, '', NaN, false
}
```

Ancora più comune è vedere scritto:

```js
if (!input) {
    // ritorno un errore perché l'input non è "valido"
}
```

Questa strategia si può usare anche per **controllare che la conversione di qualcosa da stringa a `Number` sia avvenuta con successo**. Dalla tabella di prima si può vedere che un numero che non è un numero diventa `NaN`, cioè un valore molto speciale che indica che quella variabile non contiene un numero. Dico che è un valore molto speciale perché è difficile da trattare. Ad esempio, non è uguale nemmeno a s[é](http://www.accademiadellacrusca.it/it/lingua-italiana/consulenza-linguistica/domande-risposte/accentazione-pronome-stesso) stesso!

Si può quindi verificare che un numero parsato non sia in realtà un numero così:

```js
let n = +'non un numero';

if (isNaN(n)) {
    console.log('non è un numero');
}
```

Un modo più approssimativo è quello di usare il controllo implicito della truthiness di una variabile, quindi:

```js
if (!n) {
    console.log('non è un numero');
}
```

Attenzione però, perché in questo caso il valore `0` non verrà considerato un numero!

Un altro caso d'uso della truthiness è per fornire un valore di default/fallback per una variabile. Esempio:

```js
const PORT = process.env.PORT || 3000;
```

Il significato è che la costante `PORT` assume il valore della variabile `process.env.PORT` (cioè una variabile d'ambiente di nome `PORT`) se ha un valore (secondo la regola sopra), altrimenti il valore costante 3000.

## For each?

Il ciclo for esiste e funziona come negli altri linguaggi:

```js
for (let i = 0; i <= 10; i++) {
    // boh
}
```

Il foreach non esiste, ma è sostituito da `for... of`, praticamente uguale:

```js
let arr = [1, 2, 3];

for (let item of array) {
    console.log(item);
}
```

## Undefined

In JavaScript, **le variabili che non sono ancora state inizializzate si possono usare**. Mentre in Java e in molti altri linguaggi una variabile non inizializzata non è "leggibile" finché non assume un valore, in JavaScript ha sempre un valore, che inizialmente è  `undefined`.

Si può controllare se una variabile è `undefined` così:

```js
let a; // dichiarata ma senza valore
console.log(a == undefined); // questo scrive true
```

**Se la variabile non è dichiarata, questo metodo non funziona**. Bisogna invece utilizzare l'operatore `typeof`, così:

```js
// non ho ancora dichiarato nesuna variabile
console.log(typeof a == 'undefined'); // true
```

## Node.js REPL

Siamo a metà. È ora di passare a Node.js.

Una delle prime cose da sapere quando si prende in mano Node.js è che **JavaScript è un linguaggio di scripting**. Significa che il codice può essere scritto ed eseguito inserendolo direttamente in un file .js, senza dover inserire funzioni "main", oppure classi, ecc. Il codice viene eseguito come script (come avviene con l'amico bash).

C'è di più, aprendo una finestra del terminale e scrivendo `node`, si entra nella [REPL](https://en.wikipedia.org/wiki/Read–eval–print_loop) di Node.js, e cioè una modalità interattiva in cui si può scrivere del codice JavaScript riga per riga, e le istruzioni vengono eseguite "istantaneamente".

## npm

npm è il gestore dei pacchetti di Node.js, installato in automatico con Node.js.

Nelle versioni recenti di npm, per installare un nuovo modulo nella cartella corrente si usa:

```bash
npm install nomemodulo
```

È **importante** aver eseguito `npm init` **prima** di questo comando, perché altrimenti l'installazione del modulo potrebbe non essere eseguita nella cartella corrente ma in una "padre".

I comandi `npm init` e poi `npm install` creano due file: *package.json* e *package.lock.json*.

- *package.json* memorizza le informazioni sul progetto, tra cui la lista delle sue dipendenze
- *package.lock.json* memorizza le versioni *esatte* delle dipendenze del progetto, in modo che se un'altra persona vuole poi continuare lo sviluppo sul progetto, può scaricare le stesse *identiche* dipendenze

I moduli scaricati (cioè le dipendenze) vengono installati nella cartella `node_modules`, che con il tempo diventerà enorme. Contiene il codice sorgente di tutti i moduli di cui il programma ha bisogno per funzionare. La cartella `node_modules` **non va mai committata con git**.

Consiglio di usare il sito web [gitignore.io](https://www.gitignore.io/) per generare un file .gitignore per Node.js e il vostro sistema operativo, da inserire nella radice della repository. L'effetto sarà quello di impedire che file che *devono* restare locali vengano tracciati da git.

## node e nodemon

Per eseguire un file JavaScript con Node.js, scrivi in un terminale:

```sh
node index.js
```

Dove `index.js` è l'*entrypoint* del programma.

Quando i file sorgente vengono modificati, per riavviare l'applicazione bisogna interrompere con CTRL+C, e poi eseguire di nuovo il comando.

Più facilmente, si può installare il modulo `nodemon` globalmente (cioè in una cartella di sistema che vale per tutti i progetti), e poi usarlo al posto di node. In questo modo, il processo si riavvierà in automatico ad ogni modifica del codice sorgente.

Da esguire solo una volta:

```sh
npm install -g nodemon
```

Per avviare un programma con `nodemon`:

```sh
nodemon index.js
```

## Importare i moduli nel codice

Dopo aver installato un modulo (ad es. `express` oppure `request`), il modulo può essere importato nel programma scrivendo:

```js
const express = require('express');
```

Due cose da notare:

- la funzione `require` è fornita da Node.js, e serve appunto per importare un modulo
- in genere si dichiara il modulo come costante (con `const`), in modo che non possa essere riassegnato successivamente nel codice

Il modulo importato corrisponde di solito a una funzione o a un un oggetto, e permette quindi di fare cose del tipo:

```js
const modulo = require('modulo');
modulo(); // richiamo il modulo come funzione
modulo.getQualcosa(); // il modulo è un oggetto, espone delle funzioni
```

Per sapere come si usa un modulo, bisogna fare riferimento alla documentazione del modulo, oppure al suo codice sorgente.

## La natura asincrona di Node.js

Sei concentrato? [Massima concentrazione](https://www.repstatic.it/content/nazionale/img/2018/05/16/142407308-f47a41b2-2e53-4233-bec5-6a3c29935ab5.jpg). Questa parte è importantissima.

**Node.js esegue il codice utente sempre e comunque in un unico thread**. Non significa che il runtime utilizza un thread per fare tutte le sue cose, ma ne fornisce soltanto uno allo sviluppatore, e non è nemmeno possibile crearne altri manualmente.

Questo metodo funziona perché **le operazioni di I/O (input/output) vengono eseguite in modo asincrono rispetto al thread del programma**.

Ogni volta che il programma richiede la lettura o la scrittura di un file, oppure di scrivere su un socket di rete (quindi ad esempio per inviare una richiesta HTTP), **Node.js trasferisce la richiesta al file system o ai livelli di rete inferiori, e poi libera il thread**.

**In Java**, invece, quando si legge un file o si scrive su uno stream le chiamate sono *bloccanti*. Significa che mentre il programma è in attesa di una risposta dal file system, **il thread è bloccato**, e cioè sta lì letteralmente a fare niente e nessun altro codice può essere eseguito su quel thread.

Nella pratica l'asincronismo di JavaScript/Node.js si traduce nel seguente codice:

```js
const request = require('request');

let url = 'https://unitn.it';

request(url, function finished(err, res, body) {
    console.log(res.statusCode);
});
```

Cosa sta succedendo, in breve?

- importo il modulo `request`, che ho scaricato da npm (serve per inviare richieste HTTP/S). Il modulo è una funzione, e cioè posso "metterlo in moto" scrivendo `request()`
- chiamo `request()`, e passo come primo parametro l'URL del sito web da cui voglio scaricare i dati, e **come secondo parametro una funzione**
- la chiamata della funzione  `request` esegue del codice sull'unico thread di Node.js, e poi **passa la richiesta di rete al sistema operativo**
- quando la richiesta di rete termina, il modulo `request` si occupa di chiamare la **funzione di callback** che abbiamo passato come parametro, in modo che il risultato possa essere elaborato

Ora apriamo una parentesi, e vediamo come si scrivono e si usano le funzioni. Poi torniamo a capire meglio il funzionamento asincrono di JavaScript. **Sempre massima concentrazione**.

## Funzioni, funzioni anonime, e funzioni freccia

La sintassi per creare una funzione con un nome è molto semplice:

```js
function nomeFunzione(x, y) {
    // corpo
}
```

Un po' di cose da notare:

- il tipo di ritorno non va specificato
- il nome della funzione utilizza per convenzione il *camelCase*
- i parametri della funzione non hanno tipo ma soltanto nome

Una funzione può anche essere anonima, e quindi non avere un nome. Ad esempio:

```js
function(x, y) { /* codice */ }
```

A questo punto però la funzione è dispersa nel nulla, per cui possiamo assegnarla a una variabile per poi poterla chiamare:

```js
let fn = function(x, y) { /* codice */ };
```

A partire da ES2015, è disponibile una sintassi ancora più breve per creare una funzione anonima, che prende il nome di "**funzione a freccia**", o "arrow function":

```js
let fn = (x, y) => { /* codice */ };
```

...che normalmente viene scritta su più righe:

```js
let fn = (x, y) => {
    // codice
};
```

La funzione `fn` si può *in ogni caso* chiamare come una qualsiasi funzione scrivendo ad esempio `fn(1, 2)`.

Riprendiamo il codice di prima. Vediamo che c'è una funzione con un nome (`finished`), passata come parametro.

```js
request(url, function finished(err, res, body) {
    console.log(res.statusCode);
});
```

Riscriviamo ora la funzione utilizzando la sintassi a freccia:

```js
request(url, (err, res, body) => {
    console.log(res.statusCode);
});
```

Ecco, questa è attualmente la sintassi consigliata per le *funzioni callback* (ci arriviamo in un attimo). Quindi, quando devi passare una funzione come parametro, usa una funzione a freccia, perché così fan tutti (semicit.).

## Callback?

Parlavamo di funzioni callback, no?

Una **funzione di callback** è una funzione che viene chiamata al termine dell'elaborazione di una richiesta di I/O asincrona.

Nell'esempio sopra, quando il modulo `request` ha ricevuto dal sistema operativo la "notifica" che l'operazione di rete è terminata, la funzione di callback che è stata passata da noi sviluppatori come parametro viene chiamata e quindi eseguita.

Prima o poi ti capiterà di utilizzare le callback nel tuo codice. Ad esempio, se volessi spostare il codice che fa la richiesta HTTP, che abbiamo visto sopra, in una funzione dedicata, dovrai fare qualcosa del tipo:

```js
function getResponseStatusCode(url, callback) {
    request(url, (err, res, body) => {
    	callback(res.statusCode);
	});
}

let url = 'https://unitn.it';

getResponseStatusCode(url, (code) => {
    console.log('Response status code was: ' + code);
});
```

Phew. Ci sono delle cose da chiarire qua. La funzione `getResponseStatusCode` prende come input un URL (una stringa) e una funzione di callback. All'interno della funzione, `request` viene invocato, e quando termina la funzione di callback (chiamata `callback`) viene chiamata, passando come parametro il codice di risposta HTTP.

Il codice di risposta lo ritroviamo poi nella penultima riga del codice, quando all'interno di un'altra callback viene letto usando il parametro.

Una pratica diffusa per le funzioni di callback si chiama **error-first callback**, e consiste nel passare come **primo parametro un errore** (di tipo `Error`, che è in pratica l'equivalente di un'eccezione), e come **secondo parametro il risultato dell'operazione**. Il codice di prima con la gestione degli errori diventerebbe quindi:

```js
function getResponseStatusCode(url, callback) {
    request(url, (err, res, body) => {
        if (err) { // controlla eventuali errori di rete come i timeout
            callback(err);
            return;
        }
        
    	callback(null, res.statusCode);
	});
}

let url = 'https://unitn.it';

getResponseStatusCode(url, (err, code) => {
    if (err) {
        console.log('An error has occured');
        console.log(err);
        return;
    }
    
    console.log('Response status code was: ' + code);
});
```

Il funzionamento asincrono con le callback di Node.js impedisce di scrivere codice sequenziale come negli altri linguaggi. Se vogliamo quindi eseguire una richiesta asincrona dopo l'altra, dobbiamo *annidare* le richieste all'interno delle callback. In modo semplificato, intendo questo:

```js
one(() => {
    console.log('Uno');
    
    two(() => {
        console.log('Due');
        
        three(() => {
            console.log('Tre');
        });
    });
});

console.log('Zero');
```

Se supponiamo che le funzioni `one`, `two,` e `three` siano asincrone, le istruzioni `console.log()` indicano l'ordine dell'output.

Se invece avessimo scritto il codice seguente, l'ordine dell'output sarebbe stato imprevedibile.

```js
one(() => {
    console.log('Uno');
});

two(() => {
    console.log('Due');
});
```

## Il loop degli eventi

Abbiamo detto che Node.js ha un solo thread per eseguire il codice. Ma come funziona nello specifico? Chi decide quale codice viene eseguito?

Tutto si basa sul loop degli eventi (*event loop*, come dicono quelli). È in sostanza un ciclo che esegue porzioni di codice sincrono, di continuo, una dopo l'altra, finché c'è qualcosa da processare.

Per capire meglio, rubo uno schema da [qua](https://medium.com/the-node-js-collection/what-you-should-know-to-really-understand-the-node-js-event-loop-and-its-metrics-c4907b19da4c):

![Event loop](https://cdn-images-1.medium.com/max/1600/1*ROxiavz7LeRpIfcgRDE7CA.png)

Sull'asse delle ascisse troviamo il tempo, e i blocchi verdi sono le varie fasi che compongono ogni singolo "loop". Come vedi, tra le tante c'è una fase che esegue i timer, e **una che esegue le callback**, cioè in pratica tutto il codice utente che scriverai. Ogni fase ha una coda FIFO contenente funzioni che devono essere eseguite.

Se volessimo, Node.js ci permetterebbe anche di bypassare queste code, ad esempio utilizzando `setImmediate(fn)`, che esegue la funzione specificata al prossimo loop, indipendentemente da quanto altro codice abbiamo in coda.

Si può anche utilizzare la funzione `process.nextTick(fn)`, che permette di schedulare l'esecuzione della funzione al prossimo tick (si veda il diagramma per capire cos'è un tick).

Per dimostrare il fatto che Node.js esegue una funzione alla volta, prendiamo il seguente codice:

```js
setTimeout(() => {
    console.log('Timeout expired');
}, 1000);

while (true) {
    ;
}
```

La chiamata a `setTimeout` aggiunge nella coda dei timeout una funzione da eseguire fra un secondo. Poi c'è un ciclo infinito, che non fa niente ma tiene la CPU occupata.

Eseguendo questo codice, si scoprirà che **il codice all'interno del timeout non verrà *mai* eseguito**, perché il loop degli eventi è impegnato ad eseguire il ciclo infinito.

**Questo è il motivo fondamentale per cui Node.js non va usato per eseguire operazioni che richiedono molta CPU**. Immaginate un server web che gestisce 10 richieste al secondo. Se ciascuna di queste richieste impiega un secondo di CPU (quindi ad esempio c'è un ciclo enorme), le richieste devono attendere rispettivamente 1, 2, 3, ecc. secondi prima di ricevere una risposta!

**RICORDA**: mai inserire codice sincrono che richiede molto tempo ad essere eseguito, altrimenti Node.js non worka!

C'è un'altra cosa da notare. Scriviamo del codice che esegue delle operazioni sincrone, poi chiama qualcosa di asincrono, e poi nient'altro. Nel momento in cui c'è la chiamata asincrona, il loop degli eventi "stacca" la CPU dalla funzione, e continua eventualmente il suo lavoro con altro codice in coda.

Ci sei ancora? Vediamo un esempio, con dei commenti:

```js
let out = '';
while (i < 100) {
    out += i + ';';
}
// ^^^^ le tre righe sopra sono CODICE SINCRONO
// vengono eseguite tutte insieme E NON POSSONO ESSERE MAI MAI MAI INTERROTTE

// la riga sotto effettua internamente una chiamata ASINCRONA al sistema operativo
// soltanto quando la richiesta di rete sarà terminata, il controllo sarà passato alla funzione di callback
// cioè: la richiesta *non è bloccante*
request('https://unitn.it', () => {
    console.log('Questo viene eseguito dopo');
});
```

Essendo Node.js *single threaded*, bisogna raramente preoccuparsi dei problemi di sincronizzazione, proprio perché **un blocco di codice sincrono non può mai essere interrotto a metà**, se non per la presenza di una richiesta di I/O.

## Torniamo alla definizione...

Spesso si dice che Node.js è un runtime con I/O *non-blocking* e *event-driven*.

Ora puoi capire meglio perché:

- *non-blocking*, perché quando ci sono operazioni di I/O il thread non sta lì ad aspettare bloccato
- *event-driven* perché c'è il loop degli eventi

## Express.js

Express.js è un framework per realizzare applicazioni web in Node.js, utilizzando il protocollo HTTP. Non è considerato da tutti il migliore, ma è sicuramente il più popolare.

Come si scrive un semplice server web in Express.js? Così:

```js
const express = require('express');
const app = express();

app.get('/', (req, res) => {
   res.send('Hello'); 
});
```

**Stiamo mettendo in pratica tutto quanto detto finora.**

Il modulo `express`  viene importato come costante, e questo modulo è una funzione. Nella seconda riga infatti eseguiamo `express()` per creare un'istanza del server web. Come si fa a sapere che si fa così e magari non scrivendo  `new express()`? Dipende da modulo a modulo, quindi bisogna leggere la sua documentazione.

Successivamente, nel codice registriamo una funzione per ricevere le richieste GET all'endpoint / del server web. La funzione è una funzione di callback, e in questo caso riceve due variabili come argomenti: quella che descrive la richiesta e quella che descrive la risposta. `res` permette anche di rispondere e inviare qualcosa al client.

## Express.js: rispondere con JSON

Il codice sopra si può modificare lievemente per fare in modo che risponda alle richieste con un oggetto, che viene in automaticamente serializzato (cioè convertito da oggetto a stringa) da Express.

```js
app.get('/', (req, res) => {
    let roba = { name: 'Niccolò' };
    
	res.send(roba); 
});
```

Con questo codice però c'è un problema. Se non diciamo a Express di fare diversamente, lui/lei assume che quello che vogliamo inviare sia una pagina HTML. Imposterà quindi l'header HTTP `Content-Type`  scrivendoci dentro `text/html`.

Per "risolvere" questo problema, dobbiamo sostituire la riga `res.send(roba);` con:

```js
res.json(roba);
```

Fa la stessa cosa di prima, ma imposta l'header `Content-Type: application/json`  in automatico.

## Express.js: middleware

Se vieni da Java Servlet, conoscerai i filtri, cioè catene di metodi che permettono di manipolare la richiesta/risposta HTTP.

Si può fare anche usando Express.js, con i **middleware**.

```js
app.use((req, res, next) => {
    console.log('Sta arrivando una qualsiasi richiesta');
    next();
});

app.use('/', (req, res, next) => {
    console.log('Sta arrivando una richiesta a /');
    next();
});

app.get('/', (req, res) => {
    let roba = { name: 'Niccolò' };
    
	res.send(roba); 
});

app.use((req, res) => {
    res.status(404).json({ error: 'Page not found' });
});
```

Nel codice sopra, registriamo un middleware chiamato `app.use()`. La funzione di callback viene chiamata non appena arriva una richiesta HTTP (una qualsiasi), e **l'elaborazione non procede finché non viene chiamata la funzione `next()`**.

La stessa cosa avviene con il secondo middleware registrato, che questa volta però è specifico per l'endpoint `/` del server web.

I middleware sono spesso comodi perché **permettono di manipolare la richiesta** in molti modi, ad esempio facendo il **parsing del corpo della richiesta prima dell'elaborazione dei suoi dati**. Possiamo usare i middlware anche per **gestire l'autenticazione/autorizzazione**, fermando subito le richieste non valide.

Un'altra applicazione interessante è quella di **mostrare una pagina di errore 404 personalizzata**. Lo si può fare come nell'esempio sopra, aggiungendo un middleware dopo tutti gli altri. Quel middleware sarà raggiunto soltanto se nessun middleware precedente avrà preso in carico la richiesta HTTP.

## Strutturare il codice in moduli

Abbiamo detto che le classi in JavaScript si usano poco. Ma allora come si struttura il codice?

Si possono usare i moduli. Semplificando, **in Node.js ogni file che scriviamo è un modulo**. Ogni file può quindi decidere di *esportare* delle funzioni, che saranno accessibili dagli altri moduli.

Facciamo un esempio. Questo è il file `modulo.js`, e l'ultima riga esporta una funzione:

```js
function getName() {
    return "Niccolò";
}

module.exports = getName;
```

Negli altri file possiamo poi scrivere:

```js
const modulo = require('./modulo.js');
let name = modulo();

console.log(name);
```

Un modulo può anche esportare più di una funzione, ad esempio (`modulo.js`):

```js
function getName() {
    return "Niccolò";
}

function getAge() {
    return 21;
}

module.exports = { getName, getAge };
```

Nota: l'ultima riga è equivalente a scrivere:

```js
module.exports = {
    getName: getName,
    getAge: getAge
};
```

Negli altri file, si potrà poi usare il modulo così:

```js
const modulo = require('./modulo.js');
let name = modulo.getName();
let age = modulo.getAge();

console.log(name + '(' + age + ')');
```

## Cose moderne: Promise

![](https://miro.medium.com/max/480/1*WScLtOWQpMpU0QTLvsD4yA.jpeg)

*coming soon*

## Cose moderne: async/await

*coming soon*

## Come approfondire

La documentazione di MDN per quanto riguarda JavaScript è in generale eccellente. Se vuoi davvero andare in profondità, una lettura consigliata è [questo libro gratuito](https://github.com/getify/You-Dont-Know-JS) che si chiama *You don't know JS*. Esplora l'intero linguaggio JavaScript davvero molto in profondità.

Per quanto riguarda il resto, in genere vale la [documentazione di Node.js](https://nodejs.org/dist/latest-v8.x/docs/api/) (attenzione alla versione), e quella dei moduli che usate, ad esempio [Express](http://expressjs.com/) e [request](https://github.com/request/request).