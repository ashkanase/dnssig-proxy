dnssig-proxy
============

A proxy for DNSSIG.

DNSSIG is a simple and efficient way for authenticating responses sent
by an upstream DNS resolver to a client.

It has notable advantages over DNSCrypt and DNS-over-TLS:

- interoperability: it has been designed from the ground up to cope
with broken routers, firewalls and resolvers. DNSSIG-enhanced packets
look like regular DNS packets.

- cacheability: DNSSIG-enhanced packets can be cached by local and
intermediate resolvers.

- simplicity: it is simple to implement, even just by using the API
provided by some resolvers.

- fast: DNSSIG uses the Ed25519 public-key signature system.

- small: the overhead over unsigned queries is minimal.

Q: Cut the crap, how does it work?

DNSSIG is based on SIG(0). Responses are augmented with two records,
that will eventually be merged into one.
The first one is to provide optional support for sessions and to
address replay attacks in a different way than SIG(0). The second one
is a SIG record.

Q: How does a client get the resolver public key?

The client retrieves binary certificates through a TXT query. The
zone has to be DNSSEC signed or a trust anchor can be manually
specified. The DNSSEC validation will be done by the client (thanks to
ldns).

Q: So, it only checks that what the resolver sent hasn't been tampered
with, not that the resolver didn't send wrong records, right?

Correct. This is not a replacement for DNSSEC. The purpose is to sign the
"last mile". For unsigned zones, this is better than nothing.
