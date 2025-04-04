# git_automata
Handles GitHub Project Management automation.

## Descrizione
Creiamo un processo automatizzato per aggiungere le issue su GitHub usando le API. Ti guiderò passo dopo passo.  

---

### **🔹 Step 1: Creare un Personal Access Token (PAT)**
Per interagire con le API di GitHub, serve un token di accesso:  
1. Vai su **GitHub → Settings → Developer Settings → Personal access tokens**  
2. Clicca su **"Generate new token (classic)"**  
3. Seleziona i permessi:  
   - `repo` (per scrivere sulle issue)  
   - `issues` (per creare e gestire issue)  
4. Copia il token generato (lo useremo nel codice)  

---

### **🔹 Step 2: Script Python per Creare Issues**
Questo script legge il file JSON e crea le issue su GitHub automaticamente.  

#### **📌 Codice Python**
```python
import requests
import json

# Configurazione
GITHUB_TOKEN = "tuo_token_github"
REPO_OWNER = "tuo_username_o_organizzazione"
REPO_NAME = "trading-tools"

# Carica le issues dal file JSON
with open("github_issues.json", "r") as f:
    issues = json.load(f)

# URL API GitHub per creare issue
url = f"https://api.github.com/repos/{REPO_OWNER}/{REPO_NAME}/issues"

# Headers per l'autenticazione
headers = {
    "Authorization": f"token {GITHUB_TOKEN}",
    "Accept": "application/vnd.github.v3+json"
}

# Creazione delle issue
for issue in issues:
    response = requests.post(url, headers=headers, json=issue)
    if response.status_code == 201:
        print(f"Issue creata: {issue['title']}")
    else:
        print(f"Errore ({response.status_code}): {response.text}")
```

---

### **🔹 Step 3: Esegui lo Script**
1. Salva il file JSON generato con il nome `github_issues.json`  
2. Copia lo script Python in un file, es. `create_issues.py`  
3. Apri un terminale e lancia:  
   ```bash
   python create_issues.py
   ```

Dovresti vedere le issue apparire automaticamente nel repository GitHub! 🎉  

---

### **🔹 Possibili Miglioramenti**
- Creare una GitHub Action per automatizzare il processo.  
- Aggiungere categorie (es. milestone, assegnatari).  
- Validare le issue prima di inviarle.  
