---
title: Automatically adding SEPA Credit Transfer QR codes to GnuCash/LaTeX invoices
date: 2020-06-03
lang: en
lang-ref: sepa-ct-qr-gnucash-latex-invoice # Also create a directory for this in _data/comments. Throws error otherwise. Add empty file there to propagate on Github as well, then remove again.
tags: technology bookkeeping latex gnucash invoice opensource linux qrcodes
# Run tag_generator.py for every new blog post!
published: true
---

<figure class="fr-ns w-50-ns br3 ma1 ba b--light-gray">
  	<a href="/images/blog/2020/template_blogpost_bammerlaan_English_QR.svg">
      <img src="/images/blog/2020/template_blogpost_bammerlaan_English_QR.svg" alt="GnuCash_LaTeX_Factuur" class="br3 br--top"></a>
  	<figcaption class="tc">Invoice with automatically added SEPA Credit Transfer QR code.</figcaption>
</figure>

While working on my [previous post](https://bammerlaan.nl/posts/GnuCash-LaTeX-and-gcinvoice-for-pretty-invoices) on making prettier invoices for your GnuCash invoices, a student asked me if he could also pay using a [Tikkie](https://www.tikkie.me/). This is a Dutch service that lets you send quick payment requests links through whatever messenger app you prefer, quickly allowing recipients to pay through their bank's app.

I looked into this a bit, and it turns out using Tikkie for a business of my size costs € 0,25 per transaction. Not too much in the grand scheme of things, but I'd really like to avoid as many overhead costs as possible for my small business.

## SEPA Credit Transfer QR codes

I came across a [blog post](https://aartjan.nl/blog/qr-code-factuur/) discussing different ways of generating a [SEPA Credit Transfer QR code](https://en.wikipedia.org/wiki/EPC_QR_code), which can be used in a number of European countries for fast payment (Austria, Belgium, Finland, Germany and The Netherlands).

Many Dutch banks support scanning such QR codes with their banking apps on smartphones, making this an interesting alternative for small business owners not wanting to pay Tikkie per transaction.

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
	<figcaption class="tc">Dutch banks supporting SEPA Credit Transfer QR codes.</figcaption>
</figure>

The website [https://epc-qr.eu/](https://epc-qr.eu/) offers a generator for these QR codes, tracker and cookie free, and even offers an API to use in scripts. As I was working on my template anyway, I decided to write a small script to automatically add a QR code to some of my invoices. I wanted it to be an option whether to add a QR code or not, as many of my professional customers probably don't use a banking app for payment.

## Bash script

`download_SEPA_credit_transfer_QR_Code.sh` uses [gcinvoice](https://bitbucket.org/smoerz/gcinvoice) to get the invoice number and the customer name from your GnuCash database. Since I used gcinvoice in my previous post as well and since I was already familiar with its syntax, this seemed easiest.

Find these scripts on my [Github repository](https://github.com/bammerlaan/template_blogpost_bammerlaan/tree/master/SEPA_CT_QR_codes).

`download_SEPA_credit_transfer_QR_Code.sh` can be used separately as well. This may come in handy if you want to add a SCT QR code from your GnuCash invoice to anything else. See `download_SEPA_credit_transfer_QR_Code.sh -h` for usage instructions.

I added a prompt to `Gnucash_make_invoice` to ask if the user wants to add a QR code to his invoice. If yes, it runs `download_SEPA_credit_transfer_QR_Code.sh`.

I added an if/else-statement to the LaTeX template to check if a QR code is present for the invoice number used, which then adds it to the invoice. You'll need to edit the directory it looks in at the bottom of the .tex file. 

Some constants in `Gnucash_make_invoice` to fill in have also been added, check its comments for instructions.


Keep in mind I am very much an amateur programmer, so bugs may be present in all these scripts. Let me know in the comments below if you find any. And also if you have more details about bank apps which support this!
