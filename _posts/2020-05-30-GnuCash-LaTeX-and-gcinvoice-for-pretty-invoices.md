---
title: Pretty invoices with GnuCash, LaTeX and gcinvoice using Linux
date: 2020-05-30
lang: en
lang-ref: gnucash-latex-gcinvoice # Also create a directory for this in _data/comments. Throws error otherwise. Add empty file there to propagate on Github as well, then remove again.
tags: technology bookkeeping latex gnucash invoice opensource linux
# Run tag_generator.py for every new blog post!
published: true
---

<figure class="fr-ns w-50-ns br3 ma1 ba b--light-gray">
  	<a href="/images/blog/2020/invoice_blogpost.svg">
      <img src="/images/blog/2020/invoice_blogpost.svg" alt="GnuCash_LaTeX_Factuur" class="br3 br--top"></a>
  	<figcaption class="tc">Example invoice lazily translated to English.</figcaption>
</figure>

Perhaps not the first subject you'd expect on the blog of a classical singer, but we all have our quirks.

If you want to do your own bookkeeping for your company, [GnuCash](https://www.gnucash.org/) is an excellent piece of free open source software to do it with.

The invoices GnuCash produces work fine, but leave something to be desired in terms of customization. Using the typesetting software [LaTeX](https://www.latex-project.org/) and the script [gcinvoice](https://bitbucket.org/smoerz/gcinvoice) you can make a LaTeX template, into which you pour the information from your GnuCash database to make a professional-looking invoice.

## Installation

What software you need:

1. [GnuCash](https://www.archlinux.org/packages/community/x86_64/gnucash/)
2. [LaTeX](https://www.archlinux.org/packages/extra/any/texlive-core/)
3. [LaTeX-extra](https://www.archlinux.org/packages/extra/any/texlive-latexextra/)
4. [gcinvoice](https://pypi.org/project/gcinvoice/)
5. (The [Roboto](https://www.archlinux.org/packages/community/any/ttf-roboto/) font if you want to use it in your invoice, like I do)

I use Arch Linux, where you install the first three with the following command:

```console
# pacman -S gnucash texlive-core texlive-latexextra
```

Gcinvoice is a Python script and easiest to download using [Pip](https://www.archlinux.org/packages/extra/any/python-pip/), a package manager specifically for Python. On Arch Linux:

```console
# pacman -S python-pip
$ pip install gcinvoice
```

Roboto is also easy to install, if desired:

```console
# pacman -S ttf-roboto
```

You'll need to find out what these packages are called on Ubuntu or other distributions. They are pretty standard, though, so they're likely to be present in your distribution's repository.

A program to help with manipulating LaTeX files can also come in handy. I use [texstudio](https://www.archlinux.org/packages/community/x86_64/texstudio/) for that.

## Usage

The working order is like this:

1. Make an invoice in GnuCash
2. Save your database(!)
3. Point gcinvoice to your database, enter the invoice number and point it to your LaTeX template
4. Compile the resulting LaTeX file to get a pretty pdf file

<figure class="fr-ns w-50-ns br3 ma1 ba b--light-gray">
  	<a href="/images/blog/2020/template_blogpost_bammerlaan_English.svg">
      <img src="/images/blog/2020/template_blogpost_bammerlaan_English.svg" alt="GnuCash_LaTeX_Factuur" class="br3 br--top"></a>
  	<figcaption class="tc">My template with the yet-to-be-replaced pieces of Python code. The table will show line breaks when necessary using a for loop.</figcaption>
</figure>

You need to place pieces of Python code in the invoice template, which get replaced by the corresponding numbers or text from your GnuCash database when you run gcinvoice. If, for example, you write `@{owner['full_name']}` in your template, gcinvoice will replace this with the name of the customer you addressed your invoice to. A list of the possible pieces of Python code can be found [on the web page of gcinvoice](https://pypi.org/project/gcinvoice/) under the header "Templates".

## Template

My template can be downloaded from my [Github repository](https://github.com/bammerlaan/template_blogpost_bammerlaan). Also included is a bash script you can use to automate using gcinvoice. In both the bash script and the template you will need to edit some global variables to your situation. These are indicated with comments.

My template uses my self-made background image and logo. Feel free to use these, though you'll probably want to change the logo, considering. For the prettiest results I recommend using vector images for these. If you already have svg files of your images, you can easily export them to pdf using [Inkscape](https://inkscape.org/).

The background image was made with [Scribus](https://www.scribus.net/). The Scribus file can also be found on my Github repository.

## Using GnuCash for the body text

By using billing terms to your invoice, you can automatically change the body text of your letter for different situations. In GnuCash, go to the menu `Business → Billing Terms Editor`. You can add different options here. I myself use three different ones:

- Payment within thirty days
- Payment before the first lesson
- Payment already received

By using LaTeX markup in these billing terms, you can make text bold or underlined or similar. By referencing to the variables used in the LaTeX document, you only need to make changes there when something changes.

**Payment within thirty days:**

```latex
For my services I kindly request the following payment,
to be transferred within 30 days to \usekomavar{frombank}
under the name of \textbf{\usekomavar{fromname}}.
Please mention your \underline{name} and the
\underline{invoice number}.
```

**Payment before the first lesson:**

```latex
For my services I kindly request the following payment,
to be transferred before the first lesson to
\usekomavar{frombank} under the name of
\textbf{\usekomavar{fromname}}. Please mention your
\underline{name} and the \underline{invoice number}.
```

**Payment already received:**

```latex
This invoice has already been paid and is merely for
your own administration.
```

In the future, I hope to expand on this with a script to add a SEPA Credit Transfer QR code to the invoice, to make payment easier for customers, using the API from the [Online SEPA Credit Transfer QR-code generator](https://epc-qr.eu/). UPDATE: I have done this, see my next blog post.

Did this blog post help you or do you still have questions or tips? Let me know in the comments!
