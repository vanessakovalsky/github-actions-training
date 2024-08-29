# Exercice 3 - Analyse de code et affichage de rapport


## Objectifs

Cet exercice a pour objectifs :
* D'ajouter des vérifications des normes de codage qui ajoute un commentaire à une PR
* D'afficher un rapport sur la couvertures des tests

## Analyse de qualité de code avec wemake-python-style guidede

* Nous allons mettre en place un flux de travail GitHub Actions qui sera déclenché chaque fois qu'une Pull Request est créée et ajouterons des commentaires à une revue où il trouve des problèmes potentiels.
* Pour commencer, dans le dossier racine de votre projet, créez un dossier .github/workflows
* Puis créer un nouveau fichier avec un nom workflow-pr.yaml.

```
name: Python Pull Request Workflow
on: [pull_request]
jobs:

  qa:
    name: Quality check
    runs-on: ubuntu-18.04
    steps:
      - uses: actions/checkout@v1
      - name: Set up Python
        uses: actions/setup-python@master
        with:
          python-version: 3.8
      - name: Wemake Python Stylguide
        uses: wemake-services/wemake-python-styleguide@0.13.4
        continue-on-error: true
        with:
          reporter: 'github-pr-review'
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

* C'est un flux de travail très simple avec l'intégralité namePython Pull Request Workflow. C'est déclenché onchaque pull_request, donc chaque fois que nous créons de nouveaux ou mettons à jour un autre existant suivant jobssera géré.
* Au-dessus du flux de travail, il n'y en a qu'un seul job un travail qui comporte 4 étapes:
  * actions/checkout@v1est nécessaire pour permettre au flux de travail GitHub Actions de savoir qu'il peut utiliser le code situé dans un référentiel,
  * Set up Python qui utilisent actions/setup-python@master pourconfigurer une version Python, dans notre cas, c'est python-version: 3.8.
  * Wemake Python Styleguide c'est celui qui nous intéresse le plus. Il utilise l'action wemake-services/wemake-python-styleguide@0.13.4 qui est l'éléments de base atomiques des flux de travail. 
Vous pouvez les trouver sur la place du marché GitHub (https://github.com/marketplace?type=actions), comme l'action mentionnée. Celui-ci est configuré ( withclause) à utiliser github-pr-review reporter qui permet les commentaires en ligne dans la révision du code. Enfin, ce flux de travail nécessite de passer votre GIHUB_TOKEN et c'est pourquoi la clause env est ajoutée.
