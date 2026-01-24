
### Example Captured Hashes

**NetNTLMv1 (SSP Enabled):**

`hashcat::admin-SAA37877:85DSBC2CE95161CD00000000000000000000000000000000:892F905962F76D323837F613F88DE27C2BBD6C9ABCD021D0:11223344556677BB`

**NetNTLMv1 (No SSP):**

`hashcat::admin-SAA37877:76365E2D14285612980C67D057EB9EFEEESEF6EB6FF6E04D:727B4E35F947129EA5289CDEDAE869348823EF89F50FC595:1122334455667788`

**NetNTLMv2:**

`admin::N46iSNekpT:08ca45b7d7ea58ee:88dcbe4446168966a153a0064958dac6:...`

---

## Training Sample Hashes

*Provided by PatternForge for community training. These are synthetic hashes - crack them!*

### MD5 (Mode 0)

Fast hash (~60 GH/s). Good for beginners.

```
5f4dcc3b5aa765d61d8327deb882cf99
e10adc3949ba59abbe56e057f20f883e
d8578edf8458ce06fbc5bb76a58c5ca4
0d107d09f5bbe40cade3de5c71e9e9b7
40be4e59b9a2a2b5dffb918c0e86b3d7
d0763edaa9d9bd2a9516280e9044d885
8621ffdbc5698829397d97767ac13db3
eb0a191797624dd3a48fa681d3061212
21232f297a57a5a743894a0e4a801fc3
3bf1114a986ba87ed28fc1b5884fc2f8
```

### SHA-256 (Mode 1400)

Medium speed (~8 GH/s). Common in web apps.

```
5e884898da28047151d0e56f8dc6292773603d0d6aabbdd62a11ef721d1542d8
8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92
8c6976e5b5410415bde908bd4dee15dfb167a9c873fc4bb8a81f6f2ab448a918
65e84be33532fb784c48129675f9eff3a682b27168c0ea744b2cf58ee02337c5
1c8bfe8f801d79745c4631d09fff36c82aa37fc4cce4fc946683d7b336b63032
```

### md5crypt (Mode 500)

Slow hash with salt (~30 MH/s). Unix /etc/shadow format.

```
$1$salt1234$mdTPqHoA0SfnCug0hqR1M0
$1$abcdefgh$hGGndps75hhROKqu/zh9q1
$1$xyz12345$T9p8LqolAwuBV1KjFJm4V1
$1$rounds55$bD1KLq0GgJdK7hPkCnQ8Z.
$1$testsalt$J3K7A2L9wRn.8qL2bZmPU.
```

### NTLMv2 (Mode 5600) - BigLunchCorp Dataset

Challenge-response hashes. 1000 samples available from `biglunchcorp.com` domain.

```
danderson::BIGLUNCHCORP:37311796d9d13a3e:7b1fa9e14d4a916b44301918e07cab54:56afcc2c2a521506
ppatel::BIGLUNCHCORP:58553a48f1a71fda:c59afeefcda24dd59d7a43be0d9c9008:78586ad36fe8600d
robinsonj::BIGLUNCHCORP:fa58654642a97fb3:e80592626da99aebf150782de3d7fffb:dd870aa37147e606
laura_cooper::BIGLUNCHCORP:fb4d5afb31b59331:6950bf4824fb0e852f666d33cca70b10:71fd6a2ff00f2b48
laurad::BIGLUNCHCORP:5e961631fcd511fb:9637d303313ad2448ad3bea10877ec39:a9a0ad46ead91d5c
pbaker::BIGLUNCHCORP:b1df55680c065b78:1a75ceb2ae7c951d10fdcedc408d481c:f05e84ba8da19bdc
christinegarcia::BIGLUNCHCORP:4a89383a88e1e28f:30beb6e21e42ca18df3c6be52fe2d226:7e2f355a83194e27
edwardb::BIGLUNCHCORP:78b4c9feb2a0fb48:3c41e5dc7b65740d7c58023e745a1a47:ed1427cf1d49d777
kylec::BIGLUNCHCORP:e6243200cf2e881a:f084a173025e5af82f3c6780f3efe1f9:6da8b48674004f4e
rachelj::BIGLUNCHCORP:f44e2171bd43cd08:0d0fe853ab7e7599dc2ef92f5e736427:333e5e482ab0efe5
```

> [!tip] Full Dataset
> The complete BigLunchCorp dataset (1000 NTLMv2 hashes) is available in [PatternForge](https://github.com/pitl0rd/PatternForge) at `tests/qa_data/demo_ntlmv2_hashes.txt`

[[Home]]
#education #research #training
