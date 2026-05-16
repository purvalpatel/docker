Private Docker Registry like Quay.

## Setup With Docker - Selfhosted 

Download
```
wget https://github.com/goharbor/harbor/releases/download/v2.14.0/harbor-online-installer-v2.14.0.tgz
tar -xf harbor-online-installer-v2.14.0.tgz
cd harbor
```

Change configuration
```
cp harbor.yml.tmpl harbor.yml
```
in harbor.yml,
- change the hostname : reg.xxx.xx [ it must be DNS records or add into servers /etc/hosts )
- Disable https portion
- Change Password `harbor_admin_password`
- Change data volume `data_volume`

Install:
```
sudo ./install.sh
```

Open in browser: <br>
![Uploading image.png…]()

