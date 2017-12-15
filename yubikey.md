Gotchas - Yubikey
=================

Windows
-------

On windows, if you are using Kleopatra, signing/decrypting payloads with a gpg
key stored on a yubikey may not work until you open up powershell and run:

`gpg --card-status`


