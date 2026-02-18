# Template Repo Estensioni Serie - ShiryuYomi

Questa cartella è un template pronto all'uso per creare la tua repo di estensioni serie.

---

## Struttura della cartella

```
series-extension-repo-template/
├── repo.json              ← Metadati della repo (nome, sito, fingerprint)
├── index.min.json         ← Lista delle estensioni disponibili
├── icon/                  ← Icone delle estensioni (PNG)
│   └── <pkg>.png          (es. eu.kanade.tachiyomi.animeextension.it.nomedelsito.png)
└── apk/                   ← File APK delle estensioni
    └── <nome-apk>.apk     (es. tachiyomi-it-nomedelsito-v14.1.apk)
```

---

## Cosa modificare

### 1. `repo.json` — Metadati della repo

```json
{
  "meta": {
    "name": "ShiryuYomi Serie Extensions",      ← Nome che appare nell'app
    "shortName": "SYS",                          ← Abbreviazione
    "website": "https://github.com/TUO-USERNAME/series-extensions",  ← URL del tuo repo GitHub
    "signingKeyFingerprint": "NOFINGERPRINT01"   ← Lascia così per sviluppo (bypassa la verifica firma)
  }
}
```

**Cosa cambiare:**
- `name` → il nome che vuoi dare alla tua repo
- `shortName` → abbreviazione (2-4 lettere)
- `website` → l'URL del tuo repository GitHub (es. `https://github.com/tuouser/series-extensions`)

> **Nota:** `NOFINGERPRINT01` funziona per testing perché il codice dell'app accetta qualsiasi fingerprint che inizia con `NOFINGERPRINT`. In produzione dovresti usare il vero SHA-256 fingerprint della chiave con cui firmi gli APK.

---

### 2. `index.min.json` — Lista estensioni

Questo è un array JSON. Ogni oggetto è un'estensione:

```json
[
  {
    "name": "Aniyomi: NomeDelSito",
    "pkg": "eu.kanade.tachiyomi.animeextension.it.nomedelsito",
    "apk": "tachiyomi-it-nomedelsito-v14.1.apk",
    "lang": "it",
    "code": 1,
    "version": "14.1",
    "nsfw": 0,
    "sources": [
      {
        "id": 1000000001,
        "lang": "it",
        "name": "NomeDelSito",
        "baseUrl": "https://www.esempio-sito.com"
      }
    ]
  }
]
```

**Cosa cambiare per ogni estensione:**

| Campo | Cosa metterci | Esempio |
|-------|--------------|---------|
| `name` | `"Aniyomi: "` + nome del sito. **DEVE** iniziare con `Aniyomi: ` | `"Aniyomi: StreamingCommunity"` |
| `pkg` | Package name univoco. Formato: `eu.kanade.tachiyomi.animeextension.<lang>.<nomesito>` (tutto minuscolo, no spazi) | `"eu.kanade.tachiyomi.animeextension.it.streamingcommunity"` |
| `apk` | Nome del file APK nella cartella `apk/` | `"tachiyomi-it-streamingcommunity-v14.1.apk"` |
| `lang` | Codice lingua: `"it"` = italiano, `"en"` = inglese, `"all"` = tutte | `"it"` |
| `code` | Numero versione intero. Incrementalo ad ogni aggiornamento (1, 2, 3...) | `1` |
| `version` | Versione nel formato `"<libVersion>.<patchVersion>"`. La **lib version** deve essere tra **12** e **16** | `"14.1"` |
| `nsfw` | `0` = contenuto sicuro, `1` = contenuto NSFW | `0` |
| `sources[].id` | ID numerico univoco (Long). Inventane uno grande e unico | `1000000001` |
| `sources[].lang` | Stessa lingua dell'estensione | `"it"` |
| `sources[].name` | Nome della fonte come appare nell'app | `"StreamingCommunity"` |
| `sources[].baseUrl` | URL base del sito web | `"https://streamingcommunity.example"` |

> **IMPORTANTE sul campo `version`:** Il formato è `"X.Y"` dove `X` è la **lib version** (deve essere tra 12 e 16 per essere accettata dall'app). Esempio: `"14.1"` → lib version = 14 ✓

**Per aggiungere più estensioni**, aggiungi altri oggetti all'array separati da virgola.

---

### 3. Cartella `icon/`

Metti qui l'icona dell'estensione come file PNG. Il nome del file deve essere:

```
<pkg>.png
```

Esempio: se il `pkg` è `eu.kanade.tachiyomi.animeextension.it.streamingcommunity`, il file dev'essere:

```
icon/eu.kanade.tachiyomi.animeextension.it.streamingcommunity.png
```

L'icona dovrebbe essere quadrata, idealmente 112x112 px.

---

### 4. Cartella `apk/`

Metti qui i file APK delle estensioni. Il nome del file deve corrispondere esattamente al campo `apk` in `index.min.json`.

> **Nota:** L'APK dell'estensione è un'app Android separata che implementa le interfacce di Aniyomi (`AnimeHttpSource` ecc.). Per ora puoi usare questa repo anche solo come struttura — l'APK vero e proprio lo svilupperai separatamente.

---

## Come pubblicare su GitHub

### Passo 1: Crea un repository GitHub

1. Vai su [github.com/new](https://github.com/new)
2. Nome del repo: ad esempio `series-extensions`
3. Mettilo **Public** (deve essere accessibile dall'app)
4. Clicca **Create repository**

### Passo 2: Carica i file

Puoi farlo in due modi:

**Opzione A — Tramite l'interfaccia web GitHub:**

1. Nel tuo nuovo repo, clicca **"Add file"** → **"Upload files"**
2. Trascina tutti i file dalla cartella `series-extension-repo-template/`:
   - `repo.json`
   - `index.min.json`
   - `icon/` (con i file .png)
   - `apk/` (con i file .apk)
3. Clicca **"Commit changes"**

**Opzione B — Tramite Git (se lo hai installato):**

```bash
cd series-extension-repo-template
git init
git add .
git commit -m "Initial repo setup"
git remote add origin https://github.com/TUO-USERNAME/series-extensions.git
git branch -M main
git push -u origin main
```

### Passo 3: Trova l'URL della repo

L'URL da inserire nell'app è nel formato **raw di GitHub**:

```
https://raw.githubusercontent.com/TUO-USERNAME/series-extensions/main/index.min.json
```

Sostituisci:
- `TUO-USERNAME` → il tuo username GitHub
- `series-extensions` → il nome del tuo repository
- `main` → il branch (di solito `main`)

### Passo 4: Aggiungi la repo nell'app

1. Apri ShiryuYomi
2. Vai su **Impostazioni** → **Sfoglia**
3. Tocca **"Repo delle estensioni serie"**
4. Tocca il pulsante **+** (in basso a destra)
5. Incolla l'URL:
   ```
   https://raw.githubusercontent.com/TUO-USERNAME/series-extensions/main/index.min.json
   ```
6. Conferma

L'app scaricherà `repo.json` e `index.min.json` dal tuo repo GitHub e mostrerà le estensioni disponibili nella tab **"Estensioni Serie"** sotto **Sfoglia**.

---

## Esempio completo con StreamingCommunity

Se volessi creare un'estensione per un sito chiamato "StreamingCommunity":

**`repo.json`:**
```json
{
  "meta": {
    "name": "ShiryuYomi Serie Extensions",
    "shortName": "SYS",
    "website": "https://github.com/tuouser/series-extensions",
    "signingKeyFingerprint": "NOFINGERPRINT01"
  }
}
```

**`index.min.json`:**
```json
[
  {
    "name": "Aniyomi: StreamingCommunity",
    "pkg": "eu.kanade.tachiyomi.animeextension.it.streamingcommunity",
    "apk": "tachiyomi-it-streamingcommunity-v14.1.apk",
    "lang": "it",
    "code": 1,
    "version": "14.1",
    "nsfw": 0,
    "sources": [
      {
        "id": 7894561230,
        "lang": "it",
        "name": "StreamingCommunity",
        "baseUrl": "https://streamingcommunity.example"
      }
    ]
  }
]
```

**File da aggiungere:**
- `icon/eu.kanade.tachiyomi.animeextension.it.streamingcommunity.png` (icona 112x112)
- `apk/tachiyomi-it-streamingcommunity-v14.1.apk` (l'APK dell'estensione)

**URL da inserire nell'app:**
```
https://raw.githubusercontent.com/tuouser/series-extensions/main/index.min.json
```

---

## Riassunto veloce

| Azione | Dettaglio |
|--------|----------|
| Cambia nome sito | Modifica `name`, `pkg`, `apk`, `sources[].name`, `sources[].baseUrl` in `index.min.json` |
| Cambia lingua | Modifica `lang` e `sources[].lang` |
| Aggiorna versione | Incrementa `code` e aggiorna `version` e il nome del file `apk` |
| Aggiungi estensione | Aggiungi un nuovo oggetto all'array in `index.min.json` |
| Pubblica | Push su GitHub → usa URL `raw.githubusercontent.com` nell'app |
