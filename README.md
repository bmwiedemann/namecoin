Namecoin
===================

See https://dot-bit.org/ for more project information.

Namecoin is a peer to peer naming system based on bitcoin.  It is a secure and censorship resistant replacement for DNS.

Ownership of a name is based on ownership of a coin, which is in turn based on public key cryptography.  The namecoin network reaches consensus every few minutes as to which names have been reserved or updated.

It is envisioned that the .bit domain be used for gluing namecoin domain names into the DNS.  This can be done close to the user or even by the user.

See FAQ.md for more general information.

News
=====================

Stable releases: only releases a tag that starts with "nc", that are signed and that do not end with -rc0-9 should be considered stable.  All others are experimental.

IMPORTANT VALIDATION CHANGES at block 19200 and 24000!

* Effective at block 19200, merged mining comes into effect
* Also effective at block 19200, timetravel (2015-2016 retarget) hole is closed
* Effective at block 24000, names registered at block 12000 onward will expire after 36000 blocks instead of the current value of 12000
* Also effective at block 24000, the decline in network fees will speed up by a factor of 4

You must upgrade to the latest version nc0.3.24.\* before block 19200 or your client will start rejecting blocks and will be unable to participate in the network.

Technical
=====================

The Bitcoin protocol is augmented with namecoin operations, to reserve, register and update names.  In addition to DNS like entries, arbitrary name/value pairs are allowed and multiple namespaces will be available.  This will include a personal handle namespace mapping handles to public keys and personal address data.

The protocol differences from bitcoin include:

* Different blockchain, port, IRC bootstrap and message header
* New transaction types: new, first-update, update
* Validation on the new transaction types
* RPC calls for managing names
* Network fees to slow down the initial rush

Please read DESIGN-namecoind.md before proceeding.

BUILDING
======================

Building is only supported on Linux for now.  Follow the bitcoin build instructions.  Use "makefile.unix" - it will generate namecoind.  Usage is similar to bitcoind, plus new RPC calls for the new operations.  A GUI is on the roadmap.

RUNNING
======================

You can acquire namecoins in the usual bitcoin way, by mining or by receiving some from others.  After you have acquired some namecoins, use:

`namecoind name_new d/<name>`

This will reserve a name but not make it visible yet.  Make a note of the short hex number.  You will need it for the next step.  (Do not shut down namecoind or you will also have to supply the longer hex code below.)  Wait about 12 blocks, then issue:

`namecoind name_firstupdate d/<name> <rand> <value>`

`<rand>` is the short hex number from the previous step.  This step will make the name visible to all.

after the first update, you can do more updates with:

`namecoind name_update d/<name> <value>`

and transfer to another person:

`namecoind name_update d/<name> <value> <address>`

dump your list of names:

`namecoind name_list`

dump the global list:

`namecoind name_scan`

VALUES
===================

Values for names in the d/ namespace are JSON encoded.  The simplest value is of this form:

  {"map": {"": "10.0.0.1"}}

A [full specification](http://dot-bit.org/Domain_names) is in progress.

DNS conduits
=============

SOCKS5/Tor name resolver: See `ncproxy` in the client sub-directory.

In the near future gzip encoding of the value will be possible, but ncproxy does not support this yet.

ROADMAP
===================

* DNS zone conduit to allow normal DNS server to serve the .bit domain
* Firefox/chrome/... plugins
* GUI
