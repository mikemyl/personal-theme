---
layout: post
section-type: post
title: Unix Password Manager - aka pass
description: Setup unix pass with pgp and git
category: Tech
tags: [ 'tutorial', 'unix', 'git' ]
---

#### Introduction

As the number of username-passwords we need to remember increases, password managers are becoming
more and more relevant. By using a password manager we no longer have to revert our password because
we don't remember the variation we used for that particular website, or use the same one everywhere.
But chances are you already know that since you came across
this post, so let's cut to the chase and talk about [unix pass](https://www.passwordstore.org/), the standard
unix password manager. 

#### Prerequisites

In order to start using pass, we need a [PGP key](https://en.wikipedia.org/wiki/Pretty_Good_Privacy) with encryption
capabilities. 

In most Linux distributions GnuPG toolchain should already be installed and it is avaliable through their package managers.
If not, get the latest verions from [GnuPG website](https://www.gnupg.org/download/). Make sure you use a GnuPG version > 2
(in Ubuntu for example, that would be the `gpg2` command).  

So let's generate our gpg key. 

{% highlight shell %}
gpg --full-generate-key
{% endhighlight %}

I went with the default options for the key type (RSA and RSA), and the key size (2048). I could have selected a 4096-bits long
key, but I intend to use with my [Youbikey Neo](https://www.yubico.com/products/yubikey-hardware/yubikey-neo/) and it doesn't
support 4096-bit keys yet. I then specified that the key should be valid for 1 year, my name - email, a password (for the key),
and my key was generated, as seen below.

<pre>
</pre>
