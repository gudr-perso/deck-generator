# Catalogue des archétypes

Chaque diapo générée est un `<div class="slide [s-xxx]" [data-theme="light"]>…</div>` inséré dans {{SLIDES}}. Les tokens `{{…}}` sont remplis depuis le plan.md. La 1re diapo du deck porte en plus la classe `active`. Le HTML des patrons est repris VERBATIM du deck de référence (seul le contenu est tokenisé).

## cover
Rôle : couverture d'ouverture (fond sombre).
Patron :
```html
<div class="slide s-cover active" data-i="{{i}}"><div class="inner">
  <div class="tag">{{tag}}</div>
  <h1>{{titre}}</h1>
  <p class="sub">{{sous_titre}}</p>
  <div class="meta">{{meta}}</div>
</div></div>
```
Champs : tag, titre (peut contenir `<em>…</em>`), sous_titre, meta.

## agenda
Rôle : déroulé horodaté (fond clair).
Patron :
```html
<div class="slide s-agenda" data-theme="light" data-i="{{i}}"><div class="inner">
  <div class="eyebrow light">{{eyebrow}}</div><h2>{{titre}}</h2>
  <div class="agenda-list">{{items}}</div>
</div></div>
```
Item : `<div class="ag-item"><span class="ag-time">{{hh:mm}}</span><span class="ag-label">{{label}}</span><span class="ag-tag">{{tag}}</span></div>`
Champs : eyebrow, titre, items[] (heure `hh:mm`, label, tag).

## divider
Rôle : intercalaire de partie (fond sombre).
Patron :
```html
<div class="slide s-div" data-i="{{i}}"><div class="inner">
  <div class="bignum">{{numero}}</div>
  <h2>{{titre}}</h2>
  <p>{{texte}}</p>
  <span class="time-pill">{{duree_pill}}</span>
</div></div>
```
Champs : numero (ex. `01`), titre, texte, duree_pill (optionnel, ex. `⏱ ≈ 30 minutes`).

## big-title
Rôle : affirmation forte / définition mise en avant.
Patron :
```html
<div class="slide s-big" data-i="{{i}}"><div class="inner">
  <h2>{{titre}}</h2>
  <div class="lead-card"><p>{{lead}}</p></div>
  <p class="punch">{{punch}}</p>
</div></div>
```
Champs : titre, lead (paragraphe cadre), punch (phrase de chute, optionnelle).

## points
Rôle : liste de points clés en cartes (fond clair).
Patron :
```html
<div class="slide s-points" data-theme="light" data-i="{{i}}"><div class="inner">
  <div class="eyebrow light">{{eyebrow}}</div><h2>{{titre}}</h2>
  <div class="grid">{{items}}</div>
</div></div>
```
Item : `<div class="pain-item"><div class="pain-icon ok">{{icone}}</div><div class="pain-text"><strong>{{titre_item}}</strong>{{texte_item}}</div></div>`
Champs : eyebrow, titre, items[] (icone ex. `✓`, titre_item, texte_item). 3–6 items.

## stats
Rôle : chiffres clés / idées numérotées.
Patron :
```html
<div class="slide s-stats" data-i="{{i}}"><div class="inner">
  <div class="eyebrow">{{eyebrow}}</div><h2>{{titre}}</h2>
  <div class="stats-row">{{items}}</div>
</div></div>
```
Item : `<div class="stat-box"><div class="stat-num">{{num}}</div><div class="stat-label">{{label}}</div></div>`
Champs : eyebrow, titre, items[] (num ex. `01` ou `72%`, label).

## image-full
Rôle : visuel plein cadre avec légende (fond clair).
Patron :
```html
<div class="slide s-img" data-theme="light" data-i="{{i}}"><div class="inner">
  <div class="eyebrow light">{{eyebrow}}</div><h2>{{titre}}</h2>
  <figure class="figure"><img src="{{image_base64}}" alt="{{alt}}"><figcaption>{{caption}}</figcaption></figure>
</div></div>
```
Champs : eyebrow, titre, image_base64 (data URI intégré à l'assemblage), alt, caption.

## list
Rôle : liste numérotée de ressources / étapes (fond clair).
Patron :
```html
<div class="slide s-list" data-theme="light" data-i="{{i}}"><div class="inner">
  <div class="eyebrow light">{{eyebrow}}</div><h2>{{titre}}</h2>
  <div class="num-list">{{items}}</div>
</div></div>
```
Item : `<div class="num-item"><div class="num-badge">{{n}}</div><div class="num-body"><strong>{{titre_item}}</strong> {{texte_item}}</div></div>`
Champs : eyebrow, titre, items[] (n, titre_item, texte_item).

## cta
Rôle : appel à l'action / conclusion (fond clair).
Patron :
```html
<div class="slide s-cta" data-theme="light" data-i="{{i}}"><div class="inner">
  <div class="eyebrow onpeach" style="justify-content:center;margin-bottom:20px">{{eyebrow}}</div>
  <h2>{{titre}}</h2>
  <p class="sub">{{sous_titre}}</p>
  <p class="tagline">{{tagline}}</p>
</div></div>
```
Champs : eyebrow, titre, sous_titre, tagline.

## qa
Rôle : diapo finale « questions & échanges » (variante de `cta`, fond clair).
Patron :
```html
<div class="slide s-cta" data-theme="light" data-i="{{i}}"><div class="inner">
  <div class="eyebrow onpeach" style="justify-content:center;margin-bottom:20px">{{eyebrow}}</div>
  <h2>{{titre}}</h2>
  <p class="sub">{{sous_titre}}</p>
  <p class="tagline">{{tagline}}</p>
</div></div>
```
Champs : eyebrow (ex. `On échange`), titre (ex. `Des questions ? <em>Parlons-en.</em>`), sous_titre, tagline. Utilise la classe `s-cta` du moteur ; `qa` n'est qu'un rôle sémantique de clôture.
