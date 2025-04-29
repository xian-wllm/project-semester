# Project Semester

Stratégie avancée d'apprentissage fédéré avec points de sauvegarde et rollback

    Racine initiale :
    Le modèle initial (ou dernier point de sauvegarde fiable) sert de racine dans l’arbre d'exploration.

    Traitement d’un batch :

        Tu envoies les mini-batchs aux workers disponibles avec redondance (vote majoritaire).

        Si un mini-batch atteint un consensus, on met à jour le modèle.

        Sinon, il est mis de côté ou replanifié plus tard.

    Évaluation du modèle (loss/accuracy) :

        Si amélioration : on continue.

        Si stagnation ou dégradation : le batch est marqué comme douteux.

    Traitement du batch suivant :

        Même logique : traitement avec redondance, vote, mise à jour conditionnelle.

        Après le second batch :

            Si 2 hausses consécutives : ✅ nouveau safe point créé (modèle fiable).

            Si baisse continue ou stagnation persistante : ❌ rollback au dernier safe point.

    Gestion des batchs douteux :

        Après un rollback, on reprend les batchs douteux dans le même ordre, mais avec de nouveaux workers (disponibilité différente).

        Si les performances s’améliorent, on poursuit.

        Sinon, rollback à nouveau.

    Cycle continu :

        On continue à avancer dans l’arbre en conservant uniquement les branches qui montrent des progrès validés.

        Le modèle évolue uniquement lorsqu’il est “certifié” bon par 2 hausses consécutives.