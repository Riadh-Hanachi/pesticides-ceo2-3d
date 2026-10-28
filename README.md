# Pesticides nitrés et CeO₂(111) en 3D

Visualisation 3D interactive (navigateur) de résultats DFT :

- **Orbitales des pesticides** : HOMO et LUMO du méthyl-parathion et de l'imidaclopride (isosurfaces réglables, groupe NO₂ repéré).
- **Surfaces CeO₂(111)** : la surface seule, puis avec le méthyl-parathion ou l'imidaclopride adsorbé (répétition 2 × 2, vues de côté et de dessus).

La page utilise [3Dmol.js](https://3dmol.csb.pitt.edu/) (chargé depuis cdnjs) ; une connexion Internet est nécessaire.

## Voir la page

**GitHub Pages** : dans le dépôt, *Settings → Pages → Source : Deploy from a branch*, branche `main`, dossier `/ (root)`. La page est ensuite servie à `https://<utilisateur>.github.io/<dépôt>/`. Le lien `…/#surfaces` ouvre directement l'onglet des surfaces.

**En local** : la page charge ses données avec `fetch`, ce qui ne fonctionne pas en ouvrant `index.html` par double-clic. Servez le dossier :

```bash
python -m http.server 8000
```

puis ouvrez http://localhost:8000.

## Contenu

| Fichier | Contenu |
|---|---|
| `index.html` | page complète (HTML, CSS, JavaScript) |
| `data/orbitals/*.cube` | HOMO et LUMO des deux molécules, format Gaussian cube, grille allégée à 0,30 bohr |
| `data/surfaces.json` | géométries relaxées des trois surfaces (XYZ, maille, indices des atomes de la molécule) |
| `.nojekyll` | désactive le traitement Jekyll de GitHub Pages |

## Méthodes

- **Molécules** (Psi4 1.11) : recherche de conformères RDKit/MMFF94, optimisation B3LYP-D3(BJ)/6-31G(d,p), fréquences (aucune fréquence imaginaire), orbitales B3LYP-D3(BJ)/6-311+G(d,p). Le poids du LUMO sur NO₂ est une population de Mulliken de l'orbitale (66 % pour le méthyl-parathion, 70 % pour l'imidaclopride ; robuste à ±3 points entre Mulliken/Löwdin et 6-31G(d,p)/6-311+G(d,p)).
- **Surfaces** (Quantum ESPRESSO 7.5) : CeO₂(111) p(3×3), deux tricouches O–Ce–O (tricouche inférieure gelée), PBE+U (U = 5 eV sur Ce 4f) + D3(BJ), 50/400 Ry, point Γ, correction dipolaire.

| Système | E_ads | Remarque |
|---|---|---|
| Méthyl-parathion / CeO₂(111) | −0,22 eV | physisorption, O(nitro)–Ce 2,86 Å, transfert de charge < 0,05 e |
| Imidaclopride / CeO₂(111) | −0,82 eV | O(nitro)–Ce 2,67 Å, liaison H N–H···O(surface) 1,94 Å, transfert de charge < 0,1 e |

Les relaxations des systèmes adsorbés n'ont pas atteint le critère de force (surfaces d'énergie très plates) : les E_ads sont fiables à environ 0,01 eV (méthyl-parathion) et constituent une borne pour l'imidaclopride, dont l'énergie baissait encore lentement.

## Références logicielles

- D. G. A. Smith et al., *J. Chem. Phys.* 152, 184108 (2020) — Psi4
- P. Giannozzi et al., *J. Phys.: Condens. Matter* 29, 465901 (2017) — Quantum ESPRESSO
- N. Rego, D. Koes, *Bioinformatics* 31, 1322 (2015) — 3Dmol.js
