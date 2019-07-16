# Certificate Authorities \(CA\) Recognized by Amazon SNS for HTTPS Endpoints<a name="SendMessageToHttp.https.ca"></a>

If you subscribe an HTTPS endpoint to a topic, that endpoint must have a server certificate signed by a trusted Certificate Authority \(CA\)\. Amazon SNS will only deliver messages to HTTPS endpoints that have a signed certificate from a trusted CA that Amazon SNS recognizes\. Amazon SNS recognizes the following CAs\. 

```
mozillacert81.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 07:E0:32:E0:20:B7:2C:3F:19:2F:06:28:A2:59:3A:19:A7:0F:06:9E
    mozillacert99.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): F1:7F:6F:B6:31:DC:99:E3:A3:C8:7F:FE:1C:F1:81:10:88:D9:60:33
    swisssignplatinumg2ca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 56:E0:FA:C0:3B:8F:18:23:55:18:E5:D3:11:CA:E8:C2:43:31:AB:66
    mozillacert145.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 10:1D:FA:3F:D5:0B:CB:BB:9B:B5:60:0C:19:55:A4:1A:F4:73:3A:04
    mozillacert37.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): B1:2E:13:63:45:86:A4:6F:1A:B2:60:68:37:58:2D:C4:AC:FD:94:97
    mozillacert4.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): E3:92:51:2F:0A:CF:F5:05:DF:F6:DE:06:7F:75:37:E1:65:EA:57:4B
    amzninternalitseccag2, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): FA:07:FA:A6:35:D0:BC:98:72:3D:B3:08:8A:CD:CD:CD:3E:23:F9:ED
    mozillacert70.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 78:6A:74:AC:76:AB:14:7F:9C:6A:30:50:BA:9E:A8:7E:FE:9A:CE:3C
    mozillacert88.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): FE:45:65:9B:79:03:5B:98:A1:61:B5:51:2E:AC:DA:58:09:48:22:4D
    mozillacert134.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 70:17:9B:86:8C:00:A4:FA:60:91:52:22:3F:9F:3E:32:BD:E0:05:62
    mozillacert26.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 87:82:C6:C3:04:35:3B:CF:D2:96:92:D2:59:3E:7D:44:D9:34:FF:11
    verisignclass2g2ca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): B3:EA:C4:47:76:C9:C8:1C:EA:F2:9D:95:B6:CC:A0:08:1B:67:EC:9D
    mozillacert77.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 13:2D:0D:45:53:4B:69:97:CD:B2:D5:C3:39:E2:55:76:60:9B:5C:C6
    mozillacert123.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 2A:B6:28:48:5E:78:FB:F3:AD:9E:79:10:DD:6B:DF:99:72:2C:96:E5
    utndatacorpsgcca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 58:11:9F:0E:12:82:87:EA:50:FD:D9:87:45:6F:4F:78:DC:FA:D6:D4
    mozillacert15.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 74:20:74:41:72:9C:DD:92:EC:79:31:D8:23:10:8D:C2:81:92:E2:BB
    digicertglobalrootca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): A8:98:5D:3A:65:E5:E5:C4:B2:D7:D6:6D:40:C6:DD:2F:B1:9C:54:36
    mozillacert66.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): DD:E1:D2:A9:01:80:2E:1D:87:5E:84:B3:80:7E:4B:B1:FD:99:41:34
    mozillacert112.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 43:13:BB:96:F1:D5:86:9B:C1:4E:6A:92:F6:CF:F6:34:69:87:82:37
    utnuserfirstclientauthemailca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): B1:72:B1:A5:6D:95:F9:1F:E5:02:87:E1:4D:37:EA:6A:44:63:76:8A
    verisignc2g1.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 67:82:AA:E0:ED:EE:E2:1A:58:39:D3:C0:CD:14:68:0A:4F:60:14:2A
    mozillacert55.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): AA:DB:BC:22:23:8F:C4:01:A1:27:BB:38:DD:F4:1D:DB:08:9E:F0:12
    mozillacert101.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 99:A6:9B:E6:1A:FE:88:6B:4D:2B:82:00:7C:B8:54:FC:31:7E:15:39
    mozillacert119.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 75:E0:AB:B6:13:85:12:27:1C:04:F8:5F:DD:DE:38:E4:B7:24:2E:FE
    verisignc3g1.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): A1:DB:63:93:91:6F:17:E4:18:55:09:40:04:15:C7:02:40:B0:AE:6B
    mozillacert44.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 5F:43:E5:B1:BF:F8:78:8C:AC:1C:C7:CA:4A:9A:C6:22:2B:CC:34:C6
    mozillacert108.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): B1:BC:96:8B:D4:F4:9D:62:2A:A8:9A:81:F2:15:01:52:A4:1D:82:9C
    mozillacert95.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): DA:FA:F7:FA:66:84:EC:06:8F:14:50:BD:C7:C2:81:A5:BC:A9:64:57
    keynectisrootca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 9C:61:5C:4D:4D:85:10:3A:53:26:C2:4D:BA:EA:E4:A2:D2:D5:CC:97
    mozillacert141.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 31:7A:2A:D0:7F:2B:33:5E:F5:A1:C3:4E:4B:57:E8:B7:D8:F1:FC:A6
    equifaxsecureglobalebusinessca1, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 7E:78:4A:10:1C:82:65:CC:2D:E1:F1:6D:47:B4:40:CA:D9:0A:19:45
    baltimorecodesigningca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 30:46:D8:C8:88:FF:69:30:C3:4A:FC:CD:49:27:08:7C:60:56:7B:0D
    mozillacert33.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): FE:B8:C4:32:DC:F9:76:9A:CE:AE:3D:D8:90:8F:FD:28:86:65:64:7D
    mozillacert0.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 97:81:79:50:D8:1C:96:70:CC:34:D8:09:CF:79:44:31:36:7E:F4:74
    mozillacert84.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): D3:C0:63:F2:19:ED:07:3E:34:AD:5D:75:0B:32:76:29:FF:D5:9A:F2
    mozillacert130.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): E5:DF:74:3C:B6:01:C4:9B:98:43:DC:AB:8C:E8:6A:81:10:9F:E4:8E
    mozillacert148.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 04:83:ED:33:99:AC:36:08:05:87:22:ED:BC:5E:46:00:E3:BE:F9:D7
    mozillacert22.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 32:3C:11:8E:1B:F7:B8:B6:52:54:E2:E2:10:0D:D6:02:90:37:F0:96
    verisignc1g1.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 90:AE:A2:69:85:FF:14:80:4C:43:49:52:EC:E9:60:84:77:AF:55:6F
    mozillacert7.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): AD:7E:1C:28:B0:64:EF:8F:60:03:40:20:14:C3:D0:E3:37:0E:B5:8A
    mozillacert73.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): B5:1C:06:7C:EE:2B:0C:3D:F8:55:AB:2D:92:F4:FE:39:D4:E7:0F:0E
    mozillacert137.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 4A:65:D5:F4:1D:EF:39:B8:B8:90:4A:4A:D3:64:81:33:CF:C7:A1:D1
    swisssignsilverg2ca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 9B:AA:E5:9F:56:EE:21:CB:43:5A:BE:25:93:DF:A7:F0:40:D1:1D:CB
    mozillacert11.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 05:63:B8:63:0D:62:D7:5A:BB:C8:AB:1E:4B:DF:B5:A8:99:B2:4D:43
    mozillacert29.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 74:F8:A3:C3:EF:E7:B3:90:06:4B:83:90:3C:21:64:60:20:E5:DF:CE
    mozillacert62.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): A1:DB:63:93:91:6F:17:E4:18:55:09:40:04:15:C7:02:40:B0:AE:6B
    mozillacert126.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 25:01:90:19:CF:FB:D9:99:1C:B7:68:25:74:8D:94:5F:30:93:95:42
    soneraclass1ca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 07:47:22:01:99:CE:74:B9:7C:B0:3D:79:B2:64:A2:C8:55:E9:33:FF
    mozillacert18.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 79:98:A3:08:E1:4D:65:85:E6:C2:1E:15:3A:71:9F:BA:5A:D3:4A:D9
    mozillacert51.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): FA:B7:EE:36:97:26:62:FB:2D:B0:2A:F6:BF:03:FD:E8:7C:4B:2F:9B
    mozillacert69.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 2F:78:3D:25:52:18:A7:4A:65:39:71:B5:2C:A2:9C:45:15:6F:E9:19
    mozillacert115.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 59:0D:2D:7D:88:4F:40:2E:61:7E:A5:62:32:17:65:CF:17:D8:94:E9
    verisignclass3g5ca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 4E:B6:D5:78:49:9B:1C:CF:5F:58:1E:AD:56:BE:3D:9B:67:44:A5:E5
    utnuserfirsthardwareca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 04:83:ED:33:99:AC:36:08:05:87:22:ED:BC:5E:46:00:E3:BE:F9:D7
    addtrustqualifiedca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 4D:23:78:EC:91:95:39:B5:00:7F:75:8F:03:3B:21:1E:C5:4D:8B:CF
    mozillacert40.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 80:25:EF:F4:6E:70:C8:D4:72:24:65:84:FE:40:3B:8A:8D:6A:DB:F5
    mozillacert58.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 8D:17:84:D5:37:F3:03:7D:EC:70:FE:57:8B:51:9A:99:E6:10:D7:B0
    verisignclass3g3ca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 13:2D:0D:45:53:4B:69:97:CD:B2:D5:C3:39:E2:55:76:60:9B:5C:C6
    mozillacert104.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 4F:99:AA:93:FB:2B:D1:37:26:A1:99:4A:CE:7F:F0:05:F2:93:5D:1E
    mozillacert91.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 3B:C0:38:0B:33:C3:F6:A6:0C:86:15:22:93:D9:DF:F5:4B:81:C0:04
    thawtepersonalfreemailca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): E6:18:83:AE:84:CA:C1:C1:CD:52:AD:E8:E9:25:2B:45:A6:4F:B7:E2
    certplusclass3pprimaryca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 21:6B:2A:29:E6:2A:00:CE:82:01:46:D8:24:41:41:B9:25:11:B2:79
    verisignc3g4.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 22:D5:D8:DF:8F:02:31:D1:8D:F7:9D:B7:CF:8A:2D:64:C9:3F:6C:3A
    swisssigngoldg2ca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): D8:C5:38:8A:B7:30:1B:1B:6E:D4:7A:E6:45:25:3A:6F:9F:1A:27:61
    mozillacert47.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 1B:4B:39:61:26:27:6B:64:91:A2:68:6D:D7:02:43:21:2D:1F:1D:96
    mozillacert80.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): B8:23:6B:00:2F:1D:16:86:53:01:55:6C:11:A4:37:CA:EB:FF:C3:BB
    mozillacert98.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): C9:A8:B9:E7:55:80:5E:58:E3:53:77:A7:25:EB:AF:C3:7B:27:CC:D7
    mozillacert144.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 37:F7:6D:E6:07:7C:90:C5:B1:3E:93:1A:B7:41:10:B4:F2:E4:9A:27
    starfieldclass2ca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): AD:7E:1C:28:B0:64:EF:8F:60:03:40:20:14:C3:D0:E3:37:0E:B5:8A
    mozillacert36.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 23:88:C9:D3:71:CC:9E:96:3D:FF:7D:3C:A7:CE:FC:D6:25:EC:19:0D
    mozillacert3.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 87:9F:4B:EE:05:DF:98:58:3B:E3:60:D6:33:E7:0D:3F:FE:98:71:AF
    globalsignr2ca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 75:E0:AB:B6:13:85:12:27:1C:04:F8:5F:DD:DE:38:E4:B7:24:2E:FE
    mozillacert87.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 5F:3B:8C:F2:F8:10:B3:7D:78:B4:CE:EC:19:19:C3:73:34:B9:C7:74
    mozillacert133.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 85:B5:FF:67:9B:0C:79:96:1F:C8:6E:44:22:00:46:13:DB:17:92:84
    mozillacert25.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 4E:B6:D5:78:49:9B:1C:CF:5F:58:1E:AD:56:BE:3D:9B:67:44:A5:E5
    verisignclass1g2ca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 27:3E:E1:24:57:FD:C4:F9:0C:55:E8:2B:56:16:7F:62:F5:32:E5:47
    mozillacert76.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): F9:B5:B6:32:45:5F:9C:BE:EC:57:5F:80:DC:E9:6E:2C:C7:B2:78:B7
    mozillacert122.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 02:FA:F3:E2:91:43:54:68:60:78:57:69:4D:F5:E4:5B:68:85:18:68
    godaddysecurecertificationauthority, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 7C:46:56:C3:06:1F:7F:4C:0D:67:B3:19:A8:55:F6:0E:BC:11:FC:44
    mozillacert14.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 5F:B7:EE:06:33:E2:59:DB:AD:0C:4C:9A:E6:D3:8F:1A:61:C7:DC:25
    equifaxsecureca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): D2:32:09:AD:23:D3:14:23:21:74:E4:0D:7F:9D:62:13:97:86:63:3A
    mozillacert65.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 69:BD:8C:F4:9C:D3:00:FB:59:2E:17:93:CA:55:6A:F3:EC:AA:35:FB
    mozillacert111.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 9C:BB:48:53:F6:A4:F6:D3:52:A4:E8:32:52:55:60:13:F5:AD:AF:65
    certumtrustednetworkca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 07:E0:32:E0:20:B7:2C:3F:19:2F:06:28:A2:59:3A:19:A7:0F:06:9E
    mozillacert129.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): E6:21:F3:35:43:79:05:9A:4B:68:30:9D:8A:2F:74:22:15:87:EC:79
    mozillacert54.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 03:9E:ED:B8:0B:E7:A0:3C:69:53:89:3B:20:D2:D9:32:3A:4C:2A:FD
    mozillacert100.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 58:E8:AB:B0:36:15:33:FB:80:F7:9B:1B:6D:29:D3:FF:8D:5F:00:F0
    mozillacert118.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 7E:78:4A:10:1C:82:65:CC:2D:E1:F1:6D:47:B4:40:CA:D9:0A:19:45
    mozillacert151.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): AC:ED:5F:65:53:FD:25:CE:01:5F:1F:7A:48:3B:6A:74:9F:61:78:C6
    thawteprimaryrootcag3, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): F1:8B:53:8D:1B:E9:03:B6:A6:F0:56:43:5B:17:15:89:CA:F3:6B:F2
    quovadisrootca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): DE:3F:40:BD:50:93:D3:9B:6C:60:F6:DA:BC:07:62:01:00:89:76:C9
    thawteprimaryrootcag2, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): AA:DB:BC:22:23:8F:C4:01:A1:27:BB:38:DD:F4:1D:DB:08:9E:F0:12
    deprecateditsecca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 12:12:0B:03:0E:15:14:54:F4:DD:B3:F5:DE:13:6E:83:5A:29:72:9D
    entrustrootcag2, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 8C:F4:27:FD:79:0C:3A:D1:66:06:8D:E8:1E:57:EF:BB:93:22:72:D4
    mozillacert43.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): F9:CD:0E:2C:DA:76:24:C1:8F:BD:F0:F0:AB:B6:45:B8:F7:FE:D5:7A
    mozillacert107.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 8E:1C:74:F8:A6:20:B9:E5:8A:F4:61:FA:EC:2B:47:56:51:1A:52:C6
    trustcenterclass4caii, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): A6:9A:91:FD:05:7F:13:6A:42:63:0B:B1:76:0D:2D:51:12:0C:16:50
    mozillacert94.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 49:0A:75:74:DE:87:0A:47:FE:58:EE:F6:C7:6B:EB:C6:0B:12:40:99
    mozillacert140.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): CA:3A:FB:CF:12:40:36:4B:44:B2:16:20:88:80:48:39:19:93:7C:F7
    ttelesecglobalrootclass3ca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 55:A6:72:3E:CB:F2:EC:CD:C3:23:74:70:19:9D:2A:BE:11:E3:81:D1
    amzninternalcorpca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 43:E3:E6:37:C5:88:05:67:91:37:E3:72:4D:01:7F:F4:1B:CE:3A:97
    mozillacert32.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 60:D6:89:74:B5:C2:65:9E:8A:0F:C1:88:7C:88:D2:46:69:1B:18:2C
    mozillacert83.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): A0:73:E5:C5:BD:43:61:0D:86:4C:21:13:0A:85:58:57:CC:9C:EA:46
    verisignroot.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 36:79:CA:35:66:87:72:30:4D:30:A5:FB:87:3B:0F:A7:7B:B7:0D:54
    mozillacert147.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 58:11:9F:0E:12:82:87:EA:50:FD:D9:87:45:6F:4F:78:DC:FA:D6:D4
    camerfirmachambersca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 78:6A:74:AC:76:AB:14:7F:9C:6A:30:50:BA:9E:A8:7E:FE:9A:CE:3C
    mozillacert21.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 9B:AA:E5:9F:56:EE:21:CB:43:5A:BE:25:93:DF:A7:F0:40:D1:1D:CB
    mozillacert39.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): AE:50:83:ED:7C:F4:5C:BC:8F:61:C6:21:FE:68:5D:79:42:21:15:6E
    mozillacert6.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 27:96:BA:E6:3F:18:01:E2:77:26:1B:A0:D7:77:70:02:8F:20:EE:E4
    verisignuniversalrootca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 36:79:CA:35:66:87:72:30:4D:30:A5:FB:87:3B:0F:A7:7B:B7:0D:54
    mozillacert72.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 47:BE:AB:C9:22:EA:E8:0E:78:78:34:62:A7:9F:45:C2:54:FD:E6:8B
    geotrustuniversalca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): E6:21:F3:35:43:79:05:9A:4B:68:30:9D:8A:2F:74:22:15:87:EC:79
    mozillacert136.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): D1:EB:23:A4:6D:17:D6:8F:D9:25:64:C2:F1:F1:60:17:64:D8:E3:49
    mozillacert10.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 5F:3A:FC:0A:8B:64:F6:86:67:34:74:DF:7E:A9:A2:FE:F9:FA:7A:51
    mozillacert28.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 66:31:BF:9E:F7:4F:9E:B6:C9:D5:A6:0C:BA:6A:BE:D1:F7:BD:EF:7B
    mozillacert61.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): E0:B4:32:2E:B2:F6:A5:68:B6:54:53:84:48:18:4A:50:36:87:43:84
    mozillacert79.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): D8:A6:33:2C:E0:03:6F:B1:85:F6:63:4F:7D:6A:06:65:26:32:28:27
    mozillacert125.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): B3:1E:B1:B7:40:E3:6C:84:02:DA:DC:37:D4:4D:F5:D4:67:49:52:F9
    mozillacert17.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 40:54:DA:6F:1C:3F:40:74:AC:ED:0F:EC:CD:DB:79:D1:53:FB:90:1D
    mozillacert50.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 8C:96:BA:EB:DD:2B:07:07:48:EE:30:32:66:A0:F3:98:6E:7C:AE:58
    mozillacert68.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): AE:C5:FB:3F:C8:E1:BF:C4:E5:4F:03:07:5A:9A:E8:00:B7:F7:B6:FA
    mozillacert114.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 51:C6:E7:08:49:06:6E:F3:92:D4:5C:A0:0D:6D:A3:62:8F:C3:52:39
    mozillacert57.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): D6:DA:A8:20:8D:09:D2:15:4D:24:B5:2F:CB:34:6E:B2:58:B2:8A:58
    verisignc2g3.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 61:EF:43:D7:7F:CA:D4:61:51:BC:98:E0:C3:59:12:AF:9F:EB:63:11
    verisignclass2g3ca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 61:EF:43:D7:7F:CA:D4:61:51:BC:98:E0:C3:59:12:AF:9F:EB:63:11
    mozillacert103.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 70:C1:8D:74:B4:28:81:0A:E4:FD:A5:75:D7:01:9F:99:B0:3D:50:74
    mozillacert90.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): F3:73:B3:87:06:5A:28:84:8A:F2:F3:4A:CE:19:2B:DD:C7:8E:9C:AC
    verisignc3g3.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 13:2D:0D:45:53:4B:69:97:CD:B2:D5:C3:39:E2:55:76:60:9B:5C:C6
    mozillacert46.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 40:9D:4B:D9:17:B5:5C:27:B6:9B:64:CB:98:22:44:0D:CD:09:B8:89
    godaddyclass2ca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 27:96:BA:E6:3F:18:01:E2:77:26:1B:A0:D7:77:70:02:8F:20:EE:E4
    verisignc4g3.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): C8:EC:8C:87:92:69:CB:4B:AB:39:E9:8D:7E:57:67:F3:14:95:73:9D
    mozillacert97.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 85:37:1C:A6:E5:50:14:3D:CE:28:03:47:1B:DE:3A:09:E8:F8:77:0F
    mozillacert143.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 36:B1:2B:49:F9:81:9E:D7:4C:9E:BC:38:0F:C6:56:8F:5D:AC:B2:F7
    mozillacert35.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 2A:C8:D5:8B:57:CE:BF:2F:49:AF:F2:FC:76:8F:51:14:62:90:7A:41
    mozillacert2.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 22:D5:D8:DF:8F:02:31:D1:8D:F7:9D:B7:CF:8A:2D:64:C9:3F:6C:3A
    utnuserfirstobjectca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): E1:2D:FB:4B:41:D7:D9:C3:2B:30:51:4B:AC:1D:81:D8:38:5E:2D:46
    mozillacert86.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 74:2C:31:92:E6:07:E4:24:EB:45:49:54:2B:E1:BB:C5:3E:61:74:E2
    mozillacert132.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 39:21:C1:15:C1:5D:0E:CA:5C:CB:5B:C4:F0:7D:21:D8:05:0B:56:6A
    addtrustclass1ca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): CC:AB:0E:A0:4C:23:01:D6:69:7B:DD:37:9F:CD:12:EB:24:E3:94:9D
    mozillacert24.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 59:AF:82:79:91:86:C7:B4:75:07:CB:CF:03:57:46:EB:04:DD:B7:16
    verisignc1g3.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 20:42:85:DC:F7:EB:76:41:95:57:8E:13:6B:D4:B7:D1:E9:8E:46:A5
    mozillacert9.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): F4:8B:11:BF:DE:AB:BE:94:54:20:71:E6:41:DE:6B:BE:88:2B:40:B9
    amzninternalrootca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): A7:B7:F6:15:8A:FF:1E:C8:85:13:38:BC:93:EB:A2:AB:A4:09:EF:06
    mozillacert75.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): D2:32:09:AD:23:D3:14:23:21:74:E4:0D:7F:9D:62:13:97:86:63:3A
    entrustevca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): B3:1E:B1:B7:40:E3:6C:84:02:DA:DC:37:D4:4D:F5:D4:67:49:52:F9
    secomscrootca2, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 5F:3B:8C:F2:F8:10:B3:7D:78:B4:CE:EC:19:19:C3:73:34:B9:C7:74
    camerfirmachambersignca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 4A:BD:EE:EC:95:0D:35:9C:89:AE:C7:52:A1:2C:5B:29:F6:D6:AA:0C
    secomscrootca1, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 36:B1:2B:49:F9:81:9E:D7:4C:9E:BC:38:0F:C6:56:8F:5D:AC:B2:F7
    mozillacert121.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): CC:AB:0E:A0:4C:23:01:D6:69:7B:DD:37:9F:CD:12:EB:24:E3:94:9D
    mozillacert139.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): DE:3F:40:BD:50:93:D3:9B:6C:60:F6:DA:BC:07:62:01:00:89:76:C9
    mozillacert13.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 06:08:3F:59:3F:15:A1:04:A0:69:A4:6B:A9:03:D0:06:B7:97:09:91
    mozillacert64.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 62:7F:8D:78:27:65:63:99:D2:7D:7F:90:44:C9:FE:B3:F3:3E:FA:9A
    mozillacert110.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 93:05:7A:88:15:C6:4F:CE:88:2F:FA:91:16:52:28:78:BC:53:64:17
    mozillacert128.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): A9:E9:78:08:14:37:58:88:F2:05:19:B0:6D:2B:0D:2B:60:16:90:7D
    entrust2048ca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 50:30:06:09:1D:97:D4:F5:AE:39:F7:CB:E7:92:7D:7D:65:2D:34:31
    mozillacert53.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 7F:8A:B0:CF:D0:51:87:6A:66:F3:36:0F:47:C8:8D:8C:D3:35:FC:74
    mozillacert117.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): D4:DE:20:D0:5E:66:FC:53:FE:1A:50:88:2C:78:DB:28:52:CA:E4:74
    mozillacert150.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 33:9B:6B:14:50:24:9B:55:7A:01:87:72:84:D9:E0:2F:C3:D2:D8:E9
    thawteserverca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 9F:AD:91:A6:CE:6A:C6:C5:00:47:C4:4E:C9:D4:A5:0D:92:D8:49:79
    secomvalicertclass1ca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): E5:DF:74:3C:B6:01:C4:9B:98:43:DC:AB:8C:E8:6A:81:10:9F:E4:8E
    mozillacert42.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 85:A4:08:C0:9C:19:3E:5D:51:58:7D:CD:D6:13:30:FD:8C:DE:37:BF
    gtecybertrustglobalca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 97:81:79:50:D8:1C:96:70:CC:34:D8:09:CF:79:44:31:36:7E:F4:74
    mozillacert106.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): E7:A1:90:29:D3:D5:52:DC:0D:0F:C6:92:D3:EA:88:0D:15:2E:1A:6B
    equifaxsecureebusinessca1, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): DA:40:18:8B:91:89:A3:ED:EE:AE:DA:97:FE:2F:9D:F5:B7:D1:8A:41
    mozillacert93.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 31:F1:FD:68:22:63:20:EE:C6:3B:3F:9D:EA:4A:3E:53:7C:7C:39:17
    quovadisrootca3, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 1F:49:14:F7:D8:74:95:1D:DD:AE:02:C0:BE:FD:3A:2D:82:75:51:85
    quovadisrootca2, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): CA:3A:FB:CF:12:40:36:4B:44:B2:16:20:88:80:48:39:19:93:7C:F7
    soneraclass2ca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 37:F7:6D:E6:07:7C:90:C5:B1:3E:93:1A:B7:41:10:B4:F2:E4:9A:27
    mozillacert31.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 9F:74:4E:9F:2B:4D:BA:EC:0F:31:2C:50:B6:56:3B:8E:2D:93:C3:11
    mozillacert49.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 61:57:3A:11:DF:0E:D8:7E:D5:92:65:22:EA:D0:56:D7:44:B3:23:71
    mozillacert82.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 2E:14:DA:EC:28:F0:FA:1E:8E:38:9A:4E:AB:EB:26:C0:0A:D3:83:C3
    mozillacert146.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 21:FC:BD:8E:7F:6C:AF:05:1B:D1:B3:43:EC:A8:E7:61:47:F2:0F:8A
    baltimorecybertrustca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): D4:DE:20:D0:5E:66:FC:53:FE:1A:50:88:2C:78:DB:28:52:CA:E4:74
    mozillacert20.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): D8:C5:38:8A:B7:30:1B:1B:6E:D4:7A:E6:45:25:3A:6F:9F:1A:27:61
    mozillacert38.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): CB:A1:C5:F8:B0:E3:5E:B8:B9:45:12:D3:F9:34:A2:E9:06:10:D3:36
    mozillacert5.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): B8:01:86:D1:EB:9C:86:A5:41:04:CF:30:54:F3:4C:52:B7:E5:58:C6
    mozillacert71.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 4A:BD:EE:EC:95:0D:35:9C:89:AE:C7:52:A1:2C:5B:29:F6:D6:AA:0C
    verisignclass3g4ca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 22:D5:D8:DF:8F:02:31:D1:8D:F7:9D:B7:CF:8A:2D:64:C9:3F:6C:3A
    mozillacert89.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): C8:EC:8C:87:92:69:CB:4B:AB:39:E9:8D:7E:57:67:F3:14:95:73:9D
    mozillacert135.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 62:52:DC:40:F7:11:43:A2:2F:DE:9E:F7:34:8E:06:42:51:B1:81:18
    camerfirmachamberscommerceca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 6E:3A:55:A4:19:0C:19:5C:93:84:3C:C0:DB:72:2E:31:30:61:F0:B1
    mozillacert27.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 3A:44:73:5A:E5:81:90:1F:24:86:61:46:1E:3B:9C:C4:5F:F5:3A:1B
    verisignclass3g2ca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 85:37:1C:A6:E5:50:14:3D:CE:28:03:47:1B:DE:3A:09:E8:F8:77:0F
    mozillacert60.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 3B:C4:9F:48:F8:F3:73:A0:9C:1E:BD:F8:5B:B1:C3:65:C7:D8:11:B3
    mozillacert78.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 29:36:21:02:8B:20:ED:02:F5:66:C5:32:D1:D6:ED:90:9F:45:00:2F
    certumca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 62:52:DC:40:F7:11:43:A2:2F:DE:9E:F7:34:8E:06:42:51:B1:81:18
    deutschetelekomrootca2, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 85:A4:08:C0:9C:19:3E:5D:51:58:7D:CD:D6:13:30:FD:8C:DE:37:BF
    mozillacert124.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 4D:23:78:EC:91:95:39:B5:00:7F:75:8F:03:3B:21:1E:C5:4D:8B:CF
    mozillacert16.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): DA:C9:02:4F:54:D8:F6:DF:94:93:5F:B1:73:26:38:CA:6A:D7:7C:13
    secomevrootca1, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): FE:B8:C4:32:DC:F9:76:9A:CE:AE:3D:D8:90:8F:FD:28:86:65:64:7D
    mozillacert67.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): D6:9B:56:11:48:F0:1C:77:C5:45:78:C1:09:26:DF:5B:85:69:76:AD
    globalsignr3ca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): D6:9B:56:11:48:F0:1C:77:C5:45:78:C1:09:26:DF:5B:85:69:76:AD
    mozillacert113.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 50:30:06:09:1D:97:D4:F5:AE:39:F7:CB:E7:92:7D:7D:65:2D:34:31
    aolrootca2, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 85:B5:FF:67:9B:0C:79:96:1F:C8:6E:44:22:00:46:13:DB:17:92:84
    trustcenteruniversalcai, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 6B:2F:34:AD:89:58:BE:62:FD:B0:6B:5C:CE:BB:9D:D9:4F:4E:39:F3
    aolrootca1, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 39:21:C1:15:C1:5D:0E:CA:5C:CB:5B:C4:F0:7D:21:D8:05:0B:56:6A
    mozillacert56.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): F1:8B:53:8D:1B:E9:03:B6:A6:F0:56:43:5B:17:15:89:CA:F3:6B:F2
    verisignc2g2.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): B3:EA:C4:47:76:C9:C8:1C:EA:F2:9D:95:B6:CC:A0:08:1B:67:EC:9D
    verisignclass1g3ca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 20:42:85:DC:F7:EB:76:41:95:57:8E:13:6B:D4:B7:D1:E9:8E:46:A5
    mozillacert102.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 96:C9:1B:0B:95:B4:10:98:42:FA:D0:D8:22:79:FE:60:FA:B9:16:83
    addtrustexternalca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 02:FA:F3:E2:91:43:54:68:60:78:57:69:4D:F5:E4:5B:68:85:18:68
    verisignclass3ca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): A1:DB:63:93:91:6F:17:E4:18:55:09:40:04:15:C7:02:40:B0:AE:6B
    verisignc3g2.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 85:37:1C:A6:E5:50:14:3D:CE:28:03:47:1B:DE:3A:09:E8:F8:77:0F
    mozillacert45.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 67:65:0D:F1:7E:8E:7E:5B:82:40:A4:F4:56:4B:CF:E2:3D:69:C6:F0
    verisignc4g2.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 0B:77:BE:BB:CB:7A:A2:47:05:DE:CC:0F:BD:6A:02:FC:7A:BD:9B:52
    digicertassuredidrootca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 05:63:B8:63:0D:62:D7:5A:BB:C8:AB:1E:4B:DF:B5:A8:99:B2:4D:43
    verisignclass1ca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): CE:6A:64:A3:09:E4:2F:BB:D9:85:1C:45:3E:64:09:EA:E8:7D:60:F1
    mozillacert109.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): B5:61:EB:EA:A4:DE:E4:25:4B:69:1A:98:A5:57:47:C2:34:C7:D9:71
    thawtepremiumserverca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): E0:AB:05:94:20:72:54:93:05:60:62:02:36:70:F7:CD:2E:FC:66:66
    verisigntsaca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): BE:36:A4:56:2F:B2:EE:05:DB:B3:D3:23:23:AD:F4:45:08:4E:D6:56
    mozillacert96.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 55:A6:72:3E:CB:F2:EC:CD:C3:23:74:70:19:9D:2A:BE:11:E3:81:D1
    mozillacert142.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 1F:49:14:F7:D8:74:95:1D:DD:AE:02:C0:BE:FD:3A:2D:82:75:51:85
    thawteprimaryrootca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 91:C6:D6:EE:3E:8A:C8:63:84:E5:48:C2:99:29:5C:75:6C:81:7B:81
    mozillacert34.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 59:22:A1:E1:5A:EA:16:35:21:F8:98:39:6A:46:46:B0:44:1B:0F:A9
    mozillacert1.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 23:E5:94:94:51:95:F2:41:48:03:B4:D5:64:D2:A3:A3:F5:D8:8B:8C
    mozillacert85.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): CF:9E:87:6D:D3:EB:FC:42:26:97:A3:B5:A3:7A:A0:76:A9:06:23:48
    valicertclass2ca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 31:7A:2A:D0:7F:2B:33:5E:F5:A1:C3:4E:4B:57:E8:B7:D8:F1:FC:A6
    mozillacert131.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 37:9A:19:7B:41:85:45:35:0C:A6:03:69:F3:3C:2E:AF:47:4F:20:79
    mozillacert149.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 6E:3A:55:A4:19:0C:19:5C:93:84:3C:C0:DB:72:2E:31:30:61:F0:B1
    geotrustprimaryca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 32:3C:11:8E:1B:F7:B8:B6:52:54:E2:E2:10:0D:D6:02:90:37:F0:96
    mozillacert23.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 91:C6:D6:EE:3E:8A:C8:63:84:E5:48:C2:99:29:5C:75:6C:81:7B:81
    verisignc1g2.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 27:3E:E1:24:57:FD:C4:F9:0C:55:E8:2B:56:16:7F:62:F5:32:E5:47
    mozillacert8.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 3E:2B:F7:F2:03:1B:96:F3:8C:E6:C4:D8:A8:5D:3E:2D:58:47:6A:0F
    mozillacert74.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 92:5A:8F:8D:2C:6D:04:E0:66:5F:59:6A:FF:22:D8:63:E8:25:6F:3F
    mozillacert120.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): DA:40:18:8B:91:89:A3:ED:EE:AE:DA:97:FE:2F:9D:F5:B7:D1:8A:41
    geotrustglobalca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): DE:28:F4:A4:FF:E5:B9:2F:A3:C5:03:D1:A3:49:A7:F9:96:2A:82:12
    mozillacert138.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): E1:9F:E3:0E:8B:84:60:9E:80:9B:17:0D:72:A8:C5:BA:6E:14:09:BD
    mozillacert12.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): A8:98:5D:3A:65:E5:E5:C4:B2:D7:D6:6D:40:C6:DD:2F:B1:9C:54:36
    comodoaaaca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): D1:EB:23:A4:6D:17:D6:8F:D9:25:64:C2:F1:F1:60:17:64:D8:E3:49
    mozillacert63.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 89:DF:74:FE:5C:F4:0F:4A:80:F9:E3:37:7D:54:DA:91:E1:01:31:8E
    certplusclass2primaryca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 74:20:74:41:72:9C:DD:92:EC:79:31:D8:23:10:8D:C2:81:92:E2:BB
    mozillacert127.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): DE:28:F4:A4:FF:E5:B9:2F:A3:C5:03:D1:A3:49:A7:F9:96:2A:82:12
    ttelesecglobalrootclass2ca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 59:0D:2D:7D:88:4F:40:2E:61:7E:A5:62:32:17:65:CF:17:D8:94:E9
    mozillacert19.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): B4:35:D4:E1:11:9D:1C:66:90:A7:49:EB:B3:94:BD:63:7B:A7:82:B7
    digicerthighassuranceevrootca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 5F:B7:EE:06:33:E2:59:DB:AD:0C:4C:9A:E6:D3:8F:1A:61:C7:DC:25
    mozillacert52.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 8B:AF:4C:9B:1D:F0:2A:92:F7:DA:12:8E:B9:1B:AC:F4:98:60:4B:6F
    mozillacert116.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 2B:B1:F5:3E:55:0C:1D:C5:F1:D4:E6:B7:6A:46:4B:55:06:02:AC:21
    globalsignca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): B1:BC:96:8B:D4:F4:9D:62:2A:A8:9A:81:F2:15:01:52:A4:1D:82:9C
    mozillacert41.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 6B:2F:34:AD:89:58:BE:62:FD:B0:6B:5C:CE:BB:9D:D9:4F:4E:39:F3
    mozillacert59.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 36:79:CA:35:66:87:72:30:4D:30:A5:FB:87:3B:0F:A7:7B:B7:0D:54
    mozillacert105.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 77:47:4F:C6:30:E4:0F:4C:47:64:3F:84:BA:B8:C6:95:4A:8A:41:EC
    trustcenterclass2caii, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): AE:50:83:ED:7C:F4:5C:BC:8F:61:C6:21:FE:68:5D:79:42:21:15:6E
    mozillacert92.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): A3:F1:33:3F:E2:42:BF:CF:C5:D1:4E:8F:39:42:98:40:68:10:D1:A0
    geotrustprimarycag3, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 03:9E:ED:B8:0B:E7:A0:3C:69:53:89:3B:20:D2:D9:32:3A:4C:2A:FD
    entrustsslca, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 99:A6:9B:E6:1A:FE:88:6B:4D:2B:82:00:7C:B8:54:FC:31:7E:15:39
    verisignc3g5.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 4E:B6:D5:78:49:9B:1C:CF:5F:58:1E:AD:56:BE:3D:9B:67:44:A5:E5
    geotrustprimarycag2, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): 8D:17:84:D5:37:F3:03:7D:EC:70:FE:57:8B:51:9A:99:E6:10:D7:B0
    mozillacert30.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): E7:B4:F6:9D:61:EC:90:69:DB:7E:90:A7:40:1A:3C:F4:7D:4F:E8:EE
    mozillacert48.pem, Apr 22, 2014, trustedCertEntry, 
    Certificate fingerprint (SHA1): A0:A1:AB:90:C9:FC:84:7B:3B:12:61:E8:97:7D:5F:D3:22:61:D3:CC
```