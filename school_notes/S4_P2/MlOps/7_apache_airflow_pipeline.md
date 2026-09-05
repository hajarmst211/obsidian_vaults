
### **1. La Philosophie du "DAG Mature" (Bonnes Pratiques)**
Le cours insiste sur la conception de pipelines robustes basés sur quatre piliers :
*   **Lisible :** Un nouveau venu doit comprendre le flux en moins de 2 minutes.
*   **Idempotent :** La capacité de rejouer une tâche N fois sans corrompre les données (ex: supprimer les données existantes avant de réinsérer). *« Si vous n'osez pas faire Clear sur une tâche, c'est qu'elle n'est pas idempotente. »*
*   **Modulaire :** Une tâche = une action. La logique métier doit être extraite dans des modules Python testables.
*   **Observable :** Tout ce qui compte doit être mesuré (logs structurés, métriques).

**Checklist avant production :** Vérifiez l'idempotence, configurez les `retries`, branchez des alertes (`on_failure_callback`), utilisez des `Connections` sécurisées et assurez-vous que le DAG passe les tests d'intégrité en CI.

---

### **2. Observabilité, Monitoring & Debugging**
Pour ne pas « piloter à l'aveugle », le monitoring doit répondre à trois questions : *Que se passe-t-il ? Y a-t-il un problème ? Pourquoi a-t-on échoué ?*

#### **Outils de diagnostic**
*   **Les vues de l'UI :**
    *   **Grid View :** Vue par défaut pour repérer les patterns d'échecs.
    *   **Graph View :** Visualisation des dépendances et du point d'arrêt du pipeline.
    *   **Logs :** La source de vérité brute (toujours lire la fin du fichier en cherchant `ERROR` ou `Traceback`).
*   **Métriques :** Ne pas se contenter des logs. Exposer des métriques via **StatsD** vers **Prometheus/Grafana** permet d'agir avant que les utilisateurs ne détectent un incident (ex: `scheduler.heartbeat`, `ti.failures`).

#### **Workflow de Debug en 5 étapes**
1.  **Reproduire :** Trigger manuel depuis l'UI.
2.  **Localiser :** Identifier la cellule rouge dans la *Grid View*.
3.  **Lire :** Analyser le bas des logs pour le `Traceback`.
4.  **Isoler :** Tester en local avec la commande : `airflow tasks test <dag_id> <task_id> <logical_date>`.
5.  **Corriger & Rejouer :** Nettoyer et relancer.

---

### **3. Anti-patterns à bannir**
Les deux cours convergent vers une liste de comportements à éviter absolument :
*   **Code "sale" :** Ne jamais mettre de logique métier complexe directement dans le DAG, ne pas stocker de mots de passe en clair (utiliser `Connections` ou `Secret Backend`), et ne pas faire d'imports lourds (ex: `pandas`) au niveau du module (préférez l'import à l'intérieur des tâches).
*   **Debug "aveugle" :** Ne pas cliquer sur "Clear" sans avoir lu les logs, ne pas utiliser de `print` (utiliser le module `logging`), et ne pas ignorer les erreurs de type `upstream_failed` (toujours remonter à la première tâche fautive).
*   **Surcharge :** Éviter le "Monolithe" (une seule grosse tâche) au profit de la granularité fine, qui facilite le retry ciblé et le debug.

---

### **Résumé pour la vie sereine**
1.  **Logger d'abord, déboguer ensuite :** Un pipeline sans logs est aveugle.
2.  **Alerter de manière actionnable :** Une alerte doit dire qui fait quoi.
3.  **Mesurer avant d'optimiser :** Utilisez des tableaux de bord pour visualiser l'état du système.
4.  **Le meilleur DAG :** C'est celui qui n'a pas besoin de vous pendant vos vacances.