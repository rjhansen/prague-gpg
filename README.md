# prague-gpg

Examines your GnuPG keyring to check for conformance to modern standards.

## Why is it called prague?

Because I needed a memorable name, and it Czechs keys.

## How do I run it?

`chmod +x` the script, then just: `./prague`.

## What do I need to run it?

Python 3.6 and GnuPG 2.0.15 or later.  There are no other dependencies.

## What output does it create?

GnuPG will (probably) throw some data to stderr.  prague will throw Excel-formatted `.csv` output to stdout.  You can capture it by `./prague > output.csv` and load it up in the spreadsheet of your choice.

## How reliable is it?

The best I can say is it works for me, and I welcome bug reports.

## Does it do any logging?

Yes: it writes `prague.log` to your homedir.

## How is it licensed?

See the LICENSE file.  Two-clause BSD.  Share and enjoy.
