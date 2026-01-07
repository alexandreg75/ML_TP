TP 6 - Machine Learning


Question 1 :

Capture MLflow montrant la version Production au début du TP :

![Image 1](./images_TP6/Image1.png)

Screens terminal montrant docker compose up -d et docker compose ps :

![Image 2](./images_TP6/Image2.png)


![Image 3](./images_TP6/Image3.png)


Question 2 :

Screen terminal montrant pytest -q :

![Image 4](./images_TP6/Image4.png)

On extrait une fonction pure (should_promote) afin de pouvoir tester la logique de décision indépendamment de Prefect, MLflow ou de l’environnement Docker. Cela permet d’avoir des tests unitaires simples, rapides, déterministes et faciles à maintenir.

Question 3 :

Capture MLflow montrant le résultat avec une nouvelle version créée (stage None). Le modèle Production reste le premier 1 (toujours en Production, pas archivé) : 

![Image 5](./images_TP6/Image5.png)

Screen des logs du flow : 

![Image 6](./images_TP6/Image6.png)

Le flow a entraîné un modèle candidat sur as_of=2024-02-29 et l’a comparé au modèle Production sur le même split de validation.
Le candidat obtient AUC=0.6309, alors que le modèle en Production est à AUC=0.7716.
Comme le candidat n’est pas meilleur de delta=0.01, la décision est skipped (pas de promotion).

Le delta, lui, est utilisé pour éviter de promouvoir un modèle sur une amélioration trop faible qui peut venir du hasard (bruit, split, ...) : on impose un gain minimum avant de changer la Production.

Question 4 : 

Capture d'un extrait du rapport Evidently HTML :

![Image 7](./images_TP6/Image7.png)

Screenshot de logs montrant le message RETRAINING_TRIGGERED ... et le résultat promoted/skipped :

![Image 8](./images_TP6/Image8.png)

Question 5 :

Screenshot du curl montrant la réponse JSON :

![Image 9](./images_TP6/Image9.png)

L’API doit être redémarrée car elle charge le modèle MLflow (models:/streamflow_churn/Production) au démarrage : si une nouvelle version a été promue en Production, le redémarrage est nécessaire pour que le service recharge cette nouvelle version en mémoire.
