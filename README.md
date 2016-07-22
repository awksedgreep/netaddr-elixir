netaddr-elixir
=========
[![Build Status](https://travis-ci.org/jonnystorm/netaddr-elixir.svg?branch=master)](https://travis-ci.org/jonnystorm/netaddr-elixir)

General functions for network address parsing and manipulation.

### Parsing:

```
iex> NetAddr.ipv4_cidr "192.0.2.1/24"
%NetAddr.IPv4{length: 24, network: <<192, 0, 2, 1>>}

iex> NetAddr.ipv4 "192.0.2.1"
%NetAddr.IPv4{length: 32, network: <<192, 0, 2, 1>>}


iex> NetAddr.ipv6_cidr "fe80:0:c100::c401/64"
%NetAddr.IPv6{length: 64, network: <<254, 128, 0, 0, 193, 0, 0, 0, 0, 0, 0, 0, 0, 0, 196, 1>>}

iex> NetAddr.ipv6 "fe80:0:c100::c401"
%NetAddr.IPv6{length: 128, network: <<254, 128, 0, 0, 193, 0, 0, 0, 0, 0, 0, 0, 0, 0, 196, 1>>}
```

### Pretty-printing:

```
iex> "#{NetAddr.ipv4_cidr("192.0.2.1/24")}"
"192.0.2.1/24"

iex> "#{NetAddr.ipv6("fe80:0:c100::c401")}"
"fe80:0:c100::c401/128"
```

### Conversion:

```
iex> NetAddr.ipv4_cidr("192.0.2.1/24") |> NetAddr.network
"192.0.2.0"

iex> NetAddr.ipv4_cidr("192.0.2.1/24") |> NetAddr.address_length(22) |> NetAddr.network
"192.0.0.0"


iex> NetAddr.aton <<192,0,2,1>>
3221225985

iex> NetAddr.ntoa 3221225985, 4
<<192, 0, 2, 1>>


iex> NetAddr.aton <<254, 128, 0, 0, 193, 0, 0, 0, 0, 0, 0, 0, 0, 0, 196, 1>>
338288524986991696549538495105230488577

iex> NetAddr.ntoa 338288524986991696549538495105230488577, 16
<<254, 128, 0, 0, 193, 0, 0, 0, 0, 0, 0, 0, 0, 0, 196, 1>>


iex> NetAddr.length_to_mask(30, 4)
<<255, 255, 255, 252>>

iex> NetAddr.length_to_mask(64, 16)
<<255, 255, 255, 255, 255, 255, 255, 255, 0, 0, 0, 0, 0, 0, 0, 0>>


iex> NetAddr.ipv4_cidr("198.51.100.0/24") |> NetAddr.netaddr_to_range
3325256704..3325256959

iex> NetAddr.range_to_netaddr 3325256704..3325256959, 4
%NetAddr.IPv4{address: <<198, 51, 100, 0>>, length: 24}
```

### Arbitrary length addresses:

```
iex> NetAddr.netaddr(<<1, 2, 3, 4, 5, 6>>)
%NetAddr.MAC_48{length: 48, network: <<1, 2, 3, 4, 5, 6>>}

iex> NetAddr.netaddr(<<1, 2, 3, 4, 5>>, 48, 6)
%NetAddr.Generic{length: 48, network: <<0, 1, 2, 3, 4, 5>>}

iex> NetAddr.netaddr(<<1, 2, 3, 4, 5>>)
%NetAddr.Generic{length: 40, network: <<1, 2, 3, 4, 5>>}

iex> "#{NetAddr.netaddr(<<1, 2, 3, 4, 5>>)}"
"0x0102030405/40"

iex> NetAddr.aton(<<1,2,3,4,5>>)
4328719365

iex> NetAddr.ntoa 4328719365, 5
<<1, 2, 3, 4, 5>>

iex> NetAddr.length_to_mask(37, 6)
<<255, 255, 255, 255, 248, 0>>
```
