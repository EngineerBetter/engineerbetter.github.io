---
author: Daniel Jones
date: "2019-04-30"
heroImage: /img/blog/yubikey-signing-emancipation-proclamation.jpg
title: Yubikeys for Signed Git Commits

heading: Our
headingBold: blog
Description: Get the very latest updates about recent projects, team updates, thoughts and industry news from our team of EngineerBetter experts.
---

By signing our Git commits, we can allow folks to verify that they were really written by the author tagged on the commit. If you've got a [Yubikey set up as per our other blog posts](/blog/yubikey-all-the-things/), it's a doddle to configure.

## Setup

With your Yubikey inserted and unlocked, **find the ID of your GPG key**:

```terminal
$ gpg --list-secret-keys --keyid-format LONG
/Users/deejay/.gnupg/pubring.gpg
--------------------------------
sec>  rsa2048/ADD0D0CAFEDECADE 2017-11-22 [SC] [expires: 2021-04-29]
      FEEDBEEFC0C0A7D867D34ADEADD0D0CAFEDECADE
      Card serial no. = 0006 06917459
uid                 [ultimate] Daniel Jones <daniel.jones@engineerbetter.com>
ssb>  rsa2048/AF5A4B5AA8A8C5BF 2017-11-22 [A] [expires: 2021-04-29]
ssb>  rsa2048/BA53B2D6734C8A8E 2017-11-22 [E] [expires: 2021-04-29]
```

Then, **get your public key** so that you can tell GitHub about it. The argument here is the long ID from the above command:

```terminal
$ gpg --armor --export FEEDBEEFC0C0A7D867D34ADEADD0D0CAFEDECADE
-----BEGIN PGP PUBLIC KEY BLOCK-----

mQENBFoVTx8BCAD29H458Y3YwqGoG1HDEDiiAGaZoMONCh0Ql27CI7eSxhB08lI2
[SNIP!]
DVeNShSNf4K9uOS62gEQROZQClA/
=xOmv
-----END PGP PUBLIC KEY BLOCK-----
```

Copy the above public key, including the begin and end blocks, and then [**add it as a new key on GitHub**](https://github.com/settings/gpg/new).

We then need to **tell Git to use GPG to sign commits**, and specifically this key. Use the _short_ ID from the output of the `--list-secret-keys` command we ran earlier. In my example, it follows after `rsa2048`:

```terminal
git config --global commit.gpgsign true
git config --global user.signingkey ADD0D0CAFEDECADE
```

Nearly there! Let's now **restart the GPG agent**:

```terminal
gpg-connect-agent reloadagent /bye
```

## Testing

Make a commit in any repository, and hopefully you shouldn't see an error message.

Run `git log --show-signature` to check that the commit was signed:

```terminal
$ git log --show-signature
commit d1972b6a7a3534413aec91f06d7bd0fdf9bcdc0e (HEAD -> master)
gpg: Signature made Tue 30 Apr 11:50:07 2019 BST
gpg:                using RSA key FEEDBEEFC0C0A7D867D34ADEADD0D0CAFEDECADE
gpg: Good signature from "Daniel Jones <daniel.jones@engineerbetter.com>" [ultimate]
Author: Daniel Jones <daniel.jones@engineerbetter.com>
Date:   Tue Apr 30 11:50:07 2019 +0100

    informative message
```

### Errors?

If when you made the commit you were greeted with the error `git error: gpg failed to sign the data`, there could be a number of causes:

* Git is configured to use the [wrong `gpg` executable](https://stackoverflow.com/a/40066889/774395)
* Your [GPG key is expired and needs extending](https://superuser.com/questions/813421/can-you-extend-the-expiration-date-of-an-already-expired-gpg-key/1141251#1141251)

## Push

Assuming that everything has worked thus far, you can now `git push` and bask in the resplendent glory of a lovely "Verified" badge on GitHub:

<img src="/img/blog/verified-commit.png" class="image fit">

## Why Bother?

You might ask what the benefit of all this is. After all, you're already authing to GitHub with your SSH key, right?

Your SSH key proves that you can talk to GitHub, and that you're allowed access to the repository in question. It _doesn't_ prove that the commits you are pushing were really written by the flagged authors though.

It's trivial to make a commit with a false identity:

```terminal
git commit -m "implement sensible error handling" --author="Rob Pike <rob@google.com>"
```

You can then push this, authenticating with _your_ SSH key (or HTTP basic credentials), which is of course totally valid.

You've then managed to masquerade as a colleague, presumably pushing awesome code to help them get a raise. Or, perhaps, you've done something nefarious. But you'd never do a thing like that, would you?