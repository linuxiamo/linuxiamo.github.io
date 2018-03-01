# Come usare il CSS per generare pagine da stampare in PDF

Questa guida sull'uso del codice CSS per generare pagine pronte per l'uso stampa in PDF, è tratta da [questo articolo](https://www.smashingmagazine.com/2015/01/designing-for-print-with-css/) di Smashing Magazine.

## Dimensione e orientamento della pagina

La dimensione e l'orientamento della pagina possono essere definiti come segue:

```css
/* Pagina A4 verticale */
@page { size: A4 portrait; }
```

Oppure:

```css
/* Pagina A4 orizzontale, specificando la lunghezza dei lati del foglio */
@page { size: 210mm 297mm landscape; }
```

## Sezioni di una pagina

Ogni pagina è divisa in sezioni. 

Oltre al corpo centrale (_area della pagina_), si riconoscono 16 sezioni in cui i margini sui quattro lati possono essere suddivisi (_margin boxes_). Per chiarezza si veda l'apposito [link](https://www.w3.org/TR/css3-page/#margin-boxes) della W3C.

Le _margin boxes_ sono usate esclusivamente per i contenuti generati via CSS.

### Nomi delle Margin boxes

<table style="margin-left:0px;border-collapse:collapse;border-width:1px;border-style:solid;">
<colgroup><col style="width:108px; border-width:1px; border-style:solid">
<col style="width:108px; border-width:1px; border-style:solid">
<col style="width:108px; border-width:1px; border-style:solid">
<col style="width:108px; border-width:1px; border-style:solid">
<col style="width:108px; border-width:1px; border-style:solid">
</colgroup><tbody><tr style="height:75px; border-width:1px; border-style:solid; text-align:center;">
<td>top-left-corner</td>
<td>top-left</td>
<td>top-center</td>
<td>top-right</td>
<td>top-right-corner</td></tr>
<tr style="height:75px; border-width:1px; border-style:solid; text-align:center;">
<td>left-top</td>
<td colspan="3" rowspan="3">main page area</td>
<td>right-top</td></tr>
<tr style="height:75px; border-width:1px; border-style:solid; text-align:center;">
<td>left-middle</td>
<td>right-middle</td></tr>
<tr style="height:75px; border-width:1px; border-style:solid; text-align:center;">
<td>left-bottom</td>
<td>right-bottom</td></tr>
<tr style="height:75px; border-width:1px; border-style:solid; text-align:center;">
<td>bottom-left-corner</td>
<td>bottom-left</td>
<td>bottom-center</td>
<td>bottom-right</td>
<td>bottom-right-corner</td></tr></tbody></table>

## Pseudo-class selector per le pagine

Esistono selettori per modificare i margini della pagina destra o sinistra di un documento (rilegato):

```css
@page :left { margin-left: 3cm; } 
@page :right { margin-left: 4cm; }
```

Oppure per modificare il comportamento della prima pagina:

```css
@page :first { }
```

O ancora di una pagina lasciata intenzionalmente vuota (in questo caso inseriamo un testo al centro del margine superiore per identificarla):

```css
@page :blank { @top-center { content: "This page is intentionally left blank." } }
```

### Esempio: come inserire il titolo del libro in alto a sinistra

In questo esempio vediamo come inserire il titolo del libro in alto a sinistra, sulla pagina destra:
```css
@page:right{ 
  @bottom-left {
    margin: 10pt 0 30pt 0;
    border-top: .25pt solid #666;
    content: "Book title";
    font-size: 9pt;
    color: #333;
  }
}
```

## Interruzioni di pagina

Interrompi pagina sempre prima di un H1:

```css
h1 {
  page-break-before: always;
}
```

Interrompi pagina sempre dopo le intestazioni:

```css
h1, h2, h3, h4, h5 {
  page-break-after: avoid;
}
```

Evita interruzione di pagina durante immagini e tabelle:
```css
table, figure {
  page-break-inside: avoid;
}
```

## Contatori di elementi (pagine, figure, capitoli...)

In CSS sono disponibili contatori automatici di elementi, vedi [questa guida](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Lists_and_Counters/Using_CSS_counters) di Mozilla.

I contatori si inizializzano e incrementano con i comandi:

```css
/* Il contatore inizializzato vale 0 */
body { counter-reset: nomecontatore; }

/* Può essere incrementato usando: */
h3 { counter-increment: nomecontatore; }

/* e può essere mostrato con il comando: */
h3::before { content: counter(nomecontatore); }
```

Ad ogni visualizzazione di H3, il contatore viene incrementato di 1.

### Esempio: contatori su liste numerate

Codice CSS per un contatore di nome `section` da applicare a liste ordinate:

```css
/* Creates a new instance of the section counter with each ol element */
ol {
  counter-reset: section; 
  list-style-type: none;
}

li::before {
/* Increments only this instance of the section counter */
  counter-increment: section;
/* Combines the values of all instances of the section counter, separated by a period */
  content: counters(section, ".") " ";
}
```

Implementazione in HTML:

```html
<ol>
  <li>item</li>          <!-- 1     -->
  <li>item               <!-- 2     -->
    <ol>
      <li>item</li>      <!-- 2.1   -->
      <li>item</li>      <!-- 2.2   -->
      <li>item           <!-- 2.3   -->
        <ol>
          <li>item</li>  <!-- 2.3.1 -->
          <li>item</li>  <!-- 2.3.2 -->
        </ol>
        <ol>
          <li>item</li>  <!-- 2.3.1 -->
          <li>item</li>  <!-- 2.3.2 -->
          <li>item</li>  <!-- 2.3.3 -->
        </ol>
      </li>
      <li>item</li>      <!-- 2.4   -->
    </ol>
  </li>
  <li>item</li>          <!-- 3     -->
  <li>item</li>          <!-- 4     -->
</ol>
<ol>
  <li>item</li>          <!-- 1     -->
  <li>item</li>          <!-- 2     -->
</ol>
```

### Esempio: inserire il numero di pagina in basso a lato della pagina

Per inserire il numero di pagina in basso a lato della pagina destra e sinistra, in CSS:

```css
@page:right{
  @bottom-right {
    content: counter(page);
  }
}

@page:left{
  @bottom-left {
    content: counter(page);
  }
}
```

Per scrivere "Pag. 3 di 120":

```css
@page:left{
  @bottom-left {
    content: "Pag. " counter(page) " di " counter(pages);
  }
}
```

La stessa cosa è possibile farla con i numeri dei capitoli o delle figure:

```css
body {
  counter-reset: chapternum;
}

h1.chapter:before {
  counter-increment: chapternum;
  content: counter(chapternum) ". ";
}
```

Oppure:

```css
body {
  counter-reset: chapternum figurenum;
}

h1 {
  counter-reset: figurenum;
}

h1.title:before {
  counter-increment: chapternum;
  content: counter(chapternum) ". ";
}

figcaption:before {
  counter-increment: figurenum;
  content: counter(chapternum) "-" counter(figurenum) ". ";
}
```

## Inserire stringhe predefinite nel documento via CSS

È possibile definire un testo personalizzato da inserire tramite CSS nel documento, attraverso l'uso del comando `string-set` seguito dal nome della variabile e da `content()`.

Con questo codice scriviamo il contenuto del tag H1 in `doctitle` e poi lo stampiamo in alto a destra della pagina destra. Il valore cambia ad ogni H1 che inseriamo.

```css
h1 { 
  string-set: doctitle content(); 
}

@page :right {
  @top-right {
    content: string(doctitle);
    margin: 30pt 0 10pt 0;
    font-size: 8pt;
  }
}
```

## Note a piè di pagina

Le note a piè di pagina sono realizzate attraverso uno standard specificato in [questo documento](http://www.w3.org/TR/css-gcpm-3/#footnotes) della W3C che descrive il _CSS generated content for Paged Media Module_.

L'uso delle note a piè pagina segue l'implementazione:
- Inline: il contenuto della nota è inserito in un elemento come SPAN, a cui è assegnata una classe per identificarlo come nota piè pagina
- Il contenuto della nota è poi trasformato da questo tag in una _footnote_ vera e propria.

In CSS:

```css
.fn {
  float: footnote;
}
```

In HTML:

```html
<p>Footnotes<span class="fn">Footnotes and notes placed in the footer of a document to reference the text. The footnote will be removed from the flow when the page is created.</span> are useful in books and printed documents.</p>
```

Le note a pié pagina, hanno un contatore predefinito che può essere resettato ad ogni occorrenza di H1 (verosimilmente l'inizio di un nuovo capitolo):

```css

.fn {
  counter-increment: footnote;
}

h1 {
  counter-reset: footnote;
}
```

Le note a piè pagina hanno poi un numerino in corrispondenza del punto in cui fare il rimando. Ciò può essere realizzato con `footnote-call`:

```css
.fn::footnote-call {
  content: counter(footnote);
  font-size: 9pt;
  vertical-align: super;
  line-height: none;
}
```

Per riportare questo numerino a piè pagina, prima dell'indicazione della nota, si può usare `footnote-marker`:

```css
.fn::footnote-marker {
  font-weight: bold;
}
```

Le note a piè pagina sono piazzate in un'area speciale dei margini chiamata `@footnote` che è possibile, ovviamente, stilizzare:

```css
@page {
  @footnote {
    border-top: 1pt solid black;
  }
}
```

## Come generare riferimenti automatici ai contatori

In questo paragrafo, vediamo come generare riferimenti automatici ai vari contatori presenti nel documento, ad esempio per creare un sommario con i nomi dei capitoli e i numeri di pagina relativi.

Per fare questo bisogna creare delle ancore nell'html:

```html
<a class="xref" href="#ch1" title="Chapter 1">Chapter 1</a>
```

E poi usare la proprietà `target-counter` per legare il contatore:

```css
a.xref:after {
  content: " (page " target-counter(attr(href, url), page) ")";
}
```

## Come realizzare un PDF usando queste tecniche

Per realizzare un PDF a partire dal nostro documento HTML, è possibile utilizzare lo user agent _Prince_, disponibile all'indirizzo seguente: [http://www.princexml.com/](http://www.princexml.com/).
