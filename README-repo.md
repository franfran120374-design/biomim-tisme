# Biomimétisme

> La biologie est un catalogue de solutions dont on a perdu la table des matières.
> Ce dépôt est une tentative de la reconstruire — indexée par **fonction**, jamais par organisme.

Trois choses, dans un seul dépôt :

1. **Un catalogue** de 43 stratégies du vivant (`data/strategies.json`), chacune avec son **principe abstrait** — la formulation d'où toute biologie a été retirée. C'est cette formulation, et elle seule, qui est transposable.
2. **Un atelier web** (`index.html`) : catalogue filtrable par fonction, niveau, domaine et statut, plus la fiche de protocole en 7 étapes, exportable en Markdown.
3. **Un dossier de fond** (`docs/`) : 29 pages, 12 figures, 80 références. Découvrir → douter → faire, puis projections sur l'IA et sur la santé.

## Démarrage

```powershell
# Windows / PowerShell
.\scripts\serve.ps1        # http://localhost:8000
```

```bash
# Unix
make serve
```

Le catalogue est chargé par `fetch()` : ouvrir `index.html` en double-cliquant **ne fonctionne pas** (protocole `file://`). Il faut un serveur HTTP, même minimal.

## Structure

```
index.html               atelier web (GitHub Pages sert la racine)
app/                     styles.css, app.js — vanilla JS, zéro dépendance
data/
  strategies.json        LE catalogue — c'est l'actif du dépôt
  schema.json            contrat de données
docs/
  dossier.md             source du dossier
  fig/                   12 figures générées, reproductibles
  biomimetisme-dossier.docx
  guide-pedagogique.html version courte et vulgarisée
tools/
  figures.py             régénère les 12 figures (matplotlib)
  make_reference.py      styles Word (palette, en-tête, pagination)
  build_docx.py          md + figures → docx via pandoc
  validate.py            valide le catalogue (voir ci-dessous)
scripts/                 init-repo.ps1, build.ps1, serve.ps1
```

## Le validateur, et pourquoi il est sévère

`tools/validate.py` refuse toute entrée dont le champ `principe` contient un mot de biologie — nom d'organisme, d'organe, de molécule. Ce n'est pas du zèle : c'est l'**étape 4** du protocole, celle que tout le monde saute, et la seule qui décide si vous transposez ou si vous décorez.

```
« les fourmis déposent des phéromones qui s'évaporent »          → REFUSÉ
« des agents modifient un support partagé et persistant dont
  l'état s'atténue avec le temps »                                → ACCEPTÉ
```

Le second énoncé s'applique à un système logiciel, à une chaîne logistique ou à une organisation humaine. Le premier ne s'applique qu'aux fourmis.

Le validateur tourne aussi en CI avant chaque déploiement Pages.

## Reconstruire le dossier

```powershell
.\scripts\build.ps1
```

Prérequis : Python 3, `pandoc`, et les paquets de `tools/requirements.txt`. Tout est reproductible depuis les sources — les figures sont du vectoriel généré, pas des images récupérées.

## Publier

```powershell
.\scripts\init-repo.ps1 -Owner <votre-org> -Name biomimetisme
```

Puis **Settings → Pages → Source : GitHub Actions**. Le workflow valide le catalogue avant de déployer.

## Trois avertissements

**« La nature a raison » est un sophisme.** L'évolution n'optimise pas, elle satisfait. Elle bricole à partir de l'existant, produit des optima locaux et des absurdités structurelles. Le vivant est un générateur d'hypothèses, pas une autorité.

**L'échelle tue les transpositions.** Chaque entrée porte son ordre de grandeur (`echelle_m`). Au-delà de deux décades d'écart avec votre application, identifiez explicitement quelle physique domine de chaque côté. Si ce n'est pas la même, le principe ne tient pas.

**Sans comparateur, il n'y a rien.** L'étape 7 exige une alternative conventionnelle et une métrique mesurée. Sans elle, vous avez produit de la décoration conceptuelle.

Les transpositions portent un statut : `industrialisé`, `prototype`, `hypothèse`. Les hypothèses sont des pistes d'idéation. En santé en particulier, **elles ne sont jamais des recommandations cliniques**.

## Licences

Contenu rédactionnel et données (`docs/`, `data/`) : **CC BY-SA 4.0**.
Code (`app/`, `tools/`, `scripts/`) : **MIT**. Voir `LICENSE`.
