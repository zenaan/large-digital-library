### TODO (and decide, etc):

  - file hashes: the following hashes should be stored in metadata.yaml for each content file present:
    - SHA3-256
      - see https://en.wikipedia.org/wiki/SHA-3
    - CubeHash(i=16,r=16,b=32,f=32,h=512)
      - see http://cubehash.cr.yp.to/
  - need to decide on how to identify friends, nodes etc - e.g.:
    - global block chain id ?
    - email address (with PGP key for auth) ?
    - some alternative naming system ?
      https://www.afnic.fr/en/resources/publications/issue-papers/alternative-naming-systems-to-the-dns-2.html
    - some sort of tor/i2p/onion/darknet ID ?
    - namecoin ?
    - Tor - does not support IP, nor even UDP! (only TCP)
    - I2P - ?
    - GNUnet https://en.wikipedia.org/wiki/GNUnet
      - GNUnet's GNS is an alternative to DNS "is perhaps the most technically sound"
      - seems like a more generic solution which would benefit other applications with
        any effort put in, 

      - "GNUnet is a free software framework for decentralized, peer-to-peer
        networking and an official GNU package. The framework offers link
        encryption, peer discovery, resource allocation, communication over many
        transports (such as TCP, UDP, HTTP, HTTPS, WLAN and Bluetooth) and various
        basic peer-to-peer algorithms for routing, multicast and network size
        estimation.[4]

        "GNUnet's basic network topology is that of a mesh network. GNUnet includes
        a distributed hash table (DHT) which is a randomized variant of Kademlia
        that can still efficiently route in small-world networks. GNUnet offers a
        "F2F topology" option for restricting connections to only the users'
        trusted friends. The users' friends' own friends (and so on) can then
        indirectly exchange files with the users' computer, never using its IP
        address directly."

      - The summary of features seems to fit the Large Digital Library perfectly.


<!-- vim: tw=90 ts=2 sw=2 expandtab
