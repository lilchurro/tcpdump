# tcpdump, with packet sanitization flags

[![Build Status](https://travis-ci.org/lilchurro/tcpdump.svg?branch=master)](https://travis-ci.org/lilchurro/tcpdump)

To report bugs and issues, please open a ticket on this project's
Github page.

You can view the original tcpdump project (including the original
README.md file) at https://github.com/the-tcpdump-group/tcpdump .

Tcpdump uses libpcap, a system-independent interface for user-level
packet capture.  Before building tcpdump, you must first retrieve and
build libpcap, also originally from LBL and now being maintained by
tcpdump.org; see http://www.tcpdump.org/ .

Otherwise, to build the project, run the usual commands:

	cd tcpdump
	./configure
	make
	make install

Writing out pcapng files will happen whenever tcpdump gets around to
supporting such a feature; tcpdump currently only reads pcapng but
writes to pcap.

NOTE ON EXECUTION: Our new flags ONLY work when writing to a pcap file;
the flags don't work when printing to the console.


# New flags: packet sanitization

The new flags can be tested against a pcap file like so:

	tcpdump --zero-tcpudp-payload -r [PCAP_INPUT] -w [PCAP_OUTPUT]
	tcpdump --no-tcpudp-payload -r [PCAP_INPUT] -w [PCAP_OUTPUT]


...where the option either zero out payload data after TCP/UDP headers, or
removes it completely. And by the way, these flags work just as well on
live data:

	tcpdump --zero-tcpudp-payload -w [PCAP_OUTPUT]
	tcpdump --no-tcpudp-payload -w [PCAP_OUTPUT]


# Another new flag: IP masking

Another flag of note is the star flag, which masks non-reserved IP
addresses (RFC 1918, broadcasts, and some other reserved netblocks
specified in RFC 5735) with a user-specified IP address. It can be
called like so:

    tcpdump --mask-external-address [REPLACEMENT_IP] -r [PCAP_INPUT] -w [PCAP_OUTPUT]

and similarly on live traffic:

    tcpdump --mask-external-address [REPLACEMENT_IP] -w [PCAP_OUTPUT]

All three new flags only work with IPv4 at the moment.
