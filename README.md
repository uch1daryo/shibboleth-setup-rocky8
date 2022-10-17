# Shibboleth setup Rocky Linux 8

Ansible playbooks for Shibboleth IdP/SP auto setup on Rocky Linux 8.

## Installation

clone this repository at control node.

```bash
git clone git@github.com:uch1daryo/shibboleth-setup-rocky8.git
```

copy hosts example to hosts, do the same for others. edit these files.

```bash
cd shibboleth-setup-rocky8
cp hosts.example hosts
cp group_vars/all.yml.example group_vars/all.yml
cp group_vars/idpserver.yml.example group_vars/idpserver.yml
cp group_vars/spserver.yml.example group_vars/spserver.yml

vim hosts
vim group_vars/all.yml
vim group_vars/idpserver.yml
vim group_vars/spserver.yml
```

check!

```bash
ansible-playbook -i hosts site.yml --check
```

run!!

```bash
ansible-playbook -i hosts site.yml
```

## Configuration

### update metadata

insert self-signed certificate in sp-metadata.xml on SP server.

```bash
cat /etc/shibboleth/sp-cert.pem
sudo vim /var/www/html/sp-metadata.xml
```

IdP server as well.

### share metadata and restart service

download idp-metadata.xml as partner-metadata.xml on SP server, restart service.

```bash
cd /etc/shibboleth/
sudo wget -O partner-metadata.xml https://idp.example.com/idp-metadata.xml
sudo systemctl restart shibd.service
```

IdP server as well.

```bash
cd /opt/shibboleth-idp/metadata/
sudo wget -O partner-metadata.xml https://sp.example.com/sp-metadata.xml
sudo systemctl restart jetty.service
```

## Test

create test content on SP server.

```bash
cd /var/www/html/
sudo mkdir secure
sudo vim secure/index.html
```

access `https://sp.example.com/secure/` and authenticate.

## Credit

- [貴学にてIdPv4をインストールする場合の構築手順](https://meatwiki.nii.ac.jp/confluence/pages/viewpage.action?pageId=20021624)
- [貴学にてSPをインストールする場合の構築手順](https://meatwiki.nii.ac.jp/confluence/pages/viewpage.action?pageId=12158264)

- [Shibboleth環境構築セミナー（基礎編：IdP）](https://meatwiki.nii.ac.jp/confluence/pages/viewpage.action?pageId=21441389)
- [Shibboleth環境構築セミナー（基礎編：SP）](https://meatwiki.nii.ac.jp/confluence/pages/viewpage.action?pageId=21441391)
- [Shibboleth環境構築セミナー（活用編）](https://meatwiki.nii.ac.jp/confluence/pages/viewpage.action?pageId=12158239)
  - [学内システムとして構築する方法](https://meatwiki.nii.ac.jp/confluence/pages/viewpage.action?pageId=12158249)

- [IdPメタデータテンプレート](https://meatwiki.nii.ac.jp/confluence/pages/viewpage.action?pageId=12158233)
- [SPメタデータテンプレート](https://meatwiki.nii.ac.jp/confluence/pages/viewpage.action?pageId=12158235)

- [certbot instructions - Apache on CentOS 8](https://certbot.eff.org/instructions?ws=apache&os=centosrhel8)
