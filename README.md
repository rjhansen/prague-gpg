# prague-gpg

Examines your GnuPG keyring to check for conformance to modern standards.

## Why is it called prague?

Because I needed a memorable name, and it Czechs keys.

## How do I run it?

1. `chmod +x` the script
2. `./prague > output.csv`
3. Open `output.csv` in the spreadsheet of your choice

## What do I need to run it?

UNIX, Python 3.6, and GnuPG 2.0.15 or later.  There are no other dependencies.

## What does it check for?

- **RSA, DSA, and/or Elgamal keys smaller than 3072 bits.**  These are considered too short for long-term security.
- **Elliptical curve keys smaller than 256 bits.**  Likewise.
- **Elgamal signing keys.**  These haven't been supported in many years, and for good reason.
- **SHA-1 shadowing.** Since SHA-1 is a MUST hash algorithm, any hash algorithm listed after it in your key preferences will never be used: GnuPG will reach SHA-1, realize everyone supports it (since it's a MUST), and go no further.  To prevent SHA-1 from being used preferentially, it must be your least-preferred hash.  If SHA-1 is positioned such that other choices will never be reached, these are said to be shadowed.
- **3DES shadowing.** Same as above, except 3DES is the MUST cipher algorithm.
- **Old ciphers being preferred over newer ones.**  Newer ciphers, such as AES, are much better than older ones (Blowfish, CAST5, IDEA, and 3DES).  Supporting old ciphers is okay, but preferring them over newer ones is a bad idea.
- **MD5 support.**  MD5 is a completely broken hash.  If your key advertises MD5, that's a problem.
- **RIPEMD-160 being preferred over the SHA-2 family.**  Similar logic to "old ciphers being preferred over newer ones".
- **Lack of MDC support.**  If your certificate doesn't advertise MDC support, that's a serious problem.

## How do I fix these problems?

- **Short subkeys.** If your subkeys are too short, create new ones with `gpg --edit-key [key ID] addkey` and revoke them with `revkey`.  Upload your new key to the keyservers.  You're done.
  - **Read the GnuPG instructions before you revoke anything.**
- **Short primary keys.**  Add the lines `default-preference-list AES256 AES SHA512 SHA256 BZIP2 ZIP ZLIB` and `cert-digest-algo SHA512` to your `~/.gnupg/gpg.conf` file.  Then generate a new key and migrate to it.
- **Shadowing and preference issues, and/or MD5 support.** `gpg --edit-key [keyid] setpref AES256 AES SHA512 SHA256 BZIP2 ZIP ZLIB` will fix these nicely.  Don't forget to upload your key to the keyservers.
- **Lack of MDC support.**  Good question. Ask Werner.

## What output does it create?

GnuPG will (probably) throw some data to stderr.  `prague` will throw Excel-formatted `.csv` output to stdout.  You can capture it by `./prague > output.csv` and load it up in the spreadsheet of your choice.

## How reliable is it?

The best I can say is it works for me, and I welcome bug reports.

## Does it do any logging?

Yes: it writes `prague.log` to your homedir.

## How is it licensed?

See the LICENSE file.  Two-clause BSD.  Share and enjoy.
