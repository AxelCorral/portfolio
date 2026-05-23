# Portfolio Axel Corral — Déploiement Vercel

Structure du projet :

```
portfolio-vercel/
├── api/
│   └── agent.js        ← Proxy sécurisé vers Groq (clé cachée côté serveur)
├── public/
│   └── index.html      ← Portfolio complet (HTML self-contained)
├── vercel.json         ← Config routing Vercel
├── package.json
└── .gitignore
```

---

## Déploiement en 5 étapes

### 1. Obtenir une clé Groq (gratuit, sans CB)

1. Aller sur https://console.groq.com
2. Créer un compte
3. Settings → API Keys → Create API Key
4. Copier la clé `gsk_xxxxxxxxxxxx`

---

### 2. Pousser sur GitHub

```bash
cd portfolio-vercel
git init
git add .
git commit -m "Initial portfolio"
git remote add origin https://github.com/TON_USERNAME/portfolio.git
git push -u origin main
```

---

### 3. Importer sur Vercel

1. Aller sur https://vercel.com/new
2. Importer le dépôt GitHub créé à l'étape 2
3. Laisser les paramètres par défaut (Framework : Other)
4. Cliquer **Deploy** — ne pas encore ajouter la clé ici

---

### 4. Ajouter la variable d'environnement (clé Groq)

Dans le dashboard Vercel du projet :

1. Settings → Environment Variables
2. Ajouter :
   - **Name** : `GROQ_API_KEY`
   - **Value** : `gsk_xxxxxxxxxxxx` (ta clé Groq)
   - **Environments** : Production ✓, Preview ✓, Development ✓
3. Cliquer **Save**

---

### 5. Redéployer

Dans Vercel → Deployments → cliquer les `...` du dernier déploiement → **Redeploy**

✅ Le portfolio est en ligne, l'agent fonctionne sans que les visiteurs aient à entrer une clé.

---

## Domaine personnalisé (optionnel)

Vercel → Settings → Domains → Add Domain → entrer `axelcorral.fr` (ou autre)
Suivre les instructions DNS.

---

## Tester en local

```bash
npm install
npx vercel dev
```

L'agent sera accessible sur `http://localhost:3000`.

Pour le dev local, créer un fichier `.env` à la racine :

```
GROQ_API_KEY=gsk_xxxxxxxxxxxx
```

Ce fichier est dans `.gitignore` — il ne sera jamais poussé sur GitHub.

---

## Limites Groq (plan gratuit)

| Modèle | Requêtes/minute | Tokens/minute | Tokens/jour |
|--------|----------------|--------------|-------------|
| llama-3.3-70b-versatile | 30 | 6 000 | 1 000 000 |

Largement suffisant pour un portfolio — le rate limit ne se déclenchera que si plusieurs recruteurs utilisent l'agent exactement au même moment.
