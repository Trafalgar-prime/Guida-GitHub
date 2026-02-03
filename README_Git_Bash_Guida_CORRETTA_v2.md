# 📘 Guida Git Bash – Uso CORRETTO di GitHub (clone, push, pull)

Questa guida chiarisce **una cosa fondamentale**:

❌ `git clone` **NON serve** per caricare file dal PC a GitHub  
✅ `git clone` serve **SOLO** per copiare una repository **da GitHub al PC**

---

## 🔴 CONCETTO CHIAVE (da ricordare)
GitHub è una **copia sincronizzata** del tuo progetto.

- **GitHub → PC** → `git clone` / `git pull`
- **PC → GitHub** → `git add` → `git commit` → `git push`

---

## 1️⃣ Scaricare una repository da GitHub al PC (git clone)

Usa `git clone` **SOLO quando la repository ESISTE GIÀ su GitHub**.

```bash
cd /percorso/dove_vuoi_salvare_la_repo
git clone https://github.com/USERNAME/NOME_REPO.git
```

📌 Risultato:
- Git crea una cartella `NOME_REPO`
- Tutti i file vengono copiati dal GitHub al PC

---

## 2️⃣ Aggiornare una repository da GitHub al PC (git pull)

Usa `git pull` quando:
- un file è stato aggiunto o modificato su GitHub
- vuoi aggiornare la copia locale

```bash
cd NOME_REPO
git pull
```

---

## 3️⃣ Caricare file dal PC a GitHub (METODO CORRETTO)

⚠️ Questo è **l’UNICO metodo corretto**  
❌ **NON** si usa `git clone`

```bash
cd NOME_REPO
git add .
git commit -m "Descrizione delle modifiche"
git push
```

Questo comando carica:
- file nuovi
- file modificati
- file spostati

---

## 4️⃣ CASO REALE (flusso corretto)

1. La repository **esiste già su GitHub**
2. Cloni la repo sul PC:
```bash
git clone https://github.com/USERNAME/NOME_REPO.git
```
3. Copi o modifichi file **dentro la cartella del repo**
4. Carichi le modifiche con:
```bash
git add .
git commit -m "Aggiunte o modifiche"
git push
```

✅ Questo è il flusso **giusto e professionale**

---

## 🧠 Schema mentale definitivo

| Situazione | Comando |
|----------|--------|
| Prima copia GitHub → PC | `git clone` |
| Aggiornare GitHub → PC | `git pull` |
| PC → GitHub | `git add` → `git commit` → `git push` |
| ERRORE | usare `git clone` per caricare file |

---

## ⚠️ Errori comuni da evitare
- usare `git clone` più volte nella stessa cartella
- pensare che `git clone` carichi file su GitHub
- lavorare senza fare `git pull` prima

---

## ✅ Routine consigliata

Prima di lavorare:
```bash
git pull
```

Dopo aver lavorato:
```bash
git add .
git commit -m "Messaggio chiaro"
git push
```

---

📌 Questo file è pensato per essere usato come **README.md** in una repository GitHub ed è adatto a **principianti**.
