
# def:
Data Analytics as a multidisciplinary field used to extract knowledge from structured and unstructured data.
### Types de techniques et approches:
- Predective: predir une valeur numeric ou symbolique.
- Clustering ou segmentation: identifier des groupes d'items ayant un comportement similaire.
- Association: Trouver des événement ayant une forte probabilité de se réaliser ensemble.

# Etapes:
1. Comprehension du probleme
2. Comprehension des donnees
3. Preparation des donnees
4. Modelisation: choisir un algorithme de machine learning
5. Evaluation
6. Deploiment

### Comprehension des donnees:
**Donnees:**
- Economique
- sociodemographiques
- residentielle
- concurentielle
- type d'habitat

**Donnees a ne pas utiliser:**
• Non fiables
	✓trop de valeurs aberrantes ou manquantes
• Disponibles sur une durée trop courte
	✓soumises aux variations saisonnières
• Redondantes
	✓dont le poids est artificiellement augmenté, ou dont la colinéarité rend instable les résultats de certaines méthodes
• Non pertinentes
	✓qu’il faut remplacer par de nouveaux indicateurs
• Très corrélées à l’objectif de l’étude mais seulement dans l’échantillon d’apprentissage
	✓qui entraînent un «sur-apprentissage» dans les prédictions
• Trop peu corrélées à l’objectif de l’étude
	✓qui créent du « bruit », des fluctuations aléatoires

### Possibilite de deploiment:
•Plusieurs possibilités ne s’excluant pas :
	✓utilisation d’un tableur sur un PC pour réaliser un publipostage (marketing direct)
	✓intégration dans les fichiers clients de production ou d’infocentre (pour des ciblages)
	✓intégration dans les fichiers clients de production et sur le poste de travail des commerciaux
• Spécifier les habilitations d’accès
• Spécifier la périodicité des mises à jour

### Traitement des donnees:
• **La régression linéaire** traite les variables continues
• L’analyse discriminante traite les variables explicatives continues et les variables « cible » nominales
• **La régression logistique** traite les variables explicatives continues, binaires ou nominales, et les variables « cible » nominales ou ordinales
• **Les réseaux de neurones** traitent de préférence les variables continues dans [0,1]
• Certains **arbres de décision** (CHAID) traitent directement les variables discrètes et catégorielles mais discrétisent les variables continues
• D’autres arbres (CART, C4.5, C5.0) peuvent aussi traiter directement les variables continues

---
This PDF is a comprehensive guide to the **CRISP-DM methodology**, which is the industry standard for managing any Data Science or Data Analytics project. 

Since you are in a **Deep Learning course**, this document is meant to show you that "building a model" (the Deep Learning part) is actually just one small step in a much larger professional process.

Here is a summary of the PDF and the key takeaways you should "catch."

---

### 1. The Summary: The 4 Main Pillars

#### A. The Definition of Data Analytics (The "Why")
*   It is a **multidisciplinary** field (combining math, computer science, and business).
*   **Goal:** To extract "knowledge" from structured and unstructured data to help in **decision-making**.
*   **Applications:** It's used everywhere—Banks (credit scores), Telecom (churn), Medicine (predicting recovery), and CRM (knowing the customer).

#### B. The CRISP-DM Methodology (The "How")
The core of the PDF explains the 6-step lifecycle of a project:
1.  **Business Understanding:** Setting the goals.
2.  **Data Understanding:** Checking what data you have and if it's reliable.
3.  **Data Preparation:** Cleaning and "cooking" the data (The longest phase).
4.  **Modeling:** Choosing and training the algorithm (This is where Deep Learning lives).
5.  **Evaluation:** Making sure the model actually works and isn't just "memorizing" data.
6.  **Deployment:** Putting the model to work in the real world and training users.

#### C. Data Quality & Pre-processing (The "Raw Material")
*   **Data is messy:** Real-world data is "noisy" (errors), "incomplete" (missing values), and "inconsistent."
*   **Pre-processing is mandatory:** You must perform **Cleaning** (fixing errors), **Integration** (merging files), **Transformation** (scaling/normalizing), and **Reduction** (making it smaller).
*   **The 60% Rule:** You will spend **40% to 60%** of your time just understanding and preparing data, and only about **20%** on the actual modeling/validation.

#### D. Roles in a Project (The "Who")
*   **Software Engineer:** Builds the apps and systems that *generate* the data.
*   **Data Engineer:** Builds the "pipes" and infrastructure (Hadoop/Spark) to *move and store* the data.
*   **Data Scientist:** Performs the *analysis and modeling* on top of that data.

---

### 2. What you are supposed to "catch" in general

If you want to excel in your course and future career, focus on these three realizations from the slides:

**1. Modeling is not the whole story**
In a Deep Learning course, you might think the "Neural Network" is the most important part. This PDF teaches you that a perfect model is **useless** if you didn't understand the Business Problem or if you don't know how to Deploy it.

**2. Garbage In, Garbage Out**
The sections on "Data Pre-processing" and "Data Quality" (Slides 55-59) are vital. If your data is "noisy" or "unreliable," even the most advanced Deep Learning architecture will produce "garbage" results. **You must master Data Preparation.**

**3. The Importance of Validation (Evaluation)**
Slide 39 highlights that models can give **false results** or "over-fit" (sur-apprentissage). You must "catch" the idea that you can't just trust a high accuracy score; you need to use tools like **Confusion Matrices** and **ROC Curves** to prove the model is actually smart.

**4. Different Tools for Different Jobs**
Notice Slide 27: **"Not all methods handle all types of data."** While Deep Learning is powerful, sometimes a simple Linear Regression or a Decision Tree is better, faster, and easier to explain to a client.

