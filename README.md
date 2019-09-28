# TuxMath - Un semplice gioco per imparare la matematica divertendosi

Con questo tutorial utilizzeremo HTML, CSS e Javascript per costruire un gioco simile a [TuxMath] in cui dovremo risolvere delle semplici operazioni matematiche contenute in bolle che cascano dall'alto prima che queste stesse bolle raggiungano il fondo del campo di gioco.

## Setup dell'ambiente 1: NodeJS, npm e yarn

Il prerequisito necessario per tutti è installare i programmi NodeJS, `npm` e `yarn`. Questi tool per il momento installiamoli senza farci troppe domande, vedremo più avanti a cosa servono. Per installare i tool è necessaria la connessione a internet. Occorre scaricare per prima cosa NodeJS per l'appunto dal [sito di NodeJs][NodeJS] (https://nodejs.org/en/).

Una volta fatto, verificare che l'installazione sia andata a buon fine dando i seguenti comandi dalla command line:

```
node -v
npm version
npx -v
```

Devono apparire delle stringhe con informazioni di versione.

A questo punto, utilizzando `npm`, occorre installare l'utility [`yarn`][Yarn] a livello globale. Per fare questo dare il seguente comando:

```
npm install --global yarn
```

(può darsi che sia richiesta una password di sistema o di usare il comando `sudo` se siete dei pro e avete un PC Linux).

Infine, per scrivere il codice in modo efficiente occorre avere un buon editor di codice. Anche se il mio preferito è [Vim][Vim], per il momento vi consiglio qualcosa di meno ostico, per esempio [Visual Studio Code][VSCode]. Ma se ne avete già uno che vi soddisfa continuate a lavorare con quello!

## Setup dell'ambiente 2: scarichiamo il codice di base

Come detto, questo programma utilizza NodeJS per sviluppo e deploy del software. Il setup di un progetto NodeJS è complicato e in questo momento ci interessa poco. Per questo partiamo da un pacchetto preconfezionato (boilerplate) che contiene già tutto il setup di base. Il codice di partenza è disponibile come repository su Github o, o se siete a una sessione del Coderdojo, lo potete ottenere dai mentor presenti alla sessione.

Se non avete accesso a internet o non volete usare git, fatevi dare dai mentor lo zip con la versione iniziale dei file. Potete a questo punto saltare al capitolo [Prima esecuzione](#prima-esecuzione)

### Clone del repo di partenza da GitHub

Se invece siete dei pro, avete connessione + git + command line date i seguenti comandi:

```
git clone https://github.com/coderdojofirenze/js_tuxmath_starter.git js_tuxmath
cd js_tuxmath
```

**NOTA:** Se siete dei pro di Github, prima di dare questi comandi, potete fare un fork del repository sul vostro account Github. In questo modo potrete poi pubblicare il software che realizzerete e fare push delle modifiche sul vostro repo.

A questo punto possiamo inizializzare l'ambiente scaricando tutte le librerie e le utility che servono dando il seguente comando:

```
yarn install
```

## Prima esecuzione

Qualunque di questi metodi abbiate utilizzato (unzip o git clone), dopo queste operazioni, troverete che la vostra directory di lavoro contiene già diversi file. Non attardiamoci nello scoprire a cosa servono, lo vedremo casomai più avanti. Per i più curiosi (ma che conoscono l'inglese e non si lasciano intimorire da un po' di jargon tecnico) [c'è una pagina][jsmodernsetup] che spiega come è stato costruito quest'ambiente di base, che poi in termini tecnici si chiama _boilerplate_: un codice di base che può essere utilizzato come punto di partenza per nuovi progetti.

Adesso possiamo verificare che tutto funzioni dando il comando:

```
npm run start
```

Se non ci sono problemi, nel nostro browser preferito si aprirà magicamente un tab con la nostra applicazione in esecuzione. Per il momento si tratta semplicemente di una pagina scura con un titolo "Ciao Mondo!" di colore rosso su sfondo bianco.

In più se si aprono i "Developer tools" del browser, premendo il tasto **F11**, si può vedere che sulla console è apparso un magico messaggio che ha a che fare con il numero 42.

## Come è strutturato il nostro programma

Adesso che abbiamo visto il programma in esecuzione, facciamo un rapido escursus su come è organizzato il nostro ambiente di lavoro.
Un progetto HTML/Javascript è divisibile in tre aree:

- Il 'codice' HTML che insieme al foglio di stile (CSS), rappresenta gli elementi grafici del nostro programma e come verranno rappresentati nel browser.

- Il codice Javascript, che invece realizza il "motore" che farà avvenire gli avvenimenti all'interno della nostra pagine.

Tutto il codice è contenuto all'interno della directory `src`, che è l'unica che ci interesserà d'ora in poi: qui dentro troverete:

- La pagina HTML: `index.html`
- Il foglio di stile: `style.css`
- Il codice javascript: `js/main.js`

Aprite questi file con l'editor: potete vedere il contenuto della pagina HTML che appare sul browser, le informazioni di stile che portano la pagina ad essere visualizzata con le caratteristiche grafiche che vediamo e il codice javascript che per il momento si limita a stampare una riga di log sulla console.

Provate a modificare uno qualunque di questi file e vedete cosa succede nel browser quando salvate il file. Per esempio cambiate il titolo della pagina da "Ciao Mondo!" a "TuxMath in Javascript":

```html
    <h1 class="header">
      Tux Math in Javascript
    </h1>
```


Come potete osservare, grazie al nostro potente setup, la pagina viene ricaricata automaticamente nel browser quando salviamo qualunque modifica! Questo si rivelerà molto utile durante lo sviluppo e aumenterà di molto la nostra produttività.

## TuxMath 1: il canvas

Per prima cosa aggiungiamo alla nostra pagina html un canvas che useremo per disegnare i nostri oggetti contenenti le operazioni matematiche da risolvere.

Niente di più semplice. Aggiungiamo le seguenti righe nel file `index.html` subito dopo la sezione `<h1>`:

```html
    <div id="canvasdiv">
      <canvas id="tuxmathcanvas"></canvas>
    </div>
```

E le seguenti righe alla fine del file `style.css`:

```css
#tuxmathcanvas {
	width: 400px;
	height: 600px;
	border-width: 7px;
	border-style: solid;
	border-color: var(--solar-base3-color);
	background-color: var(--solar-violet-color);
}
```

Salviamo i file e vediamo come cambia la pagina HTML. Se non ci sono errori il canvas rettangolare apparirà a schermo.

## TuxMath 2: la bolla con l'operazione dentro

Vediamo adesso come disegnare una bolla all'interno del nostro Canvas. Per fare questo dovremo modificare il file con il codice javascript `main.js`.

Togliamo la linea che stampa la stringa dimostrativa con il comando `console.log` e inseriamo codice che segue.

Per prima cosa inseriamo la parte noiosa ma che ci servirà nel seguito: definiamo i colori che useremo per disegnare gli oggetti (copiare i valori dal foglio di stile):

```js
const base03color  = "#002b36";
const base02color  = "#073642";
const base01color  = "#586e75";
const base00color  = "#657b83";
const base0color   = "#839496";
const base1color   = "#93a1a1";
const base2color   = "#eee8d5";
const base3color   = "#fdf6e3";
const yellowcolor  = "#b58900";
const orangecolor  = "#cb4b16";
const redcolor     = "#dc322f";
const magentacolor = "#d33682";
const violetcolor  = "#6c71c4";
const bluecolor    = "#268bd2";
const cyancolor    = "#2aa198";
const greencolor   = "#859900";
```

Quindi prendiamo il riferimento al contesto 2D del canvas e definiamo alcune variabili che ci torneranno utili nel seguito:

```js
var canvas = document.getElementById('tuxmathcanvas');
var ctx = canvas.getContext('2d');
var circlecolor = redcolor;
var textcolor = base02color;
ctx.font = '25px Verdana';
ctx.textAlign = "center";
ctx.textBaseline = "middle";
```

Quindi disegniamo un cerchio:

```js
ctx.beginPath();
ctx.strokeStyle = circlecolor;
ctx.fillStyle = circlecolor;
ctx.arc(150, 100, 50, 0, 2 * Math.PI, false);
ctx.fill();
ctx.stroke();
```

E quindi il testo all'interno del cerchio:

```js
ctx.fillStyle = textcolor;
var op = "4 + 5";
var txtwidth = ctx.measureText(op).width
ctx.fillText(op, 150, 100);
```

Buona parte delle istruzioni che abbiamo inserito, effettuano una serie di operazioni sul contesto "2D" del canvas. Per un riferimento su tutto quello che è possibile fare su un canvas si possono andare a vedere le guide che si trovano su internet (le risorse per i linguaggi HTML e Javascript non mancano!). In questo caso abbiamo usato la [sezione sulle canvas][w3schoolsCanvas] della guida [w3schools.com], ma il sito probabilmente più importante per chi sviluppa pagine web è la guida agli sviluppatori della Mozilla Foundation ([MDN web docs][MDNwebdocs]), molto completa e in parte disponibile anche in italiano.

## Come ripetere un'operazione periodicamente

Il codice realizzato fino ad adesso viene eseguito tutte le volte che carichiamo la pagina, ma una volta che tutte le istruzioni sono state eseguite il programma termina.

Per realizzare il nostro gioco dobbiamo però realizzare un'animazione (le bolle che cadono dall'alto verso il basso), e quindi dobbiamo essere in grado di chiamare codice continuamente: per esempio dovremo cancellare e disegnare di nuovo la nostra bolla un po' più in basso e così via.

Dobbiamo quindi trovare il modo di chiamare il nostro codice periodicamente. Per fare questo possiamo utilizzare la funzione JavaScript `setInterval()`: questo metodo chiama una funzione o valuta un'espressione continuamente con un periodo in millisecondi specificato come argomento.

Per esempio, aggiungendo il seguente codice al nostro file `main.js`:

```javascript
var ndx = 0;
setInterval(function() {
    console.log(`setInterval(): iterazione numero ${ndx}`);
    ndx += 1;

}, 1000);
```

creiamo una funzione  che viene chiamata una volta al secondo (1000 millisecondi = 1 secondo) e che scrive una linea sulla console. Per vedere il funzionamento di questo programma occorre aprire i Developers Tool premendo il tasto F12.

Notare la linea:

```javascript
    console.log(`setInterval(): iterazione numero ${ndx}`);
```

In questa linea abbiamo utilizzato una sintassi speciale di Javascript: la [**stringa template**][StringTemplate]. Le stringhe template si riconoscono perché per definirle non si usano gli apici normali (o i doppi apici) ma i "backtick" (l'accento grave). All'interno della stringa template si possono inserire delle variabili contenute all'interno della sequenza `${}`. Nel nostro caso, per stampare all'interno della stringa il contatore `ndx`, basta usare la sintassi `${ndx}`

## TuxMath 3: Animiamo la bolla!

Adesso che abbiamo a disposizione la potenza della funzione `setInterval()` facciamoci qualcosa di più interessante che stampare del testo sulla console.

In particolare vediamo come creare l'animazione di una bolla che scende dall'alto verso il basso.

In pratica dobbiamo utilizzare due variabili per salvare la posizione della bolla, e ad ogni iterazione ripulare il canvas e disegnare nuovamente la bolla un pochino più in basso.

Per fare questo modifichiamo il nostro codice in questo modo:

Per prima cosa definiamo due variabili che contengano la posizione del centro della bolla e inizializzamoli con i valori `150` e `-25` e utilizziamo queste variabili per disegnare bolla e testo. Spostiamo poi le definizioni delle variabili op e txtwidth:

```javascript
var center_x = 150;  // <<< Codice nuovo >>>
var center_y = -50;  // <<< Codice nuovo >>>
var op = '4 + 5';    // <<< riga spostata>>>
var txtwidth = ctx.measureText(op).width   // <<< riga spostata >>>
```

Spostiamo quindi il codice che disegna bolla e testo dentro la funzione `setInterval()` aggiungendoci anche una sezione che ripulisce il canvas a ogni iterazione e la riga che modifica il valore di `center_x` in modo ad ogni iterazione gli oggetti vengano disegnati un po' più in basso.

Alla fine di tutte queste operazioni il nostro codice (a parte le righe iniziali con la definizione dei colori che rimangono uguali) sarà diventato il seguente:

```javascript
var canvas = document.getElementById('tuxmathcanvas');
var ctx = canvas.getContext('2d');
var circlecolor = redcolor;
var textcolor = base02color;
ctx.font = '25px Verdana';
ctx.textAlign = 'center';
ctx.textBaseline = 'middle';

var center_x = 150;
var center_y = -50;
var op = '4 + 5';
var txtwidth = ctx.measureText(op).width

setInterval(function() {

    // ripuliamo il canvas
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    ctx.beginPath();
    ctx.rect(0, 0, canvas.width, canvas.height, base3color);
    ctx.fillStyle = 'white';
    ctx.fill();
    ctx.stroke();

    // ridisegniamo il tutto
    ctx.beginPath();
    ctx.strokeStyle = circlecolor;
    ctx.fillStyle = circlecolor;
    ctx.arc(center_x, center_y, 50, 0, 2 * Math.PI, false);
    ctx.fill();
    ctx.stroke();
    ctx.fillStyle = textcolor;
    ctx.fillText(op, center_x, center_y);

    // calcoliamo la nuova posizione che verrà utilizzata al prossimo ciclo
    center_y += 1;


}, 10);
```

Dopo questa modifica quando la pagina verrà ricaricata la bolla e il testo saranno inizialmente scomparsi, ma ben presto appariranno nella parte alta dello schermo per scendere verso il basso. Per capire questo comportamente bisogna anche tenere presente che in un canvas l'origine degli assi (il punto [0,0]) sta nell'angolo in alto a sinistra, che le ascisse aumentano andando da sinistra a destra e le ordinate andando dall'alto verso il basso.

Provare a ricaricare la pagina per credere!

## Un po' di "refactoring"

Dopo questa fase di sviluppo un po' selvaggio, i tempi sono maturi per fare un'operazione tipica dei pro: un po' di **refactoring** del codice. Questo per prepararci a introdurre le nuove funzionalità più facilmente.

Per prima cosa definiamo una "Classe" che ci permetta di rappresentare in forma astratta una delle nostre bolle con operazione. Nell'ottica di strutturare meglio il codice creiamo dentro la directory `src/js` un nuovo file `bubble.js` e scriviamoci all'interno il codice che segue.

**NOTA** ricordatevi [che i migliori programmatori sono pigri][LazyProgrammer], quindi nello scrivere questa classe non ripartite da zero ma  cercate dove possibile di ricopiare il codice scritto finora adattandolo. **EVITATE PERO' DI FARE COPY & PASTE DEL CODICE DA QUESTA PAGINA: QUESTO NON SIGNIFICHEREBBE ESSERE PIGRI BENSI' STUPIDI: SE NON SCRIVETE IL CODICE DA SOLI NON IMPARERETE MAI A PROGRAMMARE. ANZI SE C'E' QUALCOSA CHE NON CAPITE NEL CODICE CHE SCRIVETE CHIEDETE AI MENTOR!**


```javascript
// bubble.js
export default class Bubble {

    constructor(bubbleColor, textColor, font, radius, sizeCanvas_x, sizeCanvas_y) {
        this.center_x = 0;
        this.center_y = 0;
        this.radius = radius;
        this.bubbleColor = bubbleColor;
        this.textColor = textColor;
        this.addends = [0, 0];
        this.opString = "";
        this.result = 0;
        this.textFont = font;
        this.sizeCanvas_x = 0;
        this.sizeCanvas_y = 0;
        this.speed = 0;

        // salva la dimensione del canvas (ci servira' in seguito)
        this.sizeCanvas_x = sizeCanvas_x;
        this.sizeCanvas_y = sizeCanvas_y;

        // inizializza la bolla
        this.init();
    }

    init() {

        // calcola una posizione casuale per la bolla
        // posizione x casuale tra [0 + radius] e [sizeCanvas_x - radius]
        // posizione y = -radius
        this.center_x = Math.floor(Math.random() * (this.sizeCanvas_x - 2 * this.radius)) + this.radius;
        this.center_y = -this.radius;

        // calcola due addendi casuali tra 1 e 9, salva il risultato dell'operazione
        // e prepara un stringa con l'operazione
        this.addends[0] = Math.ceil(Math.random() * 9);
        this.addends[1] = Math.ceil(Math.random() * 9);
        this.result = this.addends[0] + this.addends[1];
        this.opString = `${this.addends[0]} + ${this.addends[1]}`;

        // regola la velocita' casualmente tra 1 e 5
        this.speed = Math.ceil(Math.random() * 5);

    }

    computeResult() {
        this.result = 0;
        for (i = 0; i < this.addends.number; i++) {
            this.result += addends[i];
        }
    }

    moveDownAndDraw(ctx) {

        this.center_y += this.speed;

        ctx.beginPath();
        ctx.strokeStyle = this.bubbleColor;
        ctx.fillStyle = this.bubbleColor;
        ctx.arc(this.center_x, this.center_y, this.radius, 0, 2 * Math.PI, false);
        ctx.fill();
        ctx.stroke();
        ctx.font = this.textFont;
        ctx.textAlign = 'center';
        ctx.textBaseline = 'middle';
        ctx.fillStyle = this.textColor;
        ctx.fillText(this.opString, this.center_x, this.center_y);

        // se la bolla ha raggiunto il fondo, ne reinizializziamo una nuova
        if (this.center_y > this.sizeCanvas_y)
            this.init(this.sizeCanvas_x, this.sizeCanvas_y);

    }
}
```

Adesso tutto il codice che ci permette di rappresentare la bolla è contenuto in questo file (e sarà molto più facile estendere il nostro programma nel seguito, per esempio se volessimo far scendere più di una bolla per volta), e nel file principale `main.js` rimane solo il seguente codice:

```javascript
// main.js
import '../style.css';
import Bubble from './bubble.js';

const base03color  = '#002b36';
const base02color  = '#073642';
const base01color  = '#586e75';
const base00color  = '#657b83';
const base0color   = '#839496';
const base1color   = '#93a1a1';
const base2color   = '#eee8d5';
const base3color   = '#fdf6e3';
const yellowcolor  = '#b58900';
const orangecolor  = '#cb4b16';
const redcolor     = '#dc322f';
const magentacolor = '#d33682';
const violetcolor  = '#6c71c4';
const bluecolor    = '#268bd2';
const cyancolor    = '#2aa198';
const greencolor   = '#859900';

var canvas = document.getElementById('tuxmathcanvas');
var ctx = canvas.getContext('2d');

// dichiariamo una bolla
var bubble = new Bubble(redcolor, base02color,
    '25px Verdana', 50, canvas.width, canvas.height);

// funzione chiamata periodicamente
setInterval(function() {

    // ripuliamo il canvas
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    ctx.beginPath();
    ctx.rect(0,0,canvas.width,canvas.height, base3color);
    ctx.fillStyle = base3color;
    ctx.fill();
    ctx.stroke();

    // ridisegniamo la bolla facendola scendere di un passo
    bubble.moveDownAndDraw(ctx);

}, 10);
```

## Leggere input da tastiera

**TO BE COMPLETED**

## Controllare se abbiamo indovinato il risultato e aggiornamento del punteggio

**TO BE COMPLETED**




[TuxMath]: https://en.wikipedia.org/wiki/Tux,_of_Math_Command
[Vim]: https://www.vim.org/
[VSCode]: https://code.visualstudio.com/
[Yarn]: https://yarnpkg.com/lang/en/
[NodeJS]: https://nodejs.org/en/
[jsmodernsetup]: https://fpiantini.github.io/javascript/nodejs/programming/webpack/web/2019/04/01/nodejs-project-modern-setup.html
[MDNwebdocs]: https://developer.mozilla.org/it/
[w3schools.com]: https://www.w3schools.com/
[w3schoolsCanvas]: https://www.w3schools.com/tags/ref_canvas.asp
[stringTemplate]: https://developer.mozilla.org/it/docs/Web/JavaScript/Reference/template_strings
[LazyProgrammer]: http://blogoscoped.com/archive/2005-08-24-n14.html