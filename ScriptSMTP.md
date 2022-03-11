# Save mail box/configuration dovecot and postfix


#!/bin/bash
#INSTALL postfix and dovecot(pop3)
ssh -t root@192.168.144.35 << EOF
echo “install postfix and dovecot(POP3)
apt-get install postfix
apt-get install dovecot-core dovecot-pop3d
adduser compras
adduser ventas
adduser marketing
adduser administracio
adduser recepcio
adduser tecnics
EOF

#Upload configuration
scp main.cf root@192.168.144.35:/etc/postfix/main.cf
scp dovecot.conf root@192.168.144.35:/etc/dovecot/dovecot.conf
scp 10-mail.conf root@192.168.144.35:/etc/dovecot/conf.d/10-mail.conf
scp 10-auth.conf root@192.168.144.35:/etc/dovecot/conf.d/10-auth.conf
scp 10-ssl.conf root@192.168.144.35:/etc/dovecot/conf.d/10-ssl.conf
#Upload mails
scp ventas.tar.gz root@192.168.144.35:/scriptdir/script
scp compras.tar.gz root@192.168.144.35:/scriptfir/script

ssh -t root@192.168.144.35  << EOF
tar -zxvf /scriptdir/ventas.tar.gz
tar -zxvf /scriptdir/compras.tar.gz
cp -r /scriptdir/ventas /home/ 
cp -r /scriptdir/compras  /home/ 
echo “Restarting service”
service systemctl reload postfix
echo “correo de prueba” | sendmail compras@fgblegacy.org
EOF

#endscript

