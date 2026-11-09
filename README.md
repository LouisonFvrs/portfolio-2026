# Portfolio – Louison Feuvrais

Site statique d'une page, sans build ni dépendance. On l'ouvre tel quel ou on le sert avec n'importe quel serveur HTTP.

## Structure

```
index.html                 page unique (HTML, CSS et JS inline)
favicon.svg                favicon principal
favicon-32.png             favicon de repli (anciens navigateurs)
apple-touch-icon.png       icône d'écran d'accueil iOS (180×180)
og-image.png               aperçu des partages LinkedIn, Slack… (1200×630)
CNAME                      domaine personnalisé déclaré à GitHub Pages
robots.txt, sitemap.xml    indications pour les moteurs de recherche (Google Search Console)
assets/
  fonts/                   Geist et Geist Mono variables + licence OFL
  img/                     avatar et logos des écoles / entreprise
  video/                   démo du portail (MP4) + image de couverture
ressources/cv.pdf          CV lié par les boutons « Mon CV » et le dock (généré depuis cv/cv.html)
cv/cv.html                 source du CV, au format A4
```

## Tester en local

```bash
python -m http.server 8000
```

Puis ouvrir http://localhost:8000. Passer par un serveur plutôt que double-cliquer sur le fichier : la vidéo et les polices se chargent plus fiablement.

## Mise en ligne sur GitHub Pages

Le site est publié depuis le dépôt public `LouisonFvrs/portfolio-2026`, branche `main`, dossier racine, sur `https://louisonfeuvrais.fr/` (HTTPS forcé).

Chaque `git push` sur `main` redéploie le site en 1 à 2 minutes ; l'onglet **Actions** montre le déploiement *pages build and deployment*.

L'ancien site `LouisonFvrs/Portfolio` reste en ligne tant qu'on ne le désactive pas. Une fois le nouveau validé, on peut le désactiver ou y mettre une redirection.

## Domaine louisonfeuvrais.fr

Le domaine est enregistré chez OVHcloud. Le fichier `CNAME` du dépôt le déclare déjà à GitHub Pages, et les URL absolues de `index.html` (`canonical`, `og:url`, `og:image`) pointent dessus.

1. **Dans l'espace client OVH**, *Web Cloud → Noms de domaine → louisonfeuvrais.fr → Zone DNS* :
   - supprimer les enregistrements **A** et **AAAA** de `louisonfeuvrais.fr.` créés par défaut, ainsi que le **CNAME** de `www` s'il existe ;
   - ne pas toucher aux enregistrements **MX**, **SPF** et **SRV** (e-mail Zimbra) ;
   - ajouter :

   | Type | Sous-domaine | Cible |
   |---|---|---|
   | A | *(vide)* | `185.199.108.153` |
   | A | *(vide)* | `185.199.109.153` |
   | A | *(vide)* | `185.199.110.153` |
   | A | *(vide)* | `185.199.111.153` |
   | CNAME | `www` | `louisonfvrs.github.io.` |

2. **Sur GitHub**, *Settings → Pages → Custom domain* : vérifier que `louisonfeuvrais.fr` est bien repris depuis le fichier `CNAME`, sinon le saisir et *Save*.
3. Une fois le DNS propagé (quelques minutes à 24 h), cocher **Enforce HTTPS**.
4. Recommandé : ajouter le domaine dans les réglages du compte GitHub (*Settings → Pages → Verified domains*), pour éviter qu'un autre compte puisse le revendiquer.
5. Dans OVH, vérifier que le **renouvellement automatique** est actif et qu'un moyen de paiement valide est enregistré (renouvellement : 7,79 € HT par an).

## Après la mise en ligne

- **Référencement** : propriété `louisonfeuvrais.fr` validée dans Google Search Console (enregistrement TXT `google-site-verification` à garder dans la zone DNS OVH). Mettre à jour `<lastmod>` dans `sitemap.xml` après un changement important.
- **Aperçu LinkedIn** : coller l'URL dans le [Post Inspector](https://www.linkedin.com/post-inspector/) pour vérifier l'image et forcer LinkedIn à rafraîchir son cache.
- **Confidentialité** : faire valider la vidéo et la description du Portail client par Etix Everywhere avant de diffuser le lien.

## Mettre à jour le contenu

| Quoi | Où |
|---|---|
| Ajouter une vidéo de projet | Déposer `assets/video/<projet>.mp4`, puis remplacer `<span>…gif</span>` dans le `<figure class="media">` de la carte par une balise `<video … autoplay muted loop playsinline>` sur le modèle du Portail |
| Liens dépôt / démo | Bloc `.links` de chaque projet (un commentaire HTML le signale) |
| Changer le CV | Modifier `cv/cv.html`, puis régénérer `ressources/cv.pdf` (voir ci-dessous) |

Régénérer le CV en PDF (serveur local lancé à la racine, comme dans « Tester en local ») :

```bash
"/c/Program Files (x86)/Microsoft/Edge/Application/msedge.exe" --headless=new --no-pdf-header-footer --virtual-time-budget=5000 --print-to-pdf="ressources/cv.pdf" http://localhost:8000/cv/cv.html
```

Vérifier que le PDF tient sur une seule page. Le numéro de téléphone n'est volontairement pas dans cette version publique.

Convertir un GIF en MP4 léger avec ffmpeg :

```bash
ffmpeg -i demo.gif -movflags +faststart -c:v libx264 -preset slow -crf 26 -pix_fmt yuv420p -vf "scale=trunc(iw/2)*2:trunc(ih/2)*2,fps=25" -an assets/video/demo.mp4
ffmpeg -i demo.gif -frames:v 1 -q:v 4 assets/video/demo.jpg
```

## Licences

Polices Geist et Geist Mono © Vercel, sous SIL Open Font License 1.1 (voir `assets/fonts/LICENSE.txt`).
