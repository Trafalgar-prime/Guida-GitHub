# 📘 Guida Git (Bash/WSL) – Caricare e Scaricare file tra PC e GitHub

Questa guida ti fa capire **cosa succede davvero** quando usi Git.

✅ **PC → GitHub** = carichi online (push)  
✅ **GitHub → PC** = scarichi sul PC (pull / clone)

---

## 🔥 Regola numero 1 (da ricordare)
- **`git pull`** scarica da **GitHub → PC**
- **`git push`** carica da **PC → GitHub**

---

# 1️⃣ Scaricare una REPO da GitHub al PC (prima volta) → `git clone`

Usa `git clone` **solo la prima volta**, quando sul PC non hai ancora la repo.

```bash
cd /percorso/dove_vuoi_salvare_la_repo
git clone https://github.com/USERNAME/NOME_REPO.git
```

📌 Esempio:
```bash
cd "/mnt/c/Users/utente/Università/AIRO/Neuroengineering/Repo_GitHub"
git clone https://github.com/USERNAME/NOME_REPO.git
```

✅ Risultato: viene creata una cartella `NOME_REPO` con tutti i file.

---

# 2️⃣ Scaricare aggiornamenti (GitHub → PC) → `git pull`

Se la repo esiste già sul PC e vuoi prendere i file nuovi/aggiornati da GitHub:

```bash
cd NOME_REPO
git pull
```

### Cosa significa l'output
- `Already up to date.` → sul GitHub non c'è niente di nuovo, quindi non cambia nulla sul PC
- se ci sono cambi → Git scarica e aggiorna i file sul PC

---

# 3️⃣ Caricare aggiornamenti (PC → GitHub) → `add` + `commit` + `push`

Quando aggiungi/modifichi/sposti file sul PC e vuoi pubblicarli su GitHub:

```bash
cd NOME_REPO
git add .
git commit -m "Descrizione chiara delle modifiche"
git push
```

✅ Questo carica su GitHub:
- file nuovi
- file modificati
- file spostati
- file cancellati (se li hai rimossi)

---

# 4️⃣ Esempi pratici (molto utili)

## ✅ Esempio A: hai creato un file nuovo sul PC
```bash
# (hai creato file_nuovo.md nella repo)
git add .
git commit -m "Aggiunto file_nuovo.md"
git push
```

## ✅ Esempio B: hai creato un file nuovo su GitHub (dal sito)
Sul PC fai solo:
```bash
git pull
```

---

# 5️⃣ Controlli rapidi (quando sei confuso)

## Dove sono?
```bash
pwd
```

## Cosa è cambiato?
```bash
git status
```

## In che branch sono?
```bash
git branch
```

---

# 🧠 Schema mentale finale

| Obiettivo | Direzione | Comando |
|---|---|---|
| Prima copia della repo | GitHub → PC | `git clone` |
| Prendere aggiornamenti | GitHub → PC | `git pull` |
| Pubblicare modifiche | PC → GitHub | `git add` → `git commit` → `git push` |

---

## ✅ Routine consigliata (sempre)

### Prima di lavorare
```bash
git pull
```

### Dopo aver lavorato
```bash
git add .
git commit -m "Messaggio chiaro"
git push
```

---

📌 Questo file è pronto da usare come **README.md** su GitHub.
