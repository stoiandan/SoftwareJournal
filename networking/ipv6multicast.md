In  IPv6 there no longer are any __broadcast__ addresses; however, there are some multi-cast addrees that are somewhat simmilar.
One this is a link-local – more on that late – address: `ff02::1`.

This address is — when sending __udp datagrams__ to it – will __forward__ the data to "All nodes on the local network segment" (says [Wikipedia](https://en.wikipedia.org/wiki/Multicast_address)). There are some limitations here, namely, it's not a bi-directional – two-way – communication; you can send data to all nodes, but not directly reply back; unless you yourslef write to the multicast address.

So you' expect to just use `nc -l ff02::1 12345` for a listener and `nc ff02::1 12345`. But this is exactly the reason why I'm writing this post.
It took me soooo long to find the quarks of it:

First, for both the __listener__ and the __client__ we need to rely on UDP, because these broadcast addresses will not use a connection; it's all connectionless, ergo UDP; so we need to add the `-u` flag.

Next, this is an IPv6 protocl, so, again, for both we will add the `-6` flag.

Finally, usually an address is tight to a specific NIC (network interface). However, for this broadcast address, just the address byitself is ambigious.
If you do an `ip a s` (in Linux) or `ifconfig` on macOS, you'll see you have a bunch of IPv6 addresses:
- one is public (i.e. routable) but temporary – a sort of security/privacy thing.
- another is public – also – but this one is more permanent.
- the third one is more interesting, it's not public – i.e. it's not routable on the internet – but it is on the LAN! It's what is called a link-local address, usuaally ending in, for example,  `%en0` – on macOS/Linux and other UNIX-like systems; and, if I'm not wrong, on Windows it can end with a number `%0`


In anycase, whe need to make use this sufix, so a complete address looks like: `ff02::1%en0` or `ff02::1%lo0`.

So our final outcome is `nc -6 -u -l 12345` for the listener, and `nc -6 -u ff02::1%lo0 12345` for the client.