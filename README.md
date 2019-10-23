# ssh_mailer
Sends e-mail whenever someone logs in to SSH (Triggers via PAM)

# Installation

Copy `ssh_mailer.py` to `/usr/bin/` and `chmod +x /usr/bin/ssh_mailer.py`.<br>
Then add the following to `/etc/pam.d/sshd`:

```
session   optional  pam_exec.so seteuid /usr/bin/ssh_mail.py
```

Now, whenever you login via SSH, PAM should execute `ssh_mail.py`
