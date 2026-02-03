# 📘 Guida Git Bash – Spostare e Aggiornare Repository

Questa guida spiega **in modo semplice e pratico** come usare i comandi **da terminale (bash)** per:

1. Scaricare una repository **da GitHub al PC**
2. Caricare una repository **dal PC a GitHub**
3. **Aggiornare** una repository (pull / push)

Guida adatta a **Windows (WSL / Git Bash)**, Linux e macOS.

---

## 1️⃣ Scaricare una repository da GitHub al PC

Questa operazione si fa **una sola volta**.

```bash
cd /percorso/dove/vuoi/la/repo
git clone https://github.com/USERNAME/NOME_REPO.git
```

Esempio:
```bash
cd /mnt/c/Users/utente/Università/AIRO/Neuroengineering/Repo_GitHub
git clone https://github.com/Traflgar-prime/pyhton_lezioni.git
```

---

## 2️⃣ Caricare una repository dal PC a GitHub

Serve quando hai una cartella locale e vuoi pubblicarla su GitHub.

```bash
cd /percorso/della/cartella
git init
git branch -M main
git add .
git commit -m "Primo commit"
git remote add origin https://github.com/USERNAME/NOME_REPO.git
git push -u origin main
```

---

## 3️⃣ Aggiornare una repository

### GitHub → PC
```bash
git pull
```

### PC → GitHub
```bash
git add .
git commit -m "Messaggio di aggiornamento"
git push
```

---

## 🧠 Schema mentale finale

| Operazione | Comando |
|-----------|--------|
| Prima copia GitHub → PC | git clone |
| Aggiornare GitHub → PC | git pull |
| Aggiornare PC → GitHub | git add / commit / push |

---

## ⚠️ Errori comuni

- Non fare `git pull` prima di lavorare
- Dimenticare `git add`
- Usare `git clone` più volte nella stessa cartella

---

## ✅ Routine consigliata

Prima di lavorare:
```bash
git pull
```

Dopo aver lavorato:
```bash
git add .
git commit -m "Descrizione chiara"
git push
```

---

📌 Questo file può essere usato direttamente come README.md su GitHub.
