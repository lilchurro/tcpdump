# tcpdump, with packet sanitization flags

[![Build Status](https://travis-ci.org/lilchurro/tcpdump.svg?branch=master)](https://travis-ci.org/lilchurro/tcpdump)

To report bugs and issues, please open a ticket on this project's
Github page.

This project currently syncs upstream with tcpdump 4.10.0. You can
view the original project (including the original README.md file)
at https://github.com/the-tcpdump-group/tcpdump .

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

NOTE ON EXECUTION: At the moment, our new flags ONLY work when writing
to a pcap file; the flags don't work when printing to the console, as this
is a separate code path.


# New flags: packet sanitization

The new flags can be tested against a pcap file like so:

	tcpdump -0 -r [PCAP_INPUT] -w [PCAP_OUTPUT]
	tcpdump -00 -r [PCAP_INPUT] -w [PCAP_OUTPUT]


...where `-0` zeroes out payload data after TCP/UDP headers, and `-00`
removes it completely. These flags work just as well on live data:

	tcpdump -0 -w [PCAP_OUTPUT]
	tcpdump -00 -w [PCAP_OUTPUT]


# Another new flag: IP masking

Another flag of note is the star flag, which masks non-reserved IP
addresses (RFC 1918, broadcasts, and some other reserved netblocks
specified in RFC 5735) with a user-specified IP address. It can be
called like so:

    tcpdump -* [REPLACEMENT_IP] -r [PCAP_INPUT] -w [PCAP_OUTPUT]

and similarly on live traffic:

    tcpdump -* [REPLACEMENT_IP] -w [PCAP_OUTPUT]

These new flags currently only work with IPv4.
