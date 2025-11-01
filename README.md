# 🧮 Calculateur de Tarif (Formulaire interactif)

Ce projet est un **formulaire web dynamique** permettant de calculer automatiquement un tarif en fonction de plusieurs paramètres (kilométrage, HDJ, tarif de nuit, etc.).

Il est entièrement codé en **HTML + JavaScript**, sans dépendances externes.

---

## 🚀 Démo en ligne

🔗 [Voir le formulaire en ligne](https://tonpseudo.github.io/calcul-tarif/)  
*(le lien fonctionnera une fois GitHub Pages activé dans les paramètres du dépôt)*

---

## ⚙️ Fonctionnement

L’utilisateur renseigne :

- **Kilomètres** parcourus  
- **Tarif kilométrique** (choix parmi 1.20, 1.22 ou 1.07 €/km)  
- Coche éventuellement :
  - 🏥 **HDJ / Chimio**
  - 🌙 **Tarif de nuit**
  - 🏙️ **Métropole**

### 🔢 Calcul effectué

1. Si le kilométrage saisi est inférieur à **4 km**, une erreur s’affiche.  
2. Le calcul de base :
