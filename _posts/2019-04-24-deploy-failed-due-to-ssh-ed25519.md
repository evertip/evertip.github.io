---
layout: post
title:  "Deploy failed due to ssh ed25519"
author: duybuffet
categories: [ tips ]
image: assets/images/ssh.jpg
---
**I got this error when trying to deploy to a remote server:**
```
NotImplementedError: unsupported key type `ssh-ed25519'
net-ssh requires the following gems for ed25519 support:
 * ed25519 (>= 1.2, < 2.0)
 * bcrypt_pbkdf (>= 1.0, < 2.0)
```

**To pass through that, following these steps:**

* Add the following gem to Gemfile and re - bundle
```
gem 'bcrypt_pbkdf', '< 2.0', :require => false
gem 'ed25519', '< 2.0', :require => false
```
* Generate and use latest recommended public-key algorithm (Ed25519)
```
ssh-keygen -o -a 100 -t ed25519 -f ~/.ssh/id_ed25519 -C “duy@example.com"
```
* Add your public key to remote server
* Configure your IndentityFile to your private key
```
Host awesome
  HostName 198.222.111.33
  User Duy
  IdentityFile ~/.ssh/id_ed25519
  IdentitiesOnly yes
```
