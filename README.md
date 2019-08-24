# hieradata
"[**_Hiera_**](https://puppet.com/docs/puppet/latest/hiera.html) is a built-in key-value configuration data lookup system, used for separating data from Puppet code." <br>

This repository is an example of a basic Hiera configuration and the associated data stores. Encrypted data stores use the `.eyaml` extension. The private keys were intentionally excluded so, if you want to demo Hiera, clone my repo, generate your own keys with `eyaml createkeys` and have fun. It's easy, read on!

## Table of Contents
**[Installation](#installation)**<br>
**[Encryption Keys](#encryption-keys)**<br>
**[Requirements](#requirements)**<br>
**[Usage](#usage)**<br>
**[References](#references)**<br>
**[Additional Plugins](#additional-plugins)**<br>

### Installation
1. On the Puppet `[master]`, use the following command to deploy this repository:<br>
`sudo git clone git@github.com:keysersoze206/hieradata.git /etc/puppetlabs/puppet/hieradata`
2. Run: `sudo chown -R pe-puppet:pe-puppet /etc/puppetlabs/puppet/hieradata`.
3. Add `hiera_config = $confdir/hieradata/hiera.yaml` to `$confdir/puppet.conf`.
4. Restart the `pe-puppetserver` service.

### Encryption Keys
Store the private and public key(s) in a safe place. You only need the public key to generate a new, encrypted hiera value from your Devloper system so you should not deploy the private key(s) to any system other than the Puppet `[master]`.

Encryption keys, on the Puppet `[master]`, must be owned by the `pe-puppet` user.

See [hiera.yaml](https://github.com/keysersoze206/hieradata/blob/master/hiera.yaml) for the location to the key(s) on the Puppet `[master]`.<br>
See [Usage](#usage) section for more information.

### Requirements
- [puppet](https://github.com/puppetlabs/puppetserver)
- [hiera](https://puppet.com/docs/puppet/latest/hiera_intro.html)
- [hiera-eyaml](https://github.com/voxpupuli/hiera-eyaml)
  + [Installed on Puppet `[master]`](https://github.com/voxpupuli/hiera-eyaml#installing-hiera-eyaml-for-the-new-puppet-server)
  + [Installed on Developer's system](https://github.com/voxpupuli/hiera-eyaml#installing-hiera-eyaml)

### Usage
`eyaml createkeys` -- Create new encryption keys
```bash
$ eyaml createkeys
[hiera-eyaml-core] Created key directory: ./keys
[hiera-eyaml-core] Keys created OK
```
`eyaml encrypt -s <STRING>` -- Encrypt a string
```bash
$ eyaml encrypt -s 'ASuperSecretStringHere'
string: ENC[PKCS7,MIIBiQYJKoZIhvcNAQcDoIIBejCCAXYCAQAxggEhMIIBHQIBADAFMAACAQEwDQYJKoZIhvcNAQEBBQAEggEAipzyFjL53OQ3Af6pJdu4c7S3JJ54kDLY53q2GIyJqB16xYOPAOY3Rjfs74wJEtstHo0eofp3zPsm2ub47tNuPFO0W1jLHidhJr6h5BQFWjfOF3j8v67O1Ave/b0TYEKmPGOUGAiTru75A8j5ceEqqkpnRHferLCNFwrsXVkqg69Ums5Ox4HxjuXJoNpvUyeEP+lqteAwzUTtjs/nOlC1N42vg2L0dT/A0/IAM9E/AJyfdIlT+PKeeAIiSVmDN8CiYDDfGDZfDVdnRB2/JdR4TrU+PmIGtS1PfCD2nN/QPKW8zNWrskAzgitfPx/W3AHejWW5I5xDm7/85NIB3ntnrTBMBgkqhkiG9w0BBwEwHQYJYIZIAWUDBAEqBBBTfWLgFarbvVqOo5H7XPXogCB1WHNetriskG8qqHuBF8SpP2Xcxuy5dfOz/ORpC02VXw==]

OR

block: >
    ENC[PKCS7,MIIBiQYJKoZIhvcNAQcDoIIBejCCAXYCAQAxggEhMIIBHQIBADAFMAACAQEw
    DQYJKoZIhvcNAQEBBQAEggEAipzyFjL53OQ3Af6pJdu4c7S3JJ54kDLY53q2
    GIyJqB16xYOPAOY3Rjfs74wJEtstHo0eofp3zPsm2ub47tNuPFO0W1jLHidh
    Jr6h5BQFWjfOF3j8v67O1Ave/b0TYEKmPGOUGAiTru75A8j5ceEqqkpnRHfe
    rLCNFwrsXVkqg69Ums5Ox4HxjuXJoNpvUyeEP+lqteAwzUTtjs/nOlC1N42v
    g2L0dT/A0/IAM9E/AJyfdIlT+PKeeAIiSVmDN8CiYDDfGDZfDVdnRB2/JdR4
    TrU+PmIGtS1PfCD2nN/QPKW8zNWrskAzgitfPx/W3AHejWW5I5xDm7/85NIB
    3ntnrTBMBgkqhkiG9w0BBwEwHQYJYIZIAWUDBAEqBBBTfWLgFarbvVqOo5H7
    XPXogCB1WHNetriskG8qqHuBF8SpP2Xcxuy5dfOz/ORpC02VXw==]
```

`eyaml encrypt -f <FILE>` -- Encrypt a file
```bash
$ eyaml encrypt -f ~/.ssh/id_rsa.pub
string: ENC[PKCS7,MIIDDQYJKoZIhvcNAQcDoIIC/jCCAvoCAQAxggEhMIIBHQIBADAFMAACAQEwDQYJKoZIhvcNAQEBBQAEggEANj8EexRpT58/lF0vnz9YoIzCBo2FlVQpO9tOWT7/4Zr0Mu+8MCvwlPL4KuyYpphCZCGggc5mDMVRjFqDNQoucW5UmQoDd/fXCojzUhkVmbq+XXa5fNi/woBBTuoKIeLs9DeGAKUvfmAIQ2jih/eCw/8zvs4x27MAC98eKbYNjbJ9ZunSxFw14WnotZGm+3DdG+SuMq0stGnPIsRPNXYTCJietlquVBXX+0xEUp5YB+m0sf1kS8c1OfHgVsuTiUYSflcev6K5qGNASyaEezw9JPl7qOLiImvPRMPFDaxw5uj0QjWOalPaxNZccAqJgtXJFHXHluBiwCkOrxgZ+KS4VzCCAc4GCSqGSIb3DQEHATAdBglghkgBZQMEASoEEHGFAmyRi3jdkz0KpCP/CfqAggGgrlYIoT9A47445rUFpX9+VTbr1c75nQxOwxr262tJ2ZIFlbyiEbQxHQ6Xhx7c1LuyKLPAOYFRCYa0oJgu5eaqNomYQ7JMFG/0yHEL0V2RvWFz+l0+DiH5HSiEUJVbw3shNS5L/I+Kb0q2i8/mTNtSwZtH6W1p7D8npZI4IbC+vr8CRchE/VVUQ7vnZk4Io+0+rAy0RjHKG1iOsJ96jQeP8kMD9rlRc8kaP5ihN+he4qzByNRRPgS76BOxzCBZsT0/MYfCw5maQr5rc5/D8T/avV64FuGQd3IDMwbwd0GvxMlcPIt4nLEy2JJFDbfLNrwd2TuUvV7En8MB5fUWTroTdPjc5rm3rfoeZIZBVzqmVYtzJFxeqDaNnk08ZyjP96mRbkziqCVqs/DFVqzCf8eMjImsByVBR6g64CKOPsI9zyc20f3AOijQ3k52FuLbdyFbJi7XunaG9cevwlhHdhdNoLH8cJ0LJ3xLEcYcJax32MHS7RiTQudezm34Ybo+LxeP/5dDcV6kQ7xGqs3OIo9OzPNKFIDhyjZbTownm+HBIok=]

OR

block: >
    ENC[PKCS7,MIIDDQYJKoZIhvcNAQcDoIIC/jCCAvoCAQAxggEhMIIBHQIBADAFMAACAQEw
    DQYJKoZIhvcNAQEBBQAEggEANj8EexRpT58/lF0vnz9YoIzCBo2FlVQpO9tO
    WT7/4Zr0Mu+8MCvwlPL4KuyYpphCZCGggc5mDMVRjFqDNQoucW5UmQoDd/fX
    CojzUhkVmbq+XXa5fNi/woBBTuoKIeLs9DeGAKUvfmAIQ2jih/eCw/8zvs4x
    27MAC98eKbYNjbJ9ZunSxFw14WnotZGm+3DdG+SuMq0stGnPIsRPNXYTCJie
    tlquVBXX+0xEUp5YB+m0sf1kS8c1OfHgVsuTiUYSflcev6K5qGNASyaEezw9
    JPl7qOLiImvPRMPFDaxw5uj0QjWOalPaxNZccAqJgtXJFHXHluBiwCkOrxgZ
    +KS4VzCCAc4GCSqGSIb3DQEHATAdBglghkgBZQMEASoEEHGFAmyRi3jdkz0K
    pCP/CfqAggGgrlYIoT9A47445rUFpX9+VTbr1c75nQxOwxr262tJ2ZIFlbyi
    EbQxHQ6Xhx7c1LuyKLPAOYFRCYa0oJgu5eaqNomYQ7JMFG/0yHEL0V2RvWFz
    +l0+DiH5HSiEUJVbw3shNS5L/I+Kb0q2i8/mTNtSwZtH6W1p7D8npZI4IbC+
    vr8CRchE/VVUQ7vnZk4Io+0+rAy0RjHKG1iOsJ96jQeP8kMD9rlRc8kaP5ih
    N+he4qzByNRRPgS76BOxzCBZsT0/MYfCw5maQr5rc5/D8T/avV64FuGQd3ID
    Mwbwd0GvxMlcPIt4nLEy2JJFDbfLNrwd2TuUvV7En8MB5fUWTroTdPjc5rm3
    rfoeZIZBVzqmVYtzJFxeqDaNnk08ZyjP96mRbkziqCVqs/DFVqzCf8eMjIms
    ByVBR6g64CKOPsI9zyc20f3AOijQ3k52FuLbdyFbJi7XunaG9cevwlhHdhdN
    oLH8cJ0LJ3xLEcYcJax32MHS7RiTQudezm34Ybo+LxeP/5dDcV6kQ7xGqs3O
    Io9OzPNKFIDhyjZbTownm+HBIok=]
```
`eyaml encrypt -p` -- Encrypt a password
```bash
$ eyaml encrypt -p
Enter password: ****
string: ENC[PKCS7,MIIBeQYJKoZIhvcNAQcDoIIBajCCAWYCAQAxggEhMIIBHQIBADAFMAACAQEwDQYJKoZIhvcNAQEBBQAEggEAgsDALym1Yox7Y1hGzYSKI3SOvby3tT9GO1MlyCxgXPy40BIJvDyq0p9CQnciPmq6J/sGGfXP9YS0wj0Z/nbOnS1d49hduJV3JaSbc0Ltb7HkFXYWfimyLio8d0iAmbdYUt9xtua3BtZxynF/RpiuaXP34CtLIzJkIypR4g2ZAEZDGZTsYfcIUX7CcNr+TXH4ZVNsNrirNFxgPKaFoDoO2MJEklS6OlX9nZ22tT3v1D4MczOpYNyi4u4c/91AaOOdl91fAqmZFYOQYXun6734Tnutt9fbauFvYlgtCKXy1ouDQ7+Pffy6Or9BPRXjISdlZRsC1kKMReeyl7V++yi6fTA8BgkqhkiG9w0BBwEwHQYJYIZIAWUDBAEqBBBxfCfXacSEN+bNeXtlHgZMgBD1pcmJp7xqpiP/evhMcl6u]

OR

block: >
    ENC[PKCS7,MIIBeQYJKoZIhvcNAQcDoIIBajCCAWYCAQAxggEhMIIBHQIBADAFMAACAQEw
    DQYJKoZIhvcNAQEBBQAEggEAgsDALym1Yox7Y1hGzYSKI3SOvby3tT9GO1Ml
    yCxgXPy40BIJvDyq0p9CQnciPmq6J/sGGfXP9YS0wj0Z/nbOnS1d49hduJV3
    JaSbc0Ltb7HkFXYWfimyLio8d0iAmbdYUt9xtua3BtZxynF/RpiuaXP34CtL
    IzJkIypR4g2ZAEZDGZTsYfcIUX7CcNr+TXH4ZVNsNrirNFxgPKaFoDoO2MJE
    klS6OlX9nZ22tT3v1D4MczOpYNyi4u4c/91AaOOdl91fAqmZFYOQYXun6734
    Tnutt9fbauFvYlgtCKXy1ouDQ7+Pffy6Or9BPRXjISdlZRsC1kKMReeyl7V+
    +yi6fTA8BgkqhkiG9w0BBwEwHQYJYIZIAWUDBAEqBBBxfCfXacSEN+bNeXtl
    HgZMgBD1pcmJp7xqpiP/evhMcl6u]
```

`puppet lookup <KEY>` -- Lookup Hiera key(s) and value(s) on Puppet `[master]`
```bash
$ sudo /usr/local/bin/puppet lookup git.url
--- github.com
```

`lookup('<KEY>')` -- Lookup Hiera key(s) and value(s) in code
```ruby
lookup('teams').each |String $team| { # Hiera lookup; for each loop
  lookup("users.${team}").each |String $user| { # Hiera lookup; for each loop
    user { $user:
      ensure     => present,
      managehome => true,
      password   => lookup("${user}.password"), # Hiera lookup
      shell      => '/bin/bash',
    }
  } # End each $user loop
} # End each $team loop
```
### References
1. [About Hiera](https://puppet.com/docs/puppet/latest/hiera_intro.html).
2. Terri Haber (21 October 2014), _[Encrypt Your Data Using Hiera-Eyaml](https://puppet.com/blog/encrypt-your-data-using-hiera-eyaml)_, Puppet.com.
3. Gene Liverman (15 August 2018), _[My journey to securing sensitive data in Puppet code](https://puppet.com/blog/my-journey-securing-sensitive-data-puppet-code)_, Puppet.com.
4. The Official Hiera module:
   - [Puppet Forge](https://forge.puppet.com/hunner/hiera)
   - [GitHub](https://github.com/hunner/puppet-hiera/blob/master/README.md)

### Additional Plugins
-  [Hiera-eyaml](https://atom.io/packages/hiera-eyaml) package for Atom.io

### Contribute
Contributions are always welcome!
Please open an issue or fork and submit Pull Request.

### License
[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](http://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, [keysersoze206](https://github.com/keysersoze206) has waived all copyright and related, or neighboring rights, to this work.
