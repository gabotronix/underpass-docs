# Open-SSH Server

### Initial Configuration

#### Create Users for OpenSSH:

Users for [OpenSSH Server](https://gitlab.com/vlasov-y/openssh-server) are created via a YAML file. The file is at `/opt/underpass/config/openssh/config.yml`

`config.yml` already contains a sample user named, `underpass`

You can replace it with a new name. For the password, generate it from [here](https://www.mkpasswd.net/?type=crypt-sha512):
```
Password: your_desired_password
Type: crypt-sha512
```

Click on the `Hash` button. The page will generate a hash which you can then copy to `config.yml`

When pasting the hash in `password:`, please note that it is enclosed in single quotations `'hash_here'`.

If you wish to create more users, simply copy-paste the entries below `users:`

***