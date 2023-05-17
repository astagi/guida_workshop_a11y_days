# Guida workshop per gli Accessibility Days 19/05/2023 a Roma

## Obiettivo principale

Costruire una single page application (SPA) con l'utilizzo di [Bootstrap Italia](https://italia.github.io/bootstrap-italia/) che abbia lo scopo di visualizzare tutte le notizie prese dal sito [Ansa.it](https://ansa.it) di una determinata categoria (ad esempio politica, cultura, sport..) o di una determinata regione italiana (Abruzzo, Toscana, Lombardia, Sicilia...).

<img width="1369" alt="image" src="https://github.com/astagi/guida_workshop_a11y_days/assets/537363/e784b46c-0ddb-4c2e-a544-97072b0f6c17">

## Librerie da utilizzare

- [Bootstrap Italia](https://github.com/italia/bootstrap-italia)

🔧 Potete installare la libreria includendo stili e script JavaScript all'interno della vostra pagina web

```html
<link href="https://unpkg.com/bootstrap-italia@2.4.2/dist/css/bootstrap-italia.min.css" rel="stylesheet">


<script src="https://unpkg.com/bootstrap-italia@2.4.2/dist/js/bootstrap-italia.bundle.min.js"></script>
```

⚠️ A causa di problemi con CORS per utilizzare le SVG è necessario scaricare in locale il file [sprites.svg da unpkg.com](https://unpkg.com/browse/bootstrap-italia@2.4.2/dist/svg/sprites.svg).

- [Ansa.js](https://github.com/astagi/ansa.js)

🔧 Potete installare la libreria includendo lo script JavaScript all'interno della vostra pagina web

```html
<script src="https://unpkg.com/ansa@0.0.1/bundle.js"></script>
```

👉🏻 La libreria presenta solo l'oggetto `Ansa` contenente il metodo `getNews` al quale possiamo passare una categoria o una regione per ottenere le notizie.

```js
Ansa.getNews('Toscana').then((res) => {
    console.log(res.items)
})
```

⚠️ Per via delle tempistiche strette consiglio di utilizzare solo alcuni valori come `Lazio`, `Politica`, `Sport`.

Nella variabile `res.items` troverete un array di notizie che potete far visualizzare in pagina. Ogni elemento contiene tutte le informazioni della notizia, ad esempio

```json
{
    "id": "cronaca_0",
    "title": "Multa a pensionato che ripara buca, eurodeputato paga la multa",
    "description": "Ciocca, cittadino con senso civico va ringraziato",
    "link": "https://www.ansa.it/sito/notizie/cronaca/2023/05/16/multa-a-pensionato-che-ripara-buca-eurodeputato-paga-la-multa_df716d81-08d4-4936-8002-4b0177cf62a4.html",
    "published": 1684261979000,
    "created": 1684261979000,
    "media": "https://www.ansa.it/webimages/img_457x/2020/2/22/c9093ff69955180f720e3b5fbd1ab6a8.jpg",
    "text": "Il testo della notizia",
    "daypublished": "16",
    "monthpublished": "maggio"
},
```

- (Facoltativo) Una libreria semplice di rendering template (come [Mustache](https://github.com/janl/mustache.js/))

⚠️ Una libreria di rendering template può essere utile per renderizzare una serie di elementi che arrivano in asincrono dal una richiesta HTTP e che non sono decisi a priori, come in questo caso le notizie. Una libreria molto interessante e adatta a questo scopop può essere Mustache.js ma potete anche agire senza di essa utilizzando JavaScript e `innerHTML`.

🔧 Potete installare Mustache.js includendo lo script JavaScript all'interno della vostra pagina web

```html
<script src="https://unpkg.com/mustache@latest"></script>
```

👉🏻 Per utilizzare Mustache dovete definire un template nella pagina web

```html
<script id="templateCardsNews" type="x-tmpl-mustache">
    {{#news}}
    <div>
        <h3>{{title}}</h3>
        <p>{{description}}</p>
    </div>
    {{/news}}
</script>
```

per poi utilizzarlo quando i dati sono 

```js
Ansa.getNews(value).then((res) => {
    // Prendi il template definito sopra
    const template = document.getElementById('templateCardsNews').innerHTML;
    // Renderizzalo (in questo caso passandogli le news
    const rendered = Mustache.render(template, { news: res.items });
    // Prendi l'elemento e inseriscici il template renderizzato
    document.getElementById('newsList').innerHTML = rendered;
})
```

## Componenti che necessitano di attenzione

- L'autocomplete contenente il campo di ricerca per categoria o regione delle notizie deve necessariamente stare dentro un form 

```html
<form id="searchForm" action="index.html" method="post">
    <div class="select-wrapper">
        <label for="accessibleAutocomplete">Cerca le news per categoria o perregione.</label>
        <select class="form-control" id="accessibleAutocomplete" title="Scegli una provincia" required>
            <option value='Toscana'>Toscana</option>
            <option value='Politica'>Politica</option>
            <option value='Sport'>Sport</option>
        </select>
    </div>
    <button class="btn btn-primary mt-3" type="submit" data-focus-mouse="false">
        Cerca le notizie
    </button>
</form>
```

e deve essere inizializzato tramite JavaScript insieme alla validazione. Si rimanda alla [documentazione dell'Autocomplete](https://italia.github.io/bootstrap-italia/docs/form/autocompletamento/).

```js 
// Inizializza il motore di validazione del form
const validate = new bootstrap.FormValidate('#searchForm', {
    errorFieldCssClass: 'is-invalid',
    errorLabelCssClass: 'form-feedback',
    errorLabelStyle: '',
    focusInvalidField: false,
})
// Imposta il comportamento della validazione
validate
    .addField('#accessibleAutocomplete', [
        {
            rule: 'required',
            errorMessage: 'Questo campo è richiesto',
        },
        {
            errorMessage: "Seleziona un'opzione fra quelle disponibili",
            validator: bootstrap.ValidatorSelectAutocomplete('#accessibleAutocomplete'),
        },
    ])
    .onSuccess((event) => {
        const e = selectAutocomplete._element;
        const value = e.options[e.selectedIndex].value;
        // Da qui il value può essere utilizzato per prendere le notizie con la libreria Ansa.js
    })
```

- Per visualizzare le notizie potete utilizzare una lista di card con immagine. Per capire come inizializzare le card si rimanda alla [documentazione delle card con immagine](https://italia.github.io/bootstrap-italia/docs/componenti/card/#card-con-immagine)

<img width="460" alt="image" src="https://github.com/astagi/guida_workshop_a11y_days/assets/537363/01ff5bd5-37e3-455d-9d47-c46fb016cd3a">

Nell'esempio sopra è stato inserito anche un Accordion (documentazione [qui](https://italia.github.io/bootstrap-italia/docs/componenti/accordion/)) per far apparire/scomparire il testo della notizia a discrezione dell'utente.
