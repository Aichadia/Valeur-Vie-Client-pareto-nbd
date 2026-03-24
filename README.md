# 📈 Modélisation de la Valeur Vie Client — Pareto/NBD sur CDNOW

> Prédire combien de fois un client va acheter dans les 9 prochains mois — sur 23 570 clients réels, en modélisant le churn latent avec le modèle *Buy Till You Die* (Pareto/NBD).

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=flat-square&logo=numpy&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat-square&logo=pandas&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557c?style=flat-square)
![Lifetimes](https://img.shields.io/badge/Lifetimes-BTYD-orange?style=flat-square)
![Seaborn](https://img.shields.io/badge/Seaborn-4C72B0?style=flat-square)

---

## 📌 Contexte & problème business

Dans une relation client **non contractuelle** (e-commerce, streaming, retail), on ne sait jamais si un client qui n'achète plus est **churné** ou simplement **en pause**. Estimer la Valeur Vie Client (CLV) sans modéliser ce churn latent conduit à des projections trop optimistes.

Ce projet implémente le modèle **Pareto/NBD** (Fader & Hardie, 2001) sur les données réelles **CDNOW** — 23 570 clients, 69 659 transactions sur 18 mois — avec une validation sur une période de holdout de 39 semaines.

Projet réalisé dans le cadre du Mastère *Data Science pour la Connaissance Client* — **ENSAI**.

---

## 📊 Résultats

| | |
|---|---|
| **Clients analysés** | 23 570 |
| **Transactions** | 69 659 sur 78 semaines |
| **Sur-dispersion** | Var = 3.49 >> Moy = 0.95 → Poisson inadapté, Binomiale Négative justifiée |
| **Tendance prédite** | ✅ Le modèle capture la décroissance du churn agrégé sur 39 semaines |
| **Biais observé** | ⚠️ Sous-estimation de ~30-40% en début de holdout |

> 💡 **Ce que le modèle fait bien** : il prédit correctement que les achats hebdomadaires agrégés décroissent dans le temps, reflet du churn progressif des clients inactifs.
>
> ⚠️ **Sa limite principale** : il ne capture pas la saisonnalité ni les réactivations tardives — des clients considérés comme churné reviennent parfois bien après, comme l'illustrent les données.

---

## 🔍 Ce que ce projet démontre

- **Modélisation probabiliste rigoureuse** : du mélange Gamma-Poisson à la loi Binomiale Négative, jusqu'au modèle Pareto/NBD complet — chaque étape est dérivée et justifiée
- **Validation temporelle réelle** : split calibration / holdout sur données historiques (pas de data leakage)
- **Lecture critique des résultats** : identification et explication du biais de sous-estimation (saisonnalité, churn non définitif)
- **Double estimation** : comparaison Méthode des Moments vs Maximum de Vraisemblance sur les paramètres sous-jacents

---

## 🛠️ Stack technique

| Catégorie | Outils |
|-----------|--------|
| Langage | Python 3 |
| Manipulation des données | Pandas, NumPy |
| Modélisation BTYD | `lifetimes` (ParetoNBDFitter) |
| Stats & distributions | SciPy (`nbinom`) |
| Visualisation | Matplotlib, Seaborn |
| Environnement | Google Colab |

---

## 📁 Structure du repo

```
📦Valeur-Vie-Client-pareto-nbd
 ┣ 📂 data/
 ┃ ┗ CDNOW_master.txt         # Historique transactionnel brut
 ┣ 📂 notebooks/
 ┃ ┗ VVC_projet_stat.ipynb    # Notebook principal (théorie + application)
 ┣ 📂 outputs/
 ┃ ┣ pred_vs_reel.png         # Prédictions vs réalité sur 39 semaines
 ┃ ┣ distrib_frequency.png    # Distribution de la fréquence d'achat
 ┃ ┗ ajustement_NB.png        # Ajustement Binomiale Négative (MoM)
 ┗ 📄 README.md
```

---

## 🚀 Comment reproduire

```bash
# 1. Cloner le repo
git clone https://github.com/Aichadia/Valeur Vie Client-pareto-nbd.git
cd Valeur Vie Client-pareto-nbd

# 2. Installer les dépendances
pip install lifetimes scipy pandas matplotlib seaborn

# 3. Lancer le notebook
jupyter notebook notebooks/VVC_projet_stat.ipynb
```

> Les données CDNOW sont disponibles publiquement sur [brucehardie.com](http://brucehardie.com/datasets/).

---

## 📈 Démarche

1. **EDA & sur-dispersion** — mise en évidence que Var >> Moyenne sur la fréquence d'achat → modèle de Poisson simple inadapté
2. **Cadre théorique** — dérivation rigoureuse : mélange Gamma-Poisson → Binomiale Négative → Pareto/NBD (estimateurs MM et MV, lois limites, VUMSB)
3. **Construction des features RFM** — `(frequency, recency, T)` par client sur la période de calibration (39 semaines)
4. **Estimation Pareto/NBD** — fit par maximum de vraisemblance, interprétation des paramètres (hétérogénéité d'achat et d'attrition)
5. **Prédiction & validation** — prédiction sur 39 semaines de holdout, analyse du biais et des limites du modèle

---

## 👩‍💻 Auteurs

Aicha DIAKITE - Seynabou NIANG - Louise RIGAL

**Aicha DIAKITE**  
🎓 Mastère Data Science pour la Connaissance Client — ENSAI  
🔗 [LinkedIn](https://linkedin.com/in/aicha-diakite) · [GitHub](https://github.com/Aichadia)
