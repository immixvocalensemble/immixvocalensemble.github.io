---
title: Mooie facturen met GnuCash, LaTeX en gcinvoice op Linux
date: 2020-05-30
lang: nl
lang-ref: gnucash-latex-gcinvoice # Also create a directory for this in _data/comments. Throws error otherwise. Add empty file there to propagate on Github as well, then remove again.
tags: technologie boekhouding latex gnucash factuur opensource linux
# Run tag_generator.py for every new blog post!
published: false
---

<figure class="fr-ns w-50-ns br3 ma1 ba b--light-gray">
  	<a href="/images/blog/2020/factuur_blogpost.svg">
      <img src="/images/blog/2020/factuur_blogpost.svg" alt="GnuCash_LaTeX_Factuur" class="br3 br--top"></a>
  	<figcaption class="tc">Voorbeeldfactuur met mijn eigen sjabloon.</figcaption>
</figure>

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

Op Ubuntu of andere distributies moet je even kijken hoe bovenstaande pakketten daar heten, maar ze zijn dusdanig standaard dat ze ongetwijfeld in de software repository van je distributie beschikbaar zijn.

Een programma om LaTeX-bestanden mee te verwerken kan ook van pas komen. Ik gebruik daarvoor [texstudio](https://www.archlinux.org/packages/community/x86_64/texstudio/).

## Gebruik

De werkvolgorde is als volgt:

1. Factuur opstellen in GnuCash
2. Database opslaan(!)
3. gcinvoice naar je database wijzen, het factuurnummer opgeven en naar je LaTeX-sjabloon wijzen
4. Het resulterende LaTeX-bestand compileren zodat er een mooi pdf'je uitrolt

In de sjabloonfactuur verwerk je bepaalde sleutelwoorden die worden vervangen door overeenkomende getalen of tekst uit je GnuCash-database als het bestand door gcinvoice wordt gelezen. Als in het sjabloon bijvoorbeeld ergens `@{owner['full_name']}` staat, vervangt gcinvoice dat door de naam van de klant aan wie je factuur is gericht. Een lijst van de mogelijke sleutelwoorden staat [op de webpagina van gcinvoice](https://pypi.org/project/gcinvoice/).

