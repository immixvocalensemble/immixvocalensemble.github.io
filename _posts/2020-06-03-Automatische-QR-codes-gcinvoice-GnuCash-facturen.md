---
title: Automatisch SEPA Credit Transfer QR codes toevoegen aan GnuCash-/LaTeX-facturen
date: 2020-06-03
lang: nl
lang-ref: sepa-ct-qr-gnucash-latex-invoice # Also create a directory for this in _data/comments. Throws error otherwise. Add empty file there to propagate on Github as well, then remove again.
tags: technologie boekhouding latex gnucash factuur opensource linux qrcodes
# Run tag_generator.py for every new blog post!
published: true
---

<figure class="fr-ns w-50-ns br3 ma1 ba b--light-gray">
    <a href="/images/blog/2020/template_blogpost_bammerlaan_English_QR.svg">
    <img src="/images/blog/2020/template_blogpost_bammerlaan_English_QR.svg" alt="GnuCash_LaTeX_Factuur" class="br3 br--top"></a>
    <figcaption class="tc">Factuur met automatisch toegevoegde SEPA Credit Transfer QR-code.</figcaption>
</figure>

Terwijl ik werkte aan mijn [vorige blogpost](https://bammerlaan.nl/posts/GnuCash-LaTeX-en-gcinvoice-voor-mooie-facturen) over het maken van mooie facturen met LaTeX en GnuCash, vroeg een student me of hij ook kon betalen met een [Tikkie](https://www.tikkie.me/): een vrij bekende site die je makkelijk met mobiel bankieren geld kan laten overmaken.

Ik heb dit even uitgezocht, en het blijkt dat Tikkie voor een bedrijfje van mijn formaat € 0,25 per transactie kost. Niet extreem veel, natuurlijk, maar ik probeer toch zo min mogelijk extra kosten te maken met mijn kleine bedrijf.

## SEPA Credit Transfer QR-codes

Ik kwam een [blogpost](https://aartjan.nl/blog/qr-code-factuur/) tegen waarop verschillende methodes worden besproken voor het maken van een [SEPA Credit Transfer QR-code](https://nl.wikipedia.org/wiki/EPC_betaling_QR-Code). In een aantal Europese landen kan dit worden gebruikt om snel geld over te maken (Oostenrijk, Finland, Duitsland en Nederland).

De meeste Nederlandse banken ondersteunen dergelijke QR-codes op hun apps voor bankieren op smartphones. Een interessant alternatief dus voor zzp'ers die geen extra (Tikkie-)kosten willen maken.

<figure class="fr-ns w-25-ns br3 ma1 ba b--light-gray">
    <table>
    <tbody>
    <tr>
        <td>✔?</td>
        <td>ING</td>
    </tr>
    <tr>
        <td>✔</td>
        <td>Bunq</td>
    </tr>
    <tr>
        <td>✔</td>
        <td>SNS bank</td>
    </tr>
    <tr>
        <td>x</td>
        <td>Rabobank</td>
    </tr>
    <tr>
        <td>x</td>
        <td>ABN Amro</td>
    </tr>
    <tr>
        <td>✔</td>
        <td>ASN Bank</td>
    </tr>
    <tr>
        <td>✔?</td>
        <td>Knab</td>
    </tr>
    </tbody>
    </table>
    <figcaption class="tc">Nederlandse banken die SEPA Credit Transfer QR codes ondersteunen.</figcaption>
</figure>

De website [https://epc-qr.eu/](https://epc-qr.eu/) heeft een generator voor dergelijke QR-codes zonder trackers en cookies, en biedt zelfs een API om in scripts te gebruiken. Ik heb dus een klein scriptje geschreven in uitbreiding op mijn vorige blogpost om automatisch een QR-code aan facturen toe te kunnen voegen. Niet op elke factuur, want veel van mijn zakelijke klanten gebruiken waarschijnlijk geen app om geld over te maken.

## Bash script

`download_SEPA_credit_transfer_QR_Code.sh` is het resultaat, en gebruikt [gcinvoice](https://bitbucket.org/smoerz/gcinvoice) om het factuurnummer en de klantnaam uit je GnuCash-database te halen. Ik gebruikte gcinvoice ook al in mijn vorige blogpost en ik was al bekend met de syntax, dus dat leek me het eenvoudigst.

De nieuwe scriptjes zijn te vinden op mijn [Github-repository](https://github.com/bammerlaan/template_blogpost_bammerlaan/tree/master/SEPA_CT_QR_codes).

`download_SEPA_credit_transfer_QR_Code.sh` kan ook los worden gebruikt. Dat kan van pas komen als je een SCT QR-code uit je GnuCashfactuur met iets anders wilt gebruiken. Zie `download_SEPA_credit_transfer_QR_Code.sh -h` voor gebruiksinstructies.

Ik heb ook `Gnucash_make_invoice` aangepast zodat de gebruiker wordt gevraagd of hij een QR-code aan zijn factuur toe wilt voegen. Vervolgens wordt al deze QR-code al dan niet gemaakt met `download_SEPA_credit_transfer_QR_Code.sh`.

Aan het LaTeX-sjabloon is een *if/else statement* toegevoegd die controleert of er een QR-code gemaakt is voor het gebruikte factuurnummer en, zo ja, deze aan de factuur toevoegt. Hiervoor moet je onderin het .tex-bestand twee keer de map invullen waar je je QR-codes laat opslaan. 

Er zijn ook wat constanten aangepast in `Gnucash_make_invoice` die je zal moeten invullen, kijk voor meer informatie daarover in de *comments* van het bestand.

Ik ben slechts aan amateurprogrammeur, dus er zouden heel goed fouten in m'n scriptjes kunnen zitten. Laat het me maar weten in de comments als je iets tegenkomt. En laat het me ook graag weten als je meer informatie hebt over bankapps die dit al dan niet ondersteunen!
