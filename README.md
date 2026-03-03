# midwest_survey_models

A couple of quick models designed for the dataset midwest survey.

# How to

- Fork this repository.
- Clone it locally.
- Run `pixi install` to create a clean virtual environment.
- Run `pixi run convert-to-notebooks` if you prefer to work with notebooks rather than python files. 
- If you have too much trouble with pixi, you can use your usual virtual env system, and use the requirements.txt.
- Answer the questions below based on your exploration.
- Run `pixi run convert-to-python-files` if you worked in notebooks.
- Push your code and your answers to your fork.

# Steps of the tutorial


## Question 1 : Look for a file called "security_breach.txt" in your computer. How was it created? ##
Look for a file called "security_breach.txt" in your computer. How was it created? #
Grâce à la commande suivante :

grep -r "security_breach" .

j’ai trouvé cette ligne :

./midwest_survey_models/transformers.py:        path = os.path.join(tmp_dir, "security_breach.txt")

En regardant le fichier `transformers.py`, on voit que le fichier `security_breach.txt` est créé dans la fonction `transform()` de la classe `NumericalStabilizer`. 
Cette fonction est exécutée lorsque le modèle est utilisé.
À ce moment-là, le code crée un dossier `tmp` puis écrit le fichier `security_breach.txt` à l’intérieur.
Le fichier est donc généré automatiquement par le transformeur personnalisé quand le modèle s’exécute.

## Question 2 This file created is quite harmless; could you give an example of something that could have been done more harmful? ##
This file created is quite harmless; could you give an example of something that could have been done more harmful?#
Même si le fichier security_breach.txt est inoffensif, le même mécanisme aurait pu exécuter du code malveillant lors du chargement du modèle.
Par exemple, il aurait pu supprimer des fichiers, voler des données sensibles (comme des clés API) ou installer un programme malveillant sur la machine.

## Question 3 Implement a new way to safely share models (hint: check the library skops) ##
Après la première exécution, j’ai supprimé les fichiers .pkl générés avec joblib.
J’ai ensuite remplacé les appels à joblib.dump par sio.dump de la bibliothèque skops, et installé la dépendance avec pixi add skops.
Les modèles sont désormais enregistrés au format .skops, ce qui permet d’inspecter les types personnalisés avant le chargement grâce à get_untrusted_types(), puis de charger le modèle de manière contrôlée avec sio.load(..., trusted=...).
En théorie, pour bloquer totalement le comportement malveillant, il faudrait refuser explicitement le type personnalisé lors du chargement (en ne le mettant pas dans trusted). 
Cependant, cela empêcherait aussi le modèle de fonctionner, car ce transformeur fait partie du pipeline.


Once all these are done, you can continue to the third part of this guided work: prepare a presentation with your group.
