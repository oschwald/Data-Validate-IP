# NAME

Data::Validate::IP - IPv4 and IPv6 validation methods

# VERSION

version 0.24

# SYNOPSIS

    use Data::Validate::IP qw(is_ipv4 is_ipv6);

    my $suspect = '1.2.3.4';
    if (is_ipv4($suspect)) {
        print "Looks like an IPv4 address";
    }
    else {
        print "Not an IPv4 address\n";
    }

    $suspect = '::1234';
    if (is_ipv6($suspect)) {
        print "Looks like an IPv6 address";
    }
    else {
        print "Not an IPv6 address\n";
    }

# DESCRIPTION

This module provides a number IP address validation subs that both validate
and untaint their input. This includes both basic validation (`is_ipv4()` and
`is_ipv6()`) and special cases like checking whether an address belongs to a
specific network or whether an address is public or private (reserved).

# FUNCTIONS

All of the functions below are exported by default.

All functions return an untainted value if the test passes and undef if it
fails. In theory, this means that you should always check for a defined status
explicitly but in practice there are no valid IP addresses where the string
form evaluates to false in Perl.

Note that none of these functions actually attempt to test whether the given
IP address is routable from your device; they are purely semantic checks.

## is\_ipv4($ip) and is\_ipv6($ip)

These functions simply check whether the address is a valid IPv4 or IPv6 address.

## is\_innet\_ipv4($ip, $network)

This subroutine checks whether the address belongs to the given IPv4
network. The `$network` argument can either be a string in CIDR notation like
"15.0.15.0/24" or a [NetAddr::IP](https://metacpan.org/pod/NetAddr::IP) object.

This subroutine used to accept many more forms of network specifications
(anything [Net::Netmask](https://metacpan.org/pod/Net::Netmask) accepts) but this has been deprecated.

## is\_unroutable\_ipv4($ip)

This subroutine checks whether the address belongs to any of several special
use IPv4 networks - `0.0.0.0/8`, `100.64.0.0/10`, `192.0.0.0/29`,
`198.18.0.0/15`, `240.0.0.0/4` - as defined by [RFC
5735](http://tools.ietf.org/html/rfc5735), [RFC
6333](http://tools.ietf.org/html/rfc6333), and [RFC
6958](http://tools.ietf.org/html/rfc6598).

Arguably, these should be broken down further but this subroutine will always
exist for backwards compatibility.

## is\_private\_ipv4($ip)

This subroutine checks whether the address belongs to any of the private IPv4
networks - `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16` - as defined by
[RFC 5735](http://tools.ietf.org/html/rfc5735).

## is\_loopback\_ipv4($ip)

This subroutine checks whether the address belongs to the IPv4 loopback
network - `127.0.0.0/8` - as defined by [RFC
5735](http://tools.ietf.org/html/rfc5735).

## is\_linklocal\_ipv4($ip)

This subroutine checks whether the address belongs to the IPv4 link local
network - `169.254.0.0/16` - as defined by [RFC
5735](http://tools.ietf.org/html/rfc5735).

## is\_testnet\_ipv4($ip)

This subroutine checks whether the address belongs to any of the IPv4 TEST-NET
networks for use in documentation and example code - `192.0.2.0/24`,
`198.51.100.0/24`, and `203.0.113.0/24` - as defined by [RFC
5735](http://tools.ietf.org/html/rfc5735).

## is\_anycast\_ipv4($ip)

This subroutine checks whether the address belongs to the 6to4 relay anycast
network - `192.88.99.0/24` - as defined by [RFC
5735](http://tools.ietf.org/html/rfc5735).

## is\_multicast\_ipv4($ip)

This subroutine checks whether the address belongs to the IPv4 multicast
network - `224.0.0.0/4` - as defined by [RFC
5735](http://tools.ietf.org/html/rfc5735).

## is\_loopback\_ipv6($ip)

This subroutine checks whether the address is the IPv6 loopback address -
`::1/128` - as defined by [RFC 4291](http://tools.ietf.org/html/rfc4291).

## is\_ipv4\_mapped\_ipv6($ip)

This subroutine checks whether the address belongs to the IPv6 IPv4-mapped
address network - `::ffff:0:0/96` - as defined by [RFC
4291](http://tools.ietf.org/html/rfc4291).

## is\_discard\_ipv6($ip)

This subroutine checks whether the address belongs to the IPv6 discard prefix
network - `100::/64` - as defined by [RFC
6666](http://tools.ietf.org/html/rfc6666).

## is\_special\_ipv6($ip)

This subroutine checks whether the address belongs to the IPv6 special network
\- `2001::/23` - as defined by [RFC 2928](http://tools.ietf.org/html/rfc2928).

## is\_teredo\_ipv6($ip)

This subroutine checks whether the address belongs to the IPv6 TEREDO network
\- `2001::/32` - as defined by [RFC 4380](http://tools.ietf.org/html/rfc4380).

Note that this network is a subnet of the larger special network at
`2001::/23`.

## is\_orchid\_ipv6($ip)

This subroutine checks whether the address belongs to the IPv6 ORCHID network
\- `2001::/32` - as defined by [RFC 4380](http://tools.ietf.org/html/rfc4380).

Note that this network is a subnet of the larger special network at
`2001::/23`.

This network is currently scheduled to be returned to the special pool in
March of 2014 unless the IETF extends its use. If that happens this subroutine
will continue to exist but will always return false.

## is\_documentation\_ipv6($ip)

This subroutine checks whether the address belongs to the IPv6 documentation
network - `2001:DB8::/32` - as defined by [RFC
3849](http://tools.ietf.org/html/rfc3849).

## is\_private\_ipv6($ip)

This subroutine checks whether the address belongs to the IPv6 private network
\- `FC00::/7` - as defined by [RFC 4193](http://tools.ietf.org/html/rfc4193).

## is\_linklocal\_ipv6($ip)

This subroutine checks whether the address belongs to the IPv6 link-local
unicast network - `FE80::/10` - as defined by [RFC
4291](http://tools.ietf.org/html/rfc4291).

## is\_multicast\_ipv6($ip)

This subroutine checks whether the address belongs to the IPv6 multicast
network - `FF00::/8` - as defined by [RFC
4291](http://tools.ietf.org/html/rfc4291).

## is\_public\_ipv4($ip) and is\_public\_ipv6($ip)

These subroutines check whether the given IP address belongs to any of the
special case networks defined previously. Note that this is **not** simply the
opposite of checking `is_private_ipv4()` or `is_private_ipv6()`. The private
networks are a subset of all the special case networks.

# OBJECT-ORIENTED INTERFACE

This module can also be used as a class. You can call `Data::Validate::IP->new()` to get an object and then call any of the
validation subroutines as methods on that object. This is somewhat pointless
since the object will never contain any state but this interface is kept for
backwards compatibility.

# SEE ALSO

IPv4

**\[RFC 5735\] \[RFC 1918\]**

IPv6

**\[RFC 2460\] \[RFC 4193\] \[RFC 4291\] \[RFC 6434\]**

# BUGS

Please report any bugs or feature requests to
`bug-data-validate-ip@rt.cpan.org`, or through the web interface at
[http://rt.cpan.org](http://rt.cpan.org). I will be notified, and then you'll automatically be
notified of progress on your bug as I make changes.

# ACKNOWLEDGEMENTS

Thanks to Richard Sonnen <`sonnen@richardsonnen.com`> for writing the
Data::Validate module.

Thanks to Matt Dainty <`matt@bodgit-n-scarper.com`> for adding the
`is_multicast_ipv4()` and `is_linklocal_ipv4()` code.

# AUTHORS

- Neil Neely <neil@neely.cx>
- Dave Rolsky <autarch@urth.org>

# COPYRIGHT AND LICENSE

This software is copyright (c) 2014 by Neil Neely.

This is free software; you can redistribute it and/or modify it under
the same terms as the Perl 5 programming language system itself.
