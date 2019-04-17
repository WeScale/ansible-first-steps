
# ansible-first-steps

```
#!/bin/bash
sudo apt-get update
sudo apt-get install -yq curl
sudo apt-get install -yq apache2

COMMAND=`systemctl status apache2 | grep "running"`
if [[ $COMMAND == "" ]]
then
        exit 1
fi

COMMAND=`curl http://localhost | grep -i "It works"`
if [[ $COMMAND == "" ]]
then
        exit 1
fi

sudo mkdir -p /var/www/wescale.com/html
sudo chown -R $USER:$USER /var/www/wescale.com/html
sudo chmod -R 755 /var/www/wescale.com
sudo cat > /var/www/wescale.com/html/index.html <<EOF
<html>
        <head>
                <title> Welcome Devoxx !</title>
        </head>
        <body>
                <h1>Success ! YEAHHHHHHH NO DEMO EFFECT !</h1>
        </body>
</html>
EOF

sudo cat > /etc/apache2/sites-available/wescale.com.conf <<EOF
<VirtualHost *:80>
        ServerAdmin admin@wescale.com
        ServerName wescale.com
        ServerAlias www.wescale.com
        DocumentRoot /var/www/wescale.com/html
        ErrorLog /var/log/error.log
        CustomLog /var/log/access.log combined
</VirtualHost>
EOF

a2ensite wescale.com.conf
a2dissite 000-default.conf

COMMAND=`apache2ctl configtest | grep -i "Syntax OK"`
if [[ $COMMAND == "" ]]
then
        exit 1
fi

sudo systemctl restart apache2

COMMAND=`curl htt://localhost | grep -i "ESGI CLOUD DAY"`
if [[ $COMMAND == "" ]]
then
        exit 1
fi
```
