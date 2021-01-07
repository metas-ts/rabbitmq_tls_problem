

Note that all this originally comes from https://groups.google.com/g/rabbitmq-users/c/qFJBrg8PrYM/m/uwkkPYFHBAAJ
Thx to all who prepared this test case so far!




```bash
mkdir tls
cd tls
git clone https://github.com/michaelklishin/tls-gen tls-gen
cd tls-gen/basic
# private key password
make PASSWORD=bunnies
make verify
make info
ls -l ./result
```

# Go back and copy the certs to their proper place

```bash
cd ../../../
cp ./tls/tls-gen/basic/result/* ./volumes/certs
```


## TESTING with OpenSSL (<= works for me)


Note that my hostname is `tobi-ThinkPad-E490`

```bash
# go to the certs folder
cd ./volumes/certs
```

Start a listener: 

```bash
openssl s_server -accept 5671 -cert server_certificate.pem -key server_key.pem -CAfile ca_certificate.pem
```

Try and connect: 

```bash
openssl s_client -connect tobi-ThinkPad-E490:5671  -cert client_certificate.pem -key client_key.pem -CAfile ca_certificate.pem -verify 8 -verify_hostname tobi-ThinkPad-E490
```

<details>
<summary>this is the output:</summary>

```bash
verify depth is 8
Enter pass phrase for client_key.pem:
CONNECTED(00000003)
Can't use SSL_get_servername
depth=1 CN = TLSGenSelfSignedtRootCA, L = $$$$
verify return:1
depth=0 CN = tobi-ThinkPad-E490, O = server
verify return:1
---
Certificate chain
 0 s:CN = tobi-ThinkPad-E490, O = server
   i:CN = TLSGenSelfSignedtRootCA, L = $$$$
 1 s:CN = TLSGenSelfSignedtRootCA, L = $$$$
   i:CN = TLSGenSelfSignedtRootCA, L = $$$$
---
Server certificate
-----BEGIN CERTIFICATE-----
MIIDiTCCAnGgAwIBAgIBATANBgkqhkiG9w0BAQsFADAxMSAwHgYDVQQDDBdUTFNH
ZW5TZWxmU2lnbmVkdFJvb3RDQTENMAsGA1UEBwwEJCQkJDAeFw0yMTAxMDcyMTU5
MTVaFw0zMTAxMDUyMTU5MTVaMC4xGzAZBgNVBAMMEnRvYmktVGhpbmtQYWQtRTQ5
MDEPMA0GA1UECgwGc2VydmVyMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKC
AQEAnk2zBsU6ZssjiM3V2/qgxIwSlzAu+x1i3bOHQWxveOrTL1Ob1iXJ0WwJVnza
qkMnYDrZghehoVWl1HdS/nzH4+K3AFSNmUiMOS//CZdugip9JjGzgKf8y3Z0A6rr
AstYKOnowsepaa2y8YxpAL9CbOFcKeIKHeaAJVB3F6sD1vx0lBd+U31xv7pbJBf9
fhY4R6aZ05C8z8Bf6Icyk6Hp1mLWezgx3uEXV1NKe66fdAO/cZaLL4xWSJK3Tuzh
Ob41ZbAZ5FOwPmm6SiUpwrn+i2yfBDQSNEDFtury5GK3DMZ/11VZAT+q8pPTVoXj
1vfHHPV/qonVE0D6+FzszqxuwQIDAQABo4GuMIGrMAkGA1UdEwQCMAAwCwYDVR0P
BAQDAgWgMBMGA1UdJQQMMAoGCCsGAQUFBwMBMDwGA1UdEQQ1MDOCEnRvYmktVGhp
bmtQYWQtRTQ5MIISdG9iaS1UaGlua1BhZC1FNDkwgglsb2NhbGhvc3QwHQYDVR0O
BBYEFH4cAA2TiFwqxD8Z5wmfNl5SLHlIMB8GA1UdIwQYMBaAFKi3g5KXg3C0XBPn
l9l+j79mo9woMA0GCSqGSIb3DQEBCwUAA4IBAQAulDzcZBCLixSc5m8PnLe2Y+1Y
tvAjD6nTHnBIkJmW6Knme3nAUywAgpMyVblhuvtdO0fXKVUUnY/LoCkdkud4NHeR
GWyw2glyKQxLrET3kVGsqpJkexA48O/hTBe5jAG7pIycpO6c7BxSEccf0lSfOBC9
Ok/EecnoOy/YKhQqLU6N328aqWF8Jdw1y/vbUO1B5vNNKn1DgdUDl8b2JbSUaBGx
OQyUVTNcgKqyBEXFQ5RFVu40cgzGu6bdVZZg1RCNmx1wMZQ1qWFmD3f3e93pgZQn
7J1an3V0qrl1c1lhzTvhLMz5RAn+r+Q9t9EOOFhUkaAx2crCG3cE1sOwO4dX
-----END CERTIFICATE-----
subject=CN = tobi-ThinkPad-E490, O = server

issuer=CN = TLSGenSelfSignedtRootCA, L = $$$$

---
No client certificate CA names sent
Peer signing digest: SHA256
Peer signature type: RSA-PSS
Server Temp Key: X25519, 253 bits
---
SSL handshake has read 2322 bytes and written 363 bytes
Verification: OK
Verified peername: tobi-ThinkPad-E490
---
New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384
Server public key is 2048 bit
Secure Renegotiation IS NOT supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
Early data was not sent
Verify return code: 0 (ok)
---
---
Post-Handshake New Session Ticket arrived:
SSL-Session:
    Protocol  : TLSv1.3
    Cipher    : TLS_AES_256_GCM_SHA384
    Session-ID: 9DF768AA61A2555B78FBC967C2B3A172A58BA16591CBA076B34E231D639DB0A0
    Session-ID-ctx: 
    Resumption PSK: 139B57A238883BCF52401230203E2EEE00EFC778565666750ED1D17BFE68C0838727DE521BA785B085536018A53634F2
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 7200 (seconds)
    TLS session ticket:
    0000 - 11 81 dc d4 c7 3b ec 98-51 dc 44 df 08 82 18 5a   .....;..Q.D....Z
    0010 - 06 ec fa 82 e5 11 f8 e3-4e 24 81 76 7a 41 22 42   ........N$.vzA"B
    0020 - dc 55 19 e3 ce bd da 96-8b 3d 0e b1 2d 65 99 38   .U.......=..-e.8
    0030 - a5 76 5b f6 65 c0 6d 46-c1 f6 8a fb 0e 68 8c d2   .v[.e.mF.....h..
    0040 - 05 d1 da 8f ea 47 88 2e-7d 9b 86 8b 93 eb 35 b7   .....G..}.....5.
    0050 - b0 cb 8a b1 7b 25 65 32-55 c8 72 16 2f f4 62 23   ....{%e2U.r./.b#
    0060 - 58 64 09 a1 41 b1 00 f3-0a 0b ce b5 ea f4 1e 1e   Xd..A...........
    0070 - 11 4a 26 8d d8 af 2f d1-74 97 6c aa 85 a2 e4 a8   .J&.../.t.l.....
    0080 - df 54 91 7f 3f da 46 14-c2 4b 6e dc 33 c4 b9 b0   .T..?.F..Kn.3...
    0090 - 3f 3f 03 ef dc 42 54 ca-e0 a7 22 8f 79 fb 9e 43   ??...BT...".y..C
    00a0 - 9d ac db e9 b5 28 d3 e8-37 ee ff e4 62 dd 22 03   .....(..7...b.".
    00b0 - 4e b1 88 a0 78 2a 82 84-45 f1 d3 94 ee f0 c6 04   N...x*..E.......

    Start Time: 1610060572
    Timeout   : 7200 (sec)
    Verify return code: 0 (ok)
    Extended master secret: no
    Max Early Data: 0
---
read R BLOCK
---
Post-Handshake New Session Ticket arrived:
SSL-Session:
    Protocol  : TLSv1.3
    Cipher    : TLS_AES_256_GCM_SHA384
    Session-ID: 83AFE70ED9DAA4CACA8E9B3C6F4C5A36766BDEDF51A9A53769049058E05323F0
    Session-ID-ctx: 
    Resumption PSK: 778A522E3044B30D805D1BC7E8C97887509D2D22456161EE8AFA327E5492306ACD9D2F8F8B30C8FBA952C57AC2F6C295
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 7200 (seconds)
    TLS session ticket:
    0000 - 11 81 dc d4 c7 3b ec 98-51 dc 44 df 08 82 18 5a   .....;..Q.D....Z
    0010 - 2e c0 ca 7a c7 2c 7e e1-96 67 86 94 b8 b3 f3 62   ...z.,~..g.....b
    0020 - da 52 b3 48 9b d7 bf 01-f2 f3 d9 eb e5 f8 18 ad   .R.H............
    0030 - 0c d5 3d 51 cb 84 29 2a-6b de 5d 14 e1 ac 87 de   ..=Q..)*k.].....
    0040 - 31 30 43 36 dc 45 fc 71-77 80 87 b1 43 9c 48 c3   10C6.E.qw...C.H.
    0050 - 97 14 10 71 82 6c 75 73-0e 67 30 ae 20 a1 f9 41   ...q.lus.g0. ..A
    0060 - bd 08 39 77 73 a2 e4 8e-24 97 2e df 39 88 1f 7c   ..9ws...$...9..|
    0070 - ce 6d e0 76 b0 73 4f a4-32 ba 30 1c 02 11 f4 ef   .m.v.sO.2.0.....
    0080 - ba 6d 2f 4c 43 5b 01 14-3e 68 33 b5 d8 8b f6 7e   .m/LC[..>h3....~
    0090 - de 19 c6 57 6d 4d df 61-48 2a db 13 ec 12 23 34   ...WmM.aH*....#4
    00a0 - 3e 30 67 77 19 c5 e4 31-f4 8f 78 7d 01 36 07 f7   >0gw...1..x}.6..
    00b0 - 9e 17 bf 94 8f fb 60 43-77 9d a0 17 ce c4 4b af   ......`Cw.....K.
    00c0 - c6 18 7f 0a 2f e6 3b 5d-d7 38 59 96 f2 05 3d 17   ..../.;].8Y...=.

    Start Time: 1610060572
    Timeout   : 7200 (sec)
    Verify return code: 0 (ok)
    Extended master secret: no
    Max Early Data: 0
---
read R BLOCK
```

</details>


## TESTING with RabbitMQ & OpenSSL (<= does not work for me)



### Run docker-compose to start RabbitMQ

```bash
# assuming that we are still in the certs folder..
docker-compose -f ../../docker-compose.yml up --build -d
```

### Try to connect with OpenSSL

```bash
openssl s_client -connect tobi-ThinkPad-E490:5671  -cert client_certificate.pem -key client_key.pem -CAfile ca_certificate.pem -verify 8 -verify_hostname tobi-ThinkPad-E490
```

I get the following output (and also there's no activity in the RabbitMQ logs)

<details>
<summary>this is the output:</summary>

```bash
verify depth is 8
Enter pass phrase for client_key.pem:
CONNECTED(00000003)
write:errno=0
---
no peer certificate available
---
No client certificate CA names sent
---
SSL handshake has read 0 bytes and written 283 bytes
Verification: OK
---
New, (NONE), Cipher is (NONE)
Secure Renegotiation IS NOT supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
Early data was not sent
Verify return code: 0 (ok)
---
```

</details>
