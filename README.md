# ssh_mailer
Sends e-mail whenever someone logs in to SSH (Triggers via PAM)

# Installation

Copy `ssh_mailer.py` to `/usr/bin/` and `chmod +x /usr/bin/ssh_mailer.py`.<br>
Then add the following to `/etc/pam.d/sshd`:

```
session   optional  pam_exec.so seteuid /usr/bin/ssh_mail.py
```

Then create certificates:

```
mkdir -p /etc/sshmailer
openssl genrsa -out /etc/sshmailer/sshmailer.pem 1024
openssl rsa -in /etc/sshmailer/sshmailer.pem -out /etc/sshmailer/sshmailer.pub -pubout
```

And copy the public part of the cert (without the `--- BEGIN ---` parts into your DNS TXT record, which should look something like:

```
cat /etc/sshmailer/sshmailer.pub
INSERT INTO records (name, type, content) VALUES ('default._domainkey.hvornum.se', 'TXT', 'v=DKIM1;k=rsa;p=MIGfMA+PUejXCfAQAB');
```

Now, whenever you login via SSH, PAM should execute `ssh_mail.py`
