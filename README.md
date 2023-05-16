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
            "text": "\n\t\t\t\t\t\t\t\t\t\t\n\n            \n            (ANSA) - MILANO, 16 MAG - L'europarlamentare pavese della\nLega, Angelo Ciocca, si è impegnato a pagare la multa di oltre\n800 euro a Claudio Trenta, il pensionato che ha riparato una\nbuca a Barlassina (Monza) ed è stato sanzionato dal Comune.   \n\t\t\t\tNotizia diventata virale nei giorni scorsi. Lo ha promessa in\nun'intervista nella  trasmissione \"Lombardia nera\", in onda\nquesta sera a partire dalle 20.30 su Antenna.   \n\t\t\t\t   \"Io penso che un cittadino che con senso civico ha segnalato\nun problema e poi, preso probabilmente dalla disperazione, ha\ndeciso di chiuderla vada ringraziato - ha affermato il politico\n-  Visto che noi abbiamo una indennità importante come\nparlamentari europei io mi sento in dovere di pagare la multa a\nquesto signore. Mi impegno a farlo, perché a me sembra una cosa\ningiusta lasciare la multa sulle spalle di un pensionato che\ncompie un'opera di bene. Per me il signor Trenta ha fatto\nun'opera di buon senso, ovvero agire davanti al pericolo.   \n\t\t\t\tSiccome per la sua famiglia e sua moglie questa multa è un\npensiero non posso far altro, per ringraziarlo di quello che ha\nfatto - di togliergli il pensiero di una multa ingiusta. Al\nmassimo avrebbero dovuto mandargli una diffida\".   \n\t\t\t\t   \"La prima regola - concluso Ciocca - è garantire la sicurezza\nsulle strade, il sindaco avrebbe dovuto preoccuparsi della buca\nche gli è stata segnalata più volte. Se in quella buca cadeva un\nbambino, un ciclista, un motociclista o un anziano oggi\nprobabilmente il sindaco avrebbe problemi più seri che\nrincorrere chi ha chiuso la buca. A me sembra una barzelletta di\nPierino\". (ANSA).   \n\t\t\t\t          \n\n\n\t\t\t\t\t\t\t\t\t",
            "daypublished": "16",
            "monthpublished": "maggio"
        },
```
