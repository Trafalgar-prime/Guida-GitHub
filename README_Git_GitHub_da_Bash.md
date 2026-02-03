# Git + GitHub da bash (WSL): creare una repo, riempirla, scaricare/estrarre dati — guida completa

Questa guida è pensata per lavorare **da bash in WSL** (come fai tu) e copre:
- come creare una repository su GitHub
- come collegarla a una cartella locale
- come aggiungere/aggiornare file (commit + push)
- come “estrarre i dati” (clone/pull/fetch, download di file e history)
- come funziona **.gitignore** (in modo preciso) e cosa metterci dentro

> Nota: in WSL stai usando Git/Linux. Va benissimo anche se i file del progetto stanno in `/mnt/c/...`.

---

## 0) Prerequisiti (prima di iniziare)

### 0.1 Verifica Git installato
```bash
git --version
```

Se non c’è:
```bash
sudo apt update
sudo apt install git
```

### 0.2 Imposta nome e email (una volta sola)
Questi verranno scritti nei commit.

```bash
git config --global user.name "Lorenzo"
git config --global user.email "tua_email@example.com"
```

Verifica:
```bash
git config --global --list
```

---

## 1) Autenticazione con GitHub (due modi)

Per spingere su GitHub (push) ti serve autenticarti.

### Metodo A (consigliato): SSH
**Vantaggi:** non ti chiede password ogni volta, stabile in WSL.

1) Controlla se hai già una chiave:
```bash
ls -la ~/.ssh
```

2) Se non hai `id_ed25519` (o simile), creala:
```bash
ssh-keygen -t ed25519 -C "tua_email@example.com"
```
- premi invio per il percorso di default
- passphrase: puoi metterla (più sicuro) o lasciarla vuota

3) Avvia ssh-agent e aggiungi la chiave:
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

4) Copia la chiave pubblica:
```bash
cat ~/.ssh/id_ed25519.pub
```

5) Vai su GitHub:
- Settings → SSH and GPG keys → New SSH key
- incolla il contenuto e salva

6) Test:
```bash
ssh -T git@github.com
```
Dovresti vedere un messaggio di successo.

---

### Metodo B: HTTPS con token (PAT)
GitHub non accetta più password su HTTPS: serve un **token**.

- su GitHub: Settings → Developer settings → Personal access tokens → Tokens (classic) o Fine-grained
- crea token con permessi repo
- quando fai `git push`, come password inserisci il token

---

### Metodo C: da HTTPS a SSH
GitHub non accetta più password su HTTPS: ma manda il push direttamente in porto.

```bash
git remote -v
```
Con questo comando leggo se sono in SSH o in HTTP, infatti posso ottenere due risultati:

```bash
git remote -v
```
origin  https://github.com/Traflgar-prime/pyhton_lezioni.git (fetch)
origin  https://github.com/Traflgar-prime/pyhton_lezioni.git (push)

```bash
git remote set-url origin git@github.com:Trafalgar-prime/python_lezioni.git
git remote -v
```
origin  git@github.com:Trafalgar-prime/python_lezioni.git (fetch)
origin  git@github.com:Trafalgar-prime/python_lezioni.git (push)

Nel primo caso inizia con HTTP mentre il secondo legge il codice SSH, per aggiornarlo ho usato il codice nel mezzo

---

## 2) Creare una repository su GitHub (due strade)

### Strada 1 (più comune): repo creata dal sito GitHub
1) GitHub → “New repository”
2) Dai nome, ad es. `lezione-16`
3) Scegli Public o Private
4) NON mettere file di default se vuoi fare tutto da bash (oppure metti README, cambia poco)

Poi GitHub ti dà un URL:
- SSH: `git@github.com:TUOUSER/lezione-16.git`
- HTTPS: `https://github.com/TUOUSER/lezione-16.git`

---

### Strada 2 (da terminale): GitHub CLI (`gh`)
Se vuoi farlo totalmente da bash, puoi usare `gh` (GitHub CLI).

Installa:
```bash
sudo apt update
sudo apt install gh
```

Login:
```bash
gh auth login
```

Crea repo dalla cartella corrente:
```bash
gh repo create nome-repo --public --source=. --remote=origin --push
```

> Se non vuoi installare `gh`, usa la Strada 1.

---

## 3) Inizializzare Git nella cartella del progetto (locale)

Vai nella cartella del progetto (esempio con spazi):
```bash
cd "/mnt/c/Users/utente/Lezioni CHAT GPT/Python/Lezione 16"
```

Inizializza git:
```bash
git init
```

Cosa succede:
- viene creata la cartella nascosta `.git/`
- lì dentro Git salva tutta la storia (commit, branches, ecc.)
- da quel momento la cartella è un “repository locale”

Verifica stato:
```bash
git status
```

---

## 4) `.gitignore` — come funziona DAVVERO (super importante)

### 4.1 Che cos’è
`.gitignore` è un file che contiene regole per dire a Git:

> “Questi file/cartelle **non** devono essere tracciati (tracked) e quindi **non** devono finire nei commit.”

### 4.2 Cosa fa e cosa NON fa (distinzione critica)

✅ `.gitignore` impedisce di aggiungere file **non ancora tracciati**.

❌ `.gitignore` NON rimuove automaticamente file già tracciati.

Esempio:
- hai già committato `ambiente_virtuale/`
- poi aggiungi `ambiente_virtuale/` nel `.gitignore`
- Git continuerà a tracciare quella cartella finché non fai:
```bash
git rm -r --cached ambiente_virtuale/
```

Poi commit.

### 4.3 Regole base di matching
Le regole in `.gitignore` sono pattern:
- `nomefile` ignora quel file ovunque
- `cartella/` ignora quella cartella
- `*.log` ignora tutti i file `.log`
- `!important.log` fa eccezione (non ignorare)
- `**/cache/` può ignorare cartelle annidate
- un pattern è relativo alla root del repo, a meno di non specificare diversamente

### 4.4 Cosa ci va dentro (per Python + WSL)
Esempio perfetto per i tuoi progetti:

```gitignore
# --- Ambienti virtuali ---
ambiente_virtuale/

# --- Cache Python ---
__pycache__/
*.pyc
*.pyo

# --- Jupyter / IPython ---
.ipynb_checkpoints/

# --- IDE / Editor ---
.vscode/
.idea/

# --- OS ---
.DS_Store
Thumbs.db

# --- Output / logs (se usati) ---
outputs/
*.log

# --- Dataset pesanti (se usati) ---
data/
!data/README.md
```

Nota sull’ultima parte:
- ignori `data/` (per non caricare file enormi)
- ma lasci un `data/README.md` per spiegare cosa ci dovrebbe essere

### 4.5 Creare `.gitignore` da bash
Nella root del progetto:

```bash
nano .gitignore
```

Incolla le regole, salva.

Oppure “one shot”:
```bash
cat > .gitignore << 'EOF'
ambiente_virtuale/
__pycache__/
*.pyc
.vscode/
.DS_Store
Thumbs.db
EOF
```

---

## 5) Aggiungere file al repo: add → commit

### 5.1 Vedere cosa Git vede
```bash
git status
```

### 5.2 Aggiungere file (staging)
Per aggiungere tutto ciò che non è ignorato:
```bash
git add .
```

### 5.3 Creare un commit
```bash
git commit -m "Prima versione: script e appunti"
```

Cosa succede:
- Git salva uno “snapshot” dei file tracciati
- ogni commit ha un ID (hash) unico

Vedi la storia:
```bash
git log --oneline
```

---

## 6) Collegare la repo locale a GitHub (remote)

Dopo aver creato la repo su GitHub (Strada 1), collega il remote.

### 6.1 Aggiungi remote `origin`
Esempio SSH:
```bash
git remote add origin git@github.com:TUOUSER/lezione-16.git
```

Controlla:
```bash
git remote -v
```

### 6.2 Imposta branch principale (main)
Molti sistemi usano `main`.

```bash
git branch -M main
```

---

## 7) Caricare tutto su GitHub: `push`

Primo push:
```bash
git push -u origin main
```

Spiegazione:
- `push` invia i commit locali al server GitHub
- `-u` imposta `origin/main` come upstream, così dopo basta `git push`

Da quel momento, aggiornare è:
```bash
git push
```

---

## 8) Aggiornare la repo: workflow quotidiano

Ogni volta che fai modifiche:

1) controlla:
```bash
git status
```

2) aggiungi:
```bash
git add .
```

3) commit:
```bash
git commit -m "Aggiorna lezione: aggiunti esempi e note"
```

4) push:
```bash
git push
```

---

## 9) “Estrarre i dati” da GitHub: clone / pull / fetch (spiegati bene)

### 9.1 `clone` (scaricare tutto per la prima volta)
Se la repo è su GitHub e vuoi averla in locale:

```bash
cd ~
git clone git@github.com:TUOUSER/lezione-16.git
```

Questo crea una cartella `lezione-16/` con:
- tutti i file
- tutta la storia (commit)
- i remote già configurati

### 9.2 `pull` (aggiornare la tua copia locale)
Dentro la cartella del repo:

```bash
git pull
```

`pull` = `fetch` + `merge` (di default):
- scarica i nuovi commit dal remote
- li integra nel tuo branch corrente

### 9.3 `fetch` (scaricare SENZA integrare)
```bash
git fetch
```

Questo aggiorna i riferimenti remoti (es. `origin/main`) ma non cambia i tuoi file.
È utile per “vedere cosa c’è di nuovo” senza modificarli.

Vedi differenze:
```bash
git log --oneline --decorate --graph --all
```

Oppure:
```bash
git diff main..origin/main
```

---

## 10) Scaricare/estrarre un singolo file o una versione specifica

### 10.1 “Dammi il file com’era in un commit”
Esempio:
```bash
git show HASH_COMMIT:percorso/del/file.py
```

### 10.2 Ripristinare un file all’ultima versione committata
```bash
git restore percorso/del/file.py
```

### 10.3 Ripristinare tutti i file (attenzione: perdi modifiche non committate)
```bash
git restore .
```

---

## 11) Branch (minimo indispensabile)

- `main` = branch principale
- un branch ti permette di lavorare senza “sporcare” main

Crea branch:
```bash
git checkout -b feature-nuovi-esempi
```

Commits e push su quel branch:
```bash
git push -u origin feature-nuovi-esempi
```

Poi su GitHub apri una Pull Request.

---

## 12) Troubleshooting essenziale

### 12.1 “Ho committato una cosa che doveva essere in .gitignore”
1) mettila in `.gitignore`
2) rimuovi dal tracking (non dal disco):
```bash
git rm -r --cached NOME_CARTELLA_O_FILE
```
3) commit + push:
```bash
git commit -m "Rimuove file tracciati e aggiorna .gitignore"
git push
```

### 12.2 “Permission denied (publickey)” con SSH
- non hai aggiunto la chiave su GitHub
- oppure non hai `ssh-agent` attivo

Rifai:
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
ssh -T git@github.com
```

### 12.3 “Git vede file strani su Windows”
Se lavori in `/mnt/c/...` a volte puoi avere differenze di line endings (CRLF/LF).
Consigliato:

```bash
git config --global core.autocrlf input
```

---

## 13) Esempio completo (end-to-end) in 12 comandi

```bash
# 1) entra nella cartella progetto
cd "/mnt/c/Users/utente/Lezioni CHAT GPT/Python/Lezione 16"

# 2) init git
git init

# 3) crea .gitignore
cat > .gitignore << 'EOF'
ambiente_virtuale/
__pycache__/
*.pyc
.vscode/
.DS_Store
Thumbs.db
EOF

# 4) add + commit
git add .
git commit -m "Prima versione"

# 5) collega remote (sostituisci TUOUSER e repo)
git remote add origin git@github.com:TUOUSER/lezione-16.git
git branch -M main

# 6) push
git push -u origin main
```

---

### Fine

Se vuoi, posso anche crearti:
- un `.gitignore` “perfetto” per progetti ML (con modelli, dataset e checkpoint)
- un template `README.md` per le tue lezioni (setup, run, dipendenze, troubleshooting)
