# jQuery cheatsheet

Per attivare jQuery, usare i seguenti CDN.

Per progetti web:

```html
<!-- Jquery minified -->
<script src="https://code.jquery.com/jquery-3.3.1.min.js" integrity="sha256-FgpCb/KJQlLNfOu91ta32o/NMZxltwRo8QtmkMRdAu8=" crossorigin="anonymous"></script>
```
Per progetti Electron:

```html
<script>window.jQuery = window.$ = require('jquery');</script>
```
Bootstrap per web o Electron: 

```html
<!-- Latest compiled and minified CSS -->
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">

<!-- Optional theme -->
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap-theme.min.css" integrity="sha384-rHyoN1iRsVXV4nD0JutlnGaslCJuC7uwjduW9SVrLvRYooPp2bWYgmgJQIXwl/Sp" crossorigin="anonymous">

<!-- Latest compiled and minified JavaScript -->
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js" integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

## Javascript

Domanda OK | Cancel:

```javascript
function askConfirm(variabile) {
        
    // http://www.w3schools.com/js/js_popup.asp
    
    var risposta = confirm("Press a button");
    if (risposta == true) {
        risultato = "Hai premuto OK.";
    } else {
        risultato = "Hai premuto Cancel.";
    } 
    
}
```

Lettura dei valori GET passati alla pagina:

```javascript
window.onload = function() {

    // Recupero i valori passati con GET
    var get = parseGetVars();

    // estraggo dall'array contenente uno dei valori della querystring
    var variabile = get['nome_variabile_query_string'];

    if (nome_variabile_query_string != null) {

    }
};
```

## Uso di jQuery

In jQuery viene manipolato il DOM della pagina. 

jQuery può essere individuato dalla parola `jQuery` o dal simbolo `$`.

L'uso di base di jQuery avviene inserendo tutti i comandi all'interno di:

```javascript
jQuery(document).ready(function(){
	// qui va il codice da eseguire
});
```

jQuery consente di selezionare gli elementi, in modi differenti:

```javascript
// Modalità di selezione avanzata degli elementi: per ID e per class
$("#container");	// selezione di un elemento ID
$(".articles");		// selezione di una classe

// Descendant selector, ovvero selezionare un elemento all'interno di un ID o classe (2)
$("#container li");		// container è l'id, li è l'elemento selezionato (seleziona tutti i li)
$("#container > li");	// container è l'id, li è l'elemento selezionato (seleziona solo i li suoi figli, non i nipoti)

// Selezionare elementi sia per id che per classe
$("#container, .promo");

// Selezionare un elemento dalla lista
$("#destination li:first");	// primo
$("#destination li:odd");	// dispari
$("#destination li:even");	// pari
$("#destination li:last");	// ultimo
```

### Cambiamento del testo in un tag 
```javascript
// Come modificare il testo di un elemento:
$("h1").text("nuovo testo");

$("#id").html(variabile)
```

### Evento Click su un elemento

Click su un elemento tipo **button** o altro:

```javascript
$('#id').click(function() {
        
});
```

Click su un elemento **radio**:

```javascript
$('#id input:radio').click(function() {
    
    switch($(this).val()) {
    
        case '1':	// caso 1
        break;
        
        case '2':	// caso 2 e 3 (mancando il break a caso 2)
        case '3':
        break;
        
        default:    // caso di default
            
    } // end switch
    
});
```

### AJAX in jQuery

Inviare una richiesta POST ad una pagina php, attraverso il click di un pulsante: 

```javascript
$("#id").click(function(){

    pagina_di_destinazione = "pagina.php";
    query_json = {variabile1:"stringa",variabile2:"stringa"};

    $.post(pagina_di_destinazione, query_json, function(response) {
        $("#id").html(response); // scrive la risposta in un elemento
    });

});
```

### Aggiungere o eliminare le classi da un elemento

Le classi si aggiungono con `.addClass("nuova_classe1 nuova_classe2")`, si rimuovono con `.removeClass("classe_da_rimuovere1 classe_da_rimuovere2")`:

```javascript
// Rimuove e aggiunge classi selezionate
$("#id").removeClass('classe1 classe2').addClass('classe3');

// Rimuove tutte le classi e le sostituisce con una indicata, modificando l'attributo class
$("#id").attr( "class", "classe_sostitutiva_delle_altre" );
```


## Funzioni jQuery utili

```javascript
var PHP_DB_SERVER = "serv_.php";

var algoritmo = 3;
var chiam = "index.html";

varr = {};		// inizializzazione array associativo leggibile da JSON

//$(this).attr('id')
varr['campo1'] = $("#id_input_normale").val();
varr['campo2'] = $("#id_input_normale").val();
varr['campo3'] = $('input:radio[name=NOME_ELEMENTI_RADIO]:checked').val();

varr_jsonato = JSON.stringify({varr: varr});

$.post(PHP_DB_SERVER, {chiamante:chiam, algoritmo:algoritmo, varr_json: varr_jsonato},
    function(data) {
    $("#div_esito").html(data);
});
```

### Controllare che un iFrame sia stato caricato

```javascript
// Controlla se un iframe è pronto
function frameIsReady(frameId) {
    if ($("#" + frameId).ready() === true) {
        return(true);
    } else {
        return(false);
    }
}
```
