## notify-boot-service

```
cp notify-boot /usr/sbin/notify-boot
cp notify-boot.service /etc/systemd/system/notify-boot.service
systemctl enable notify-boot
systemctl enable systemd-networkd-wait-online 
```

## mandrill w/ postfix

### debian / ubuntu
`apt-get install libsasl2-modules`

### arch
`pacman -S postfix cyrus-sasl`

### postfix + sasl

```
echo [smtp.mandrillapp.com] USERNAME:API_KEY >> /etc/postfix/sasl_passwd
chmod 600 /etc/postfix/sasl_passwd
postmap /etc/postfix/sasl_passwd
```

```
cat << EOF >> /etc/postfix/main.cf
# enable SASL authentication
smtp_sasl_auth_enable = yes
# tell Postfix where the credentials are stored
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd 
smtp_sasl_security_options = noanonymous
# use STARTTLS for encryption
smtp_use_tls = yes 
relayhost = [smtp.mandrillapp.com]
EOF
```


