#### CREATE CERT SSL FOR RANCHER
````bash
sudo certbot certonly --standalone -d rancher.subdomain.com --preferred-challenges http --agree-tos -m email@email.com --keep-until-expiring
````

#### GET RANCHER PASSWORD
````bash
docker logs rancher-server 2>&1 | grep "Bootstrap Password:"
````