# Daily Games

Landing page di [dailygames-hub.github.io](https://dailygames-hub.github.io): piccoli giochi gratuiti da browser con una sfida nuova ogni giorno.

Ogni gioco vive nel proprio repository dell'organizzazione ed è pubblicato con GitHub Pages come sottocartella del sito radice (per esempio `/dotto/`). L'elenco dei giochi in home page si aggiorna da solo.

## Come funziona l'elenco automatico

`index.html` legge `games.json` e disegna una scheda per ogni gioco. Il file `games.json` viene rigenerato dalla GitHub Action `.github/workflows/update-games.yml`, che ogni 6 ore (o quando la lanci a mano da Actions, "Aggiorna elenco giochi", Run workflow):

1. elenca i repository pubblici dell'organizzazione;
2. per ognuno cerca un file `game.json` nella radice del branch principale;
3. rigenera `games.json` con i giochi trovati, ordinati per `order` e poi per nome.

Se non trova nessun `game.json` non tocca nulla. I workflow a orario vengono sospesi da GitHub dopo 60 giorni senza commit nel repository: basta lanciarlo a mano una volta per riattivarlo.

## Aggiungere un gioco

1. Crea un repository pubblico nell'organizzazione con il nome del gioco in minuscolo (diventa l'indirizzo: `dailygames-hub.github.io/nome/`).
2. Attiva GitHub Pages (Settings, Pages, Deploy from a branch, `main`, root).
3. Metti nella radice un file `game.json`:

```json
{
  "name": "NOME",
  "description": { "it": "Una riga che spiega il gioco.", "en": "One line describing the game." },
  "accent": "#ffb324",
  "languages": ["it", "en"],
  "daily": true,
  "preview": "preview.svg",
  "order": 2
}
```

Campi: `name` (testo o oggetto `{it, en}`), `description` (idem), `accent` (colore esadecimale usato per il pulsante e il pallino della scheda), `languages`, `daily` (mostra l'etichetta "ogni giorno"), `preview` (immagine nella radice del gioco, ideale 480x220, oppure un URL https), `order` (posizione in elenco), `hidden: true` per nasconderlo, `path` per un percorso diverso dal nome del repository.

4. Aspetta il prossimo giro dell'Action o lanciala a mano. La scheda compare in home page.

## File

- `index.html`: landing page (IT/EN), legge `games.json`
- `games.json`: elenco dei giochi, generato dall'Action (si può anche modificare a mano)
- `privacy.html`: informativa sulla privacy, con la sezione sugli annunci già pronta da attivare
- `favicon.svg`, `fonts/`: icona e carattere Bricolage Grotesque (SIL OFL)
- `.github/workflows/update-games.yml`: l'Action che aggiorna l'elenco
