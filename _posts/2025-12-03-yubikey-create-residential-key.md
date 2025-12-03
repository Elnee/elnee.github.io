---
title: "How to create a resident key with Yubikey and use it on GitHub/Gitlab"
categories:
  - Guides
tags:
  - SSH
  - Yubikey
  - GitHub
  - Gitlab
toc: true
toc_sticky: true
---

## Generating the resident key

Let's use the most secure approach and generate a key that requires both FIDO2
pin and User Presence check (requires tapping the Yubikey). You **don't need**
passphrase for this as the actual key is stored on Yubikey itself and will
never leave it.

```bash
ssh-keygen -t ed25519-sk -O resident -O verify-required -C "your@email.com"
```

Let's assume you choose `resident` as the name for the key file. Then the command
above will create two files:

```
resident
resident.pub
```

You can mistakenly treat these as a private and public keys. But actually it's
only partially true. The `resident.pub` file is a public key but the file that
appears to be a private key is actually a **"key handle file"**. It's not the
private key itself but it contains the information about how the `ssh-agent`
should query the Yubikey to retrieve this exact key we just generated.

In fact, you can even just delete them both and you can acquire a new pair anytime.

### How to regenerate key handle and public key for existing resident key

If you lost the generated files in the previous step you can simply regenerate them
from an existing key that Yubikey is holding. It's done by the next command:

```bash
ssh-keygen -K
```

## Adding the key to GitHub

You can simply add the key as you would do with any other key. Just copy the
content of `resident.pub` file and paste it in the corresponding field on a git
hosting of your choice.

**Warning:** With resident keys it's not an issue to accidentally copy
`resident` file content because it's not the actual key. But anyway, don't do
it! Pay attention and copy the content of public key, not the key handle file.
{: .notice--warning}

## Using with `ssh-agent`

1. In the terminal session start the `ssh-agent`:
```bash
eval `ssh-agent`
```
2. Load the key into the `ssh-agent` from the Yubikey:
```bash
ssh-add -K
```

Aaaaaand, if you try to do, for example, `git fetch` you will most likely get
the next error :)
```
sign_and_send_pubkey: signing failed for ED25519-SK "" from agent: agent refused operation
git@github.com: Permission denied (publickey).
```

But if you don't you're very lucky! Congratulations :) The setup is done for
you. For everyone else who is out of luck let's go to the troubleshooting
section.

### Troubleshooting "agent refused operation" error

What actually happens is the `ssh-agent` has no way to ask you for the FIDO2
pin and User Presence. Yes, the `ssh` itself can do it from the terminal, but
`ssh-agent` is a separate background process and it needs some GUI way to ask
you for this. Even when you type the command like `git fetch` in the terminal.

- For Fedora it's enough to install the `ssh-askpass` dialog. No other
  configuration is needed.
```bash
sudo dnf install openssh-askpass
```

Unfortunately, I have no opportunity to test it on Mac/Windows. But feel free
to share if this problem exists on those operating systems and I will add the
solution to this section. Thank you in advance ;)
