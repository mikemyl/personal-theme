---
layout: post
section-type: post
title: Unix Password Manager - aka pass
description: Setup unix pass with pgp and git
category: Tech
tags: [ 'tutorial', 'unix', 'git' ]
---


### Introduction

As the number of username-passwords we need to remember increases, password managers are becoming
more and more relevant. By using a password manager we no longer have to revert our password because
we can't remember the variation we used for that particular website, or use the same one everywhere.
But chances are you already know that since you came across
this post, so let's cut to the chase and talk about [pass](https://www.passwordstore.org/), the standard
unix password manager. 

Pass is a simple password manager that stores our credentials in gpg encrypted files, where the filenames correspond to the 
respective titles of the service / website. 

< image with a tree - and a simple password >

We can also utilize the build-in git integration to keep those credentials
synced between our devices. Note that, although our passwords are encrypted (so without our private key they cannot be decrypted)
the filenames are not encrypted so one could see the websites / services that we maintain an account. We could use a private and / or a self-hosted git repository as a workaround. 

### Prerequisites

#### Create a PGP key

In order to start using pass, we need a [PGP key](https://en.wikipedia.org/wiki/Pretty_Good_Privacy) with encryption
capabilities. 

In most Linux distributions GnuPG toolchain should already be installed and is avaliable through their package managers.
If not, get the latest verion from [GnuPG website](https://www.gnupg.org/download/). Make sure you use a GnuPG version > 2
(in Ubuntu for example, that would be the `gpg2` command):

{% highlight shell %}
gpg --version
{% endhighlight %}

<pre>
gpg (GnuPG) 2.2.5
libgcrypt 1.8.2
Copyright (C) 2018 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <https://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Home: /home/mike/foo
Supported algorithms:
Pubkey: RSA, ELG, DSA, ECDH, ECDSA, EDDSA
Cipher: IDEA, 3DES, CAST5, BLOWFISH, AES, AES192, AES256, TWOFISH,
        CAMELLIA128, CAMELLIA192, CAMELLIA256
Hash: SHA1, RIPEMD160, SHA256, SHA384, SHA512, SHA224
Compression: Uncompressed, ZIP, ZLIB, BZIP2
</pre>

So let's generate our gpg key using the following command:

{% highlight shell %}
gpg --full-generate-key
{% endhighlight %}

I went with the default options for the key type (RSA and RSA), and the key size (2048). I could have selected a 
4096-bits long key, but I intend to use with my [Youbikey Neo](https://www.yubico.com/products/yubikey-hardware/yubikey-neo/) and it doesn't
support 4096-bit keys yet. I then specified the valid until date, my name - email, a password. Make sure you don't forget
that key's password cause every password managed by pass is encrypted using the private key's password. But we used to
remember our credentials for all those services that we use so a single key's password won't be much of an issue! 

We can verify that the key was successfully generated, using the command below:

{% highlight shell %}
gpg -K
{% endhighlight %}
<pre>
gpg: checking the trustdb
gpg: marginals needed: 3  completes needed: 1  trust model: pgp
gpg: depth: 0  valid:   1  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 1u
gpg: next trustdb check due at 2019-06-10
/home/mike/foo/pubring.kbx
--------------------------
sec   rsa2048 2018-06-10 [SC] [expires: 2019-06-10]
      15E886BF97A7828A2F5795DBC22FADC6585FDF18
uid           [ultimate] Michail Mylonakis (My gpg key) <mike@mikemylonakis.com>
ssb   rsa2048 2018-06-10 [E] [expires: 2019-06-10]
</pre>  

#### Install pass

Pass is available on all major linux distributions, so it should be easy to install using the package manager. In Arch
linux that would be pacman, and we can easily install pass.

{% highlight shell %}
pacman -S pass
{% endhighlight %}
  
  

#### Initialise pass

In order to set up pass, we need to run the following:

{% highlight shell %}
pass init mike@mikemylonakis.com
{% endhighlight %}

Note that we used the same email address of our secret gpg key.
Let's also enable the git integration:

{% highlight shell %}
pass git init
{% endhighlight %}

Now our password store (the `~/.password-store` directory) is a git repository, so we can utilize git to keep our password
synced between our multiple devices (we ll see how in a next section).


####   Export the private key

Now let's export our private key so that we can import it into our other devices. It also makes sense to save it somewhere
"safe" as a backup.

{% highlight shell %}
gpg --export-secret-keys > secret.asc
{% endhighlight %}


### Using pass


