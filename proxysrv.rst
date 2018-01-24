Reverse Web Proxy Server
========================

This server will serves as a reverse proxy for limited partner site web communication. It should be placed in demilitarized zone (DMZ).

======== ================================= ====== ====================
Label    CUP                               Memory Disk
======== ================================= ====== ====================
webproxy 1 x 2.3 GHz (6 cores; 12 threads) 16GB   273GB root partition
======== ================================= ====== ====================

DMZ inbound connection is allowed for: HTTPS (443), HTTP (80) -> automatic redirection to HTTPS

DMZ outbound connections is allowed for: 8080, 9000