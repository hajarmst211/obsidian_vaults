### **Question 1**
**"Expliquez comment la prise en compte du 'Context information' (par exemple, 'language model') peut améliorer la performance de l'analyse des sentiments, en particulier dans le cas de textes contenant de l'ironie ou du sarcasme. Donnez un exemple concret."**

*   **Réponse :** L'analyse des sentiments classique (basée sur des lexiques) se contente de compter les mots positifs et négatifs. Or, l'ironie utilise des mots positifs pour exprimer un sentiment négatif. Un **modèle de langage** (comme BERT ou GPT) analyse la relation entre les mots (le contexte) plutôt que les mots isolés. Il comprend que la structure de la phrase ou la contradiction entre deux faits indique un changement de polarité.

---

### **Question 2**
**"Comparez et contrastez les approches 'sac de mots' (Bag of Words) et les approches basées sur des embeddings de mots (Word Embeddings, ex. Word2Vec, GloVe) pour la représentation de documents textuels. Discutez de leurs avantages et de leurs inconvénients respectifs en termes de performance, d'interprétabilité et de capacité à capturer les relations séman1tiques entre les mots."**

*   **Réponse :**
    *   **Sac de mots (BoW) :** Représente le texte par la fréquence des mots.
        *   *Avantage :* Très interprétable (on sait quel mot a influencé le modèle). Simple à mettre en œuvre.
        *   *Inconvénient :* Matrice très creuse (énormément de zéros), ignore l'ordre des mots et les synonymes (pour BoW, "auto" et "voiture" sont deux choses totalement différentes).
    *   **Embeddings (Word2Vec) :** Représente les mots par des vecteurs denses dans un espace continu.
        *   *Avantage :* Capture la sémantique (les mots proches en sens ont des vecteurs proches). Très performant pour le deep learning.
        *   *Inconvénient :* Moins interprétable ("Boîte noire"). Nécessite beaucoup de données pour être entraîné.Embeddings

---

### **Question 3**
**"Quelles sont les principales améliorations que les Transformers ont apportées par rapport aux modèles de langage précédents comme les LSTMs ?"**

- Le **LSTM** est un type de réseau de neurones récurrent. Sa particularité est qu'il traite les informations de manière **séquentielle** (un mot après l'autre).
- **Transformers:** C'est l'architecture qui a permis la création de modèles comme **BERT, GPT-4 ou Claude**. Contrairement au LSTM, le Transformer ne traite pas les mots l'un après l'autre, mais **tous en même temp**

*   **Réponse :**
    1.  **Parallélisation :** Contrairement aux LSTMs qui lisent le texte mot à mot (séquentiel), les Transformers traitent tous les mots d'une phrase en même temps, ce qui accélère l'entraînement.
    2.  **Mécanisme d'Attention (Self-Attention) :** Le modèle peut se "concentrer" sur n'importe quel mot de la phrase, peu importe sa distance. Les LSTMs avaient tendance à "oublier" le début d'une phrase très longue (problème du gradient qui disparaît).

---

### **Question 4**
**"Qu'est-ce que le modèle RAG (Retrieval-Augmented Generation) et comment fonctionne-t-il ?"**

*   **Réponse :** Le RAG est une technique qui combine un modèle de génération (comme GPT) avec un système de recherche documentaire.
*   **Fonctionnement :** 
    1.  **Retrieval :** Quand on pose une question, le système va d'abord chercher des documents pertinents dans une base de données externe (privée ou à jour).
    2.  **Augmentation :** On donne ces documents au modèle de langage.
    3.  **Génération :** Le modèle génère une réponse en se basant sur les informations fraîchement récupérées. Cela réduit les "hallucinations" (erreurs) du modèle.

---

### **Question 5**
**"Qu'est-ce que le fine-tuning d'un modèle de langage ? En quoi consiste-t-il ?"**

*   **Réponse :** Le fine-tuning consiste à prendre un modèle de langage déjà pré-entraîné sur une masse gigantesque de données et à l'entraîner légèrement sur un **jeu de données spécifique** à une tâche.
*   **En quoi il consiste :** On ajuste les poids du modèle pour qu'il devienne expert dans un domaine précis