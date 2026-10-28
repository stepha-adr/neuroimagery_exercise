# Récapitulatif – Séance 1 (28 octobre 2026)

## Ce qu'on a fait

1. **On a installé Python** avec un environnement conda dédié, appelé `neuro`.
2. **On a installé VS Code et Git**, puis configuré VS Code pour les notebooks.
3. **On a créé ton dépôt Git** `neuroimagery_exercise`, où tu déposeras tes exercices.

---

## 1. L'environnement Python

Les fichiers `environment.yml` et `check_env.py` t'ont été envoyés avant la séance. Dans un terminal, depuis le dossier qui les contient :

```bash
conda env create -f environment.yml   # crée l'environnement "neuro" (une seule fois)
conda activate neuro                  # à faire à chaque nouveau terminal
python check_env.py                   # doit se terminer par "Tout est prêt !"
```

L'environnement contient Python 3.12, NumPy, SciPy, Matplotlib, nibabel, nilearn, Jupyter et nipraxis.

À retenir : si un `import` ne fonctionne pas, vérifie d'abord que l'environnement `neuro` est bien activé. Son nom apparaît entre parenthèses au début de la ligne du terminal.

---

## 2. VS Code et Git

Installation deja fait.

**Configuration de Git (si necessaire) :**

```bash
git config --global user.name "Prénom Nom"
git config --global user.email "ton-email-github"
git config --global init.defaultBranch main
```

**Dans VS Code :**

1. Installer deux extensions, **Python** et **Jupyter** (toutes deux de Microsoft). Clique sur l'icône Extensions dans la barre de gauche et cherche-les.
2. Ouvrir un fichier `.ipynb`, cliquer sur **Select Kernel** en haut à droite et choisir l'environnement **`neuro`**.

---

## 3. Git et GitHub

- **Git** est l'outil installé sur ton ordinateur. Il garde l'historique de ton dossier : chaque étape enregistrée est une « photo » à laquelle tu peux revenir.
- **GitHub** est le site web qui garde une copie de ce dossier en ligne. Ça sert de sauvegarde, et ça nous permet de voir ton travail et de le commenter.

Ton dépôt : **`neuroimagery_exercise`**.

### Les commandes à retenir

| Commande | Ce qu'elle fait |
|---|---|
| `git clone <adresse>` | Copie un dépôt GitHub sur ton ordinateur. Une seule fois, au début. |
| `git add <fichier>` | Choisit les fichiers modifiés à inclure dans la prochaine « photo ». |
| `git commit -m "message"` | Prend la photo : enregistre les fichiers ajoutés dans l'historique, avec un message qui décrit ce qui a changé. |
| `git push` | Envoie tes commits vers GitHub. |
| `git fetch` | Regarde s'il y a du nouveau sur GitHub, sans rien modifier dans ton dossier. |
| `git pull` | Récupère les nouveautés de GitHub et les intègre à ton dossier (fetch + mise à jour). |
| `git status` | Indique quels fichiers ont changé et ce qui est prêt à être commité. À utiliser sans modération. |

### Le cycle de travail habituel

```
git pull                  # 1. récupérer les éventuelles nouveautés
   … tu travailles …      # 2. modifier, tester
git status                # 3. voir ce qui a changé
git add devoir1.ipynb     # 4. choisir les fichiers
git commit -m "Ajoute l'histogramme de la question 3"   # 5. enregistrer
git push                  # 6. envoyer sur GitHub
```

**Bonnes pratiques :**

- Un message de commit dit **ce qui** a changé : « Ajoute l'histogramme de la question 3 », pas « update ».
- Fais un commit à chaque étape qui fonctionne, pas un seul à la fin.
- Ne mets **jamais** de données IRM dans le dépôt (`.nii`, `.nii.gz`, `.gii`), seulement du code et des notebooks.

---

## 4. Devoir pour la séance 2 (jeudi 12 novembre)

Le notebook `devoir1.ipynb` est à mettre dans ton dépôt `neuroimagery_exercise/homework`. Chaque notion y est expliquée avec un petit exemple avant les questions, donc tu n'as pas besoin d'avoir lu le manuel Nipraxis avant de commencer.

- **Partie 0 :** rappels sur les tableaux NumPy (forme, indexation, masques booléens, `reshape`).
- **Partie 1 :** une vraie coupe d'IRM anatomique comme tableau. Histogramme, seuil, masque.
- **Partie 2 :** premier pas vers notre projet. Tu construis une barre qui balaie l'écran et un champ récepteur (pRF) gaussien, puis tu prédis la réponse du pRF.

Si tu bloques plus de 20 minutes sur une question, passe à la suivante et note ta question pour la séance.

**Pour aller plus loin (facultatif) :** les chapitres du manuel [Nipraxis](https://textbook.nipraxis.org/intro.html) indiqués dans le notebook. Pour les ouvrir comme notebooks dans VS Code :

```bash
git clone https://github.com/nipraxis/textbook
cd textbook
jupytext --to ipynb *.Rmd
```

---

**Prochaine séance :** jeudi 12 novembre. Images 3D/4D, format NIfTI, séries temporelles et qualité du signal.
