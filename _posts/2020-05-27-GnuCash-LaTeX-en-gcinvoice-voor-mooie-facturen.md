---
title: Mooie facturen met GnuCash, LaTeX en gcinvoice op Linux
date: 2020-05-27
lang: nl
lang-ref: gnucash-latex-gcinvoice # Also create a directory for this in _data/comments. Throws error otherwise. Add empty file there to propagate on Github as well, then remove again.
tags: technologie boekhouding latex gnucash factuur opensource linux
# Run tag_generator.py for every new blog post!
published: true
---

<figure class="fr-ns w-50-ns br3 ma1 ba b--light-gray">
  	<a href="/images/blog/2020/factuur_blogpost.svg">
      <img src="/images/blog/2020/factuur_blogpost.svg" alt="GnuCash_LaTeX_Factuur" class="br3 br--top"></a>
  	<figcaption class="tc">Voorbeeldfactuur met mijn eigen sjabloon.</figcaption>
</figure>

Niet het meest in de lijn der verwachting liggende onderwerp voor een zanger, maar ach.

Als je je eigen boekhouding wilt doen voor je bedrijf is [GnuCash](https://www.gnucash.org/) een prima stukje gratis open source software om dit mee te doen. (Zie voor hulp met het opzetten van je boekhouding met GnuCash [deze uitstekende Nederlandse handleiding](http://www.accoo.nl/handleiding-gnucash/) op de website van Accoo.)

De facturen die GnuCash maakt werken prima, maar zijn wel een beetje kaal. Met behulp van de zetsoftware [LaTeX](https://nl.wikipedia.org/wiki/LaTeX) en het script [gcinvoice](https://bitbucket.org/smoerz/gcinvoice) kan je een LaTeX-sjabloon maken waar je informatie uit je GnuCash-database in giet om een professioneel ogende factuur mee te maken.

## Installatie

Welke software je nodig:

1. [GnuCash](https://www.archlinux.org/packages/community/x86_64/gnucash/)
2. [LaTeX](https://www.archlinux.org/packages/extra/any/texlive-core/)
3. [LaTeX-extra](https://www.archlinux.org/packages/extra/any/texlive-latexextra/)
4. [gcinvoice](https://pypi.org/project/gcinvoice/)
5. (Het [Roboto](https://www.archlinux.org/packages/community/any/ttf-roboto/)lettertype als je dat zoals ik in je sjabloon wilt gebruiken)

Ik gebruik Arch Linux, waar je de eerste drie installeert met het volgende commando:

```console
# pacman -S gnucash texlive-core texlive-latexextra
```

Gcinvoice is een Python-script en het makkelijkst te downloaden met [Pip](https://www.archlinux.org/packages/extra/any/python-pip/), een pakketbeheerder specifiek voor Python. Op Arch Linux:

```console
# pacman -S python-pip
$ pip install gcinvoice
```

Roboto is eventueel ook makkelijk te installeren: 

```console
# pacman -S ttf-roboto
```

Op Ubuntu of andere distributies moet je even kijken hoe bovenstaande pakketten daar heten, maar ze zijn dusdanig standaard dat ze ongetwijfeld in de software repository van je distributie beschikbaar zijn.

Een programma om LaTeX-bestanden mee te verwerken kan ook van pas komen. Ik gebruik daarvoor [texstudio](https://www.archlinux.org/packages/community/x86_64/texstudio/).

## Gebruik

De werkvolgorde is als volgt:

1. Factuur opstellen in GnuCash
2. Database opslaan(!)
3. gcinvoice naar je database wijzen, het factuurnummer opgeven en naar je LaTeX-sjabloon wijzen
4. Het resulterende LaTeX-bestand compileren zodat er een mooi pdf'je uitrolt

<figure class="fr-ns w-50-ns br3 ma1 ba b--light-gray">
  	<a href="/images/blog/2020/template_blogpost_bammerlaan.svg">
      <img src="/images/blog/2020/template_blogpost_bammerlaan.svg" alt="GnuCash_LaTeX_Factuur" class="br3 br--top"></a>
  	<figcaption class="tc">Mijn sjabloon met de nog niet vervangen stukjes Pythoncode. De tabel vormt meerdere regels waar nodig door middel van een <i>for loop</i>.</figcaption>
</figure>

In de sjabloonfactuur verwerk je bepaalde sleutelwoorden (stukjes Pythoncode) die worden vervangen door overeenkomende getalen of tekst uit je GnuCash-database als het bestand door gcinvoice wordt gelezen. Als in het sjabloon bijvoorbeeld ergens `@{owner['full_name']}` staat, vervangt gcinvoice dat door de naam van de klant aan wie je factuur is gericht. Een lijst van de mogelijke stukjes Pythoncode staat [op de webpagina van gcinvoice](https://pypi.org/project/gcinvoice/) onder het kopje "Templates".

## Sjabloon

Mijn sjabloon is te downloaden van mijn [Github-repository](https://github.com/bammerlaan/template_blogpost_bammerlaan). Er staat ook een bash-script bij dat je kan gebruiken om het draaien van het gcinvoice wat mee te vergemakkelijken. In zowel het bash-script als het LaTeX-sjabloon zal je wat wat globale variabelen aan moeten passen aan jouw situatie. Deze heb ik aangegeven met comments.

Mijn sjabloon gebruikt mijn zelfgemaakte achtergrond en logo. Voel je vrij deze te gebruiken, maar met name het logo kan ik je aanraden te veranderen. Voor het mooiste effect gebruik je hiervoor vectorbestanden. Als je al svg-versies hiervan hebt kan je deze via [Inkscape](https://inkscape.org/) gemakkelijk naar pdf exporteren.

De achtergrondafbeelding heb ik gemaakt met [Scribus](https://www.scribus.net/). Het Scribus-bestand staat ook op mijn bovenstaande Github-repository.

## GnuCash gebruiken voor de brieftekst

Door betalingsvoorwaarden aan je factuur toe te voegen kan je gemakkelijk de tekst van je brief automatisch aanpassen aan de situatie. Ga hiervoor in Gnucash naar het menu `MKB → Betalingsvoorwaarden bewerken`. Hier kan je verschillende opties invoeren. Ik heb zelf bijvoorbeeld drie opties: 
- Betaling binnen dertig dagen
- Betaling vóór de eerste les
- Betaling reeds voldaan

Door LaTeX-opmaak te gebruiken in deze voorwaarden kan je ook tekst dikgedrukt of onderstreept of iets dergelijks maken. Door naar de variabelen van het LaTeX-document te verwijzen hoef je maar op één plek iets te veranderen bij wijzigingen.

**Betaling binnen dertig dagen:**
```latex
Ik verzoek u vriendelijk het verschuldigde bedrag
binnen 30 dagen over te maken op rekeningnummer
\usekomavar{frombank} op naam van \textbf{\usekomavar{fromname}}
onder vermelding van uw \underline{naam} en
het \underline{factuurnummer}.
```
**Betaling vóór de eerste les:**
```latex
Ik verzoek u vriendelijk het verschuldigde bedrag
vóór aanvang van de eerste les over te maken
op rekeningnummer \usekomavar{frombank} op naam 
van \textbf{\usekomavar{fromname}} onder vermelding
van uw \underline{naam} en het \underline{factuurnummer}.
```
**Betaling reeds voldaan:**
```latex
Deze factuur is al betaald en is slechts voor uw eigen administratie.
```

In de toekomst hoop ik dit systeem uit te kunnen breiden met een scriptje om automatisch een *SEPA Credit Transfer* QR-code aan de factuur toe te voegen. Deze kunnen klanten dan scannen met de app van hun bank, om zo het betalen makkelijker te maken. Hiervoor kan ik hopelijk de API van de [Online SEPA Credit Transfer QR-code generator](https://epc-qr.eu/) gebruiken.

Heb je iets aan deze blogpost gehad of heb je nog vragen? Laat het me weten via de comments!