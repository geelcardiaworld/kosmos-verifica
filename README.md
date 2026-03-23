# Kòsmos Card — Pagina di Verifica

Pagina di verifica pubblica per le Kòsmos Card del sistema Patron di **Elcardiano**.  
Ospitata su GitHub Pages, permette a chiunque di controllare l'autenticità di una Kòsmos Card inserendo il codice a 7 caratteri.

## Struttura

```
/
├── index.html       ← Pagina di verifica (file unico, nessuna dipendenza)
├── patrons.json     ← Registro dei patron (da aggiornare mensilmente)
└── README.md        ← Questo file
```

## Deployment su GitHub Pages

### Prima volta

1. Crea un nuovo repository su GitHub (es. `kosmos-verifica`)
2. Carica `index.html`, `patrons.json` e `README.md`
3. Vai su **Settings → Pages**
4. In "Source" seleziona **Deploy from a branch**
5. Seleziona il branch `main` e la cartella `/ (root)`
6. Salva — il sito sarà online in pochi minuti all'indirizzo:
   ```
   https://tuousername.github.io/kosmos-verifica/
   ```

### Aggiornamento mensile

Ogni volta che aggiungi, rimuovi o modifichi un patron, modifica il file `patrons.json` e fai commit. GitHub Pages si aggiorna automaticamente in 1-2 minuti.

## Formato di `patrons.json`

Il file è un array JSON. Ogni oggetto rappresenta un patron:

```json
{
  "code": "K7X9M2W",     // Codice univoco a 7 caratteri (MAIUSCOLE + NUMERI)
  "tier": "O",            // Lettera tier: R (Rame), A (Argento), O (Oro), P (Platino), E (Elettro)
  "months": 3,            // Mesi consecutivi di abbonamento
  "since": "Marzo 2026",  // Mese e anno della prima iscrizione
  "period": "2603",        // Periodo corrente (AAMM)
  "active": true           // true = abbonamento attivo, false = scaduto/cancellato
}
```

### Operazioni comuni

**Aggiungere un nuovo patron:**
```json
{
  "code": "NUOVO01",
  "tier": "R",
  "months": 1,
  "since": "Aprile 2026",
  "period": "2604",
  "active": true
}
```

**Patron che cancella l'abbonamento:**  
Non rimuovere la riga — imposta `"active": false`. Il sistema lo mostrerà come "abbonamento scaduto" se qualcuno verifica il codice.

**Patron che cambia tier (upgrade/downgrade):**  
Modifica solo il campo `"tier"` con la nuova lettera.

**Aggiornamento mensile (ogni 1° del mese):**
- Incrementa `"months"` di 1 per tutti i patron attivi
- Aggiorna `"period"` con il nuovo AAMM
- Imposta `"active": false` per chi non ha rinnovato
- Aggiungi le righe per i nuovi abbonati del mese

## Link diretto di verifica (per QR Code)

La pagina supporta la verifica automatica via URL. Formato:

```
https://tuousername.github.io/kosmos-verifica/?code=K7X9M2W
```

Questo link può essere inserito in un QR Code sulla Kòsmos Card.  
Quando qualcuno lo apre (o scansiona il QR), la verifica parte automaticamente.

## Automazione futura

Quando il numero di patron crescerà, potrai automatizzare l'aggiornamento di `patrons.json` con una GitHub Action che legge dal Google Sheet via API, genera il JSON aggiornato, e fa commit automaticamente. Chiedi al Team Simulacrum per la configurazione.

## Licenza

Uso riservato — Elcardiano. Tutti i diritti riservati.
