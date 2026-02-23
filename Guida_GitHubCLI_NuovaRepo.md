# Creare una nuova repository GitHub da terminale (Git + GitHub CLI `gh`) + spiegazione completa di `gh auth login`

> Obiettivo: **creare una nuova repo su GitHub** e **caricare (pushare) un progetto dal terminale**, con una guida **passo-passo** e una spiegazione chiara di **tutte le domande** che compaiono dopo `gh auth login` (incluso il caso WSL dove non si apre il browser).

---

## Prerequisiti (1 volta sola)

### A) Hai Git installato?
```bash
git --version
```

### B) Hai GitHub CLI (`gh`) installato?
```bash
gh --version
```

### C) Sei su WSL (Ubuntu su Windows)?
Se vedi percorsi come `/mnt/c/...`, sì: sei in WSL.  
In WSL spesso **non c’è un browser** installato, quindi quando `gh` prova ad aprirlo può fallire (vedi sezione dedicata più sotto).

---

## Parte 1 — Login a GitHub con `gh auth login` (spiegazione completa di tutte le opzioni)

Esegui:
```bash
gh auth login
```

Di solito comparirà un flusso simile a questo:

### 1) `What account do you want to log into? (GitHub.com / GitHub Enterprise)`
- **GitHub.com** → il GitHub normale (account personale/standard).
- **GitHub Enterprise** → GitHub aziendale con dominio dedicato.

✅ Nella maggior parte dei casi scegli **GitHub.com**.

---

### 2) `What is your preferred protocol for Git operations on this host? (SSH / HTTPS)`
Questa è IMPORTANTISSIMA: decide **come farai clone/push/pull**.

#### Opzione SSH (consigliata se vuoi “zero password”)
- Usi URL del tipo: `git@github.com:TUOUSER/REPO.git`
- Ti autentichi tramite **chiave SSH** (`~/.ssh/id_ed25519` ecc.)
- Dopo averla configurata, push/pull diventano fluidi e veloci.

#### Opzione HTTPS
- Usi URL del tipo: `https://github.com/TUOUSER/REPO.git`
- GitHub **non accetta più password**: serve un **Personal Access Token (PAT)** come “password”.

✅ Se hai scelto SSH, stai facendo la strada più comoda sul lungo periodo.

---

### 3) `Upload your SSH public key to your GitHub account? /home/xxx/.ssh/id_ed25519.pub`
Se hai scelto SSH, `gh` cerca una chiave pubblica (`.pub`) da caricare su GitHub.

- Se esiste già una chiave (es. `~/.ssh/id_ed25519.pub`), te la propone.
- Se accetti, la carica nel tuo account GitHub in **Settings → SSH and GPG keys**.

✅ Risposta consigliata: **Yes** (così GitHub riconosce il tuo PC/WSL).

> Nota: la chiave **pubblica** (file `.pub`) è quella che si carica su GitHub.  
> La chiave **privata** resta sul tuo PC/WSL e non va mai condivisa.

---

### 4) `Title for your SSH key`
È solo un **nome/etichetta** per riconoscere quella chiave dentro GitHub.

Esempi utili:
- `Lorenzo-fisso`
- `WSL-Ubuntu`
- `Laptop-Lorenzo`

✅ Consiglio: usa un nome che ti faccia capire **da quale dispositivo** arriva.

---

### 5) `How would you like to authenticate GitHub CLI? (Login with a web browser / Paste an authentication token)`
Qui stai scegliendo *come* autorizzare la CLI `gh` ad operare sul tuo account.

#### A) `Login with a web browser` (più semplice)
`gh` ti mostra:
- un **codice monouso** (es: `B279-3DBF`)
- un link: `https://github.com/login/device`

Tu apri quel link nel browser, inserisci il codice e autorizzi.

**Caso WSL (errore xdg-open / browser non trovato)**  
In WSL spesso esce:
- `xdg-open: no method available for opening 'https://github.com/login/device'`

✅ Soluzione: apri **manualmente** nel browser di Windows:
- vai a `https://github.com/login/device`
- incolla il codice
- autorizza GitHub CLI

Poi torna al terminale: il login si completa da solo.

#### B) `Paste an authentication token` (alternativa)
Incolli un token (PAT) creato su GitHub. È meno comodo ma utile in ambienti senza browser.

---

### Verifica finale login
```bash
gh auth status
```
Se sei loggato vedrai qualcosa tipo: “Logged in to github.com as …”.

---

## Parte 2 — Creare una nuova repo da terminale e caricare il progetto (metodo consigliato con `gh`)

### Step 0) Vai nella cartella del progetto
Esempio:
```bash
cd "/mnt/c/Users/utente/GitHub/Python/mio_progetto"
```

Controlla i file:
```bash
ls -la
```

---

### Step 1) Inizializza Git (se non è già una repo)
```bash
git init
```

---

### Step 2) Crea un `.gitignore` (fortemente consigliato)
Esempi rapidi:

**Python**
```bash
cat > .gitignore << 'EOF'
__pycache__/
*.pyc
.venv/
venv/
.env
.DS_Store
Thumbs.db
EOF
```

**Java / IntelliJ**
```bash
cat > .gitignore << 'EOF'
*.class
*.jar
bin/
out/
.idea/
*.iml
.DS_Store
Thumbs.db
EOF
```

---

### Step 3) Primo commit
```bash
git add .
git commit -m "Initial commit"
```

Se ti chiede nome/email:
```bash
git config --global user.name "Il Tuo Nome"
git config --global user.email "tuaemail@example.com"
```

Poi rifai il commit.

---

### Step 4) Crea repo su GitHub e fai push (1 comando)
```bash
gh repo create NOME_REPO --public --source=. --remote=origin --push
```

Se la vuoi privata:
```bash
gh repo create NOME_REPO --private --source=. --remote=origin --push
```

**Cosa fa questo comando:**
- crea la repo su GitHub
- collega il remote `origin`
- fa push del branch corrente

---

## Parte 3 — Creare una repo su GitHub “a mano” (senza `gh`) e collegarla

Se vuoi creare repo dal sito GitHub (New repository) e poi collegarla:

1) Crea repo vuota (meglio NON spuntare README/.gitignore/license se li hai già)
2) Copia l’URL della repo

Poi nel progetto:
```bash
git init
git add .
git commit -m "Initial commit"

git branch -M main
git remote add origin git@github.com:TUOUSER/NOME_REPO.git
git push -u origin main
```

Se usi HTTPS:
```bash
git remote add origin https://github.com/TUOUSER/NOME_REPO.git
git push -u origin main
```

---

## Parte 4 — Branch: `main` vs `master` (perché ti appare `master`)

Oggi GitHub usa spesso `main` come branch di default, ma potresti avere:
- repo vecchie con `master`
- configurazioni locali che creano `master`

### Convertire a `main` (se vuoi allinearti allo standard moderno)
```bash
git branch -M main
git push -u origin main
```

Se su GitHub la repo è già nata con `master`, puoi anche lasciarla così: non è “sbagliato”, è solo un nome.

---

## Parte 5 — Workflow quotidiano (quello che userai sempre)

Ogni volta che fai modifiche:
```bash
git status
git add .
git commit -m "Messaggio chiaro"
git push
```

Per vedere lo storico:
```bash
git log --oneline --decorate --graph --all
```

---

## Parte 6 — Troubleshooting (errori comuni e come risolverli)

### 1) `nothing to commit, working tree clean`
Significa: **non hai modifiche nuove** da committare.  
Verifica:
```bash
git status
```

Se vuoi testare:
```bash
echo "print('ciao')" > test.py
git add .
git commit -m "Aggiungo test.py"
git push
```

---

### 2) `Everything up-to-date`
Il tuo repository locale è già sincronizzato con GitHub: **non c’è nulla di nuovo da pushare**.

---

### 3) `xdg-open: ... not found` (WSL non apre il browser)
È normale in WSL. Soluzione: apri manualmente nel browser di Windows:
- `https://github.com/login/device`
- inserisci il codice

---

### 4) Problemi SSH (push chiede password o fallisce)
Verifica la connessione SSH:
```bash
ssh -T git@github.com
```

Se vedi un messaggio di successo (“Hi USER! …”), SSH è ok.

Se fallisce, controlla:
- che la chiave sia in `~/.ssh/`
- che sia stata caricata su GitHub (Settings → SSH and GPG keys)

---

## Parte 7 — Checklist finale (per sapere se hai fatto tutto bene)

Dentro la cartella progetto:
```bash
git status
git remote -v
gh auth status
```

Devi vedere:
- working tree pulito o modifiche sensate
- `origin` puntare a GitHub
- `gh` loggato correttamente

---

## Comandi “riassunto super rapido” (da copiare al volo)

### Nuova repo + push con `gh`
```bash
cd /percorso/progetto
git init
git add .
git commit -m "Initial commit"
gh repo create NOME_REPO --public --source=. --remote=origin --push
```

### Aggiornare la repo dopo modifiche
```bash
git add .
git commit -m "Update"
git push
```

---

## Extra: controllare quali repo hai dal terminale
```bash
gh repo list --limit 50
```

---

Fine.
