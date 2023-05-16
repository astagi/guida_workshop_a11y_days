# Guida workshop per gli Accessibility Days 19/05/2023 a Roma

## Obiettivo principale

Costruire una single page application (SPA) con l'utilizzo di [Bootstrap Italia](https://italia.github.io/bootstrap-italia/) che abbia lo scopo di visualizzare tutte le notizie prese dal sito [Ansa.it](https://ansa.it) di una determinata categoria (ad esempio politica, cultura, sport..) o di una determinata regione italiana (Abruzzo, Toscana, Lombardia, Sicilia...).

## Librerie da utilizzare

- Bootstrap Italia

🔧 Potete installare l'applicazione direttamente includendo stili e script JavaScript all'interno della vostra pagina web

- Ansa.js

🔧 Potete installare l'applicazione direttamente includendo lo script JavaScript all'interno della vostra pagina web

```html
<script src="https://unpkg.com/ansa@0.0.1/bundle.js"></script>
```

👉🏻 La libreria presenta solo l'oggetto `Ansa` contenente il metodo `getNews` al quale possiamo passare una categoria o una regione per ottenere le notizie.

```js
Ansa.getNews('Toscana').then((res) => {
    console.log(res.items)
})
```

⚠️ Per semplicità e tempistiche consiglio di utilizzare solo alcuni valori come `Toscana`, `Politica`, `Sport`.

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
