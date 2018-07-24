# Conoha server setup memo

2018/01/03

* Ubuntu 16.04

[toc]

## First of all

```bash
apt-get update && apt-get upgrade
```

### sshd

#### add user

```bash
adduser <user-name>
gpasswd -a user-name sudo
```

#### Deny root login

/etc/ssh/sshd_config

```bash
PermitRootLogin no
AuthorizedKeysFile .ssh/authorized_keys
```

### git

```git
git config --global user.name "<user-name>"
git config --global user.email "<email-address>"
```

#### add git user

```bash
sudo useradd git
sudo passwd git
sudo mkdir /home/repos
sudo chown git:git /home/repos
sudo usermod -d /home/repos
#sudo usermod -s /usr/bin/git-shell git
```

<!--
#### add commands

```bash
sudo cp ~/Dropbox/memo/git/* /home/repos/
sudo chown git:git /home/repos/*
```

##### client

```bash
## alias for yuchesc
alias gitinit='ssh git@yuchesc ./gitinit.sh'
alias gitlist='ssh git@yuchesc ./gitlist.sh'
alias gitdelete='ssh git@yuchesc ./gitdelete.sh'
```

#### リポジトリ追加

```bash
sudo su - git
mkdir www.git
cd www.git
git --bare init --shared
```
-->

### Nginx

default.conf
nginx.conf

#### security setting

/etc/nginx/nginx.conf

```nginx
server_tokens off;
```

/etc/nginx/conf.d/default.conf

```nginx
etag off;
add_header X-Frame-Options SAMEORIGIN;
add_header X-XSS-Protection "1; mode=block";
add_header X-Content-Type-Options nosniff;
client_max_body_size 1k;
client_header_buffer_size 1k;
large_client_header_buffers 4 8k;
```

### dropbox

cd ~ && wget -O - "https://www.dropbox.com/download?plat=lnx.x86_64" | tar xzf -

#### systemd

/etc/systemd/system/dropbox.service

```bash
[Unit]
Description=Dropbox
After=local-fs.target network.target

[Service]
ExecStart=/home/hoshi/.dropbox-dist/dropboxd
User=hoshi

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl enable dropbox
```

## dummy cert

Change dir in /etc/ssl/openssl.conf

dir = ./yuchescCA

### make directories

```bash
cd /etc/ssl
sudo mkdir -p yuchescCA/certs
sudo mkdir -p yuchescCA/private
sudo mkdir -p yuchescCA/crl
sudo mkdir -p yuchescCA/newcerts
sudo sh -c 'echo "01" > yuchescCA/cerial'
sudo touch yuchescCA/index.txt
cd yuchescCA
sudo openssl req -new -x509 -newkey rsa:2048 -out cacert.pem -keyout private/cakey.pem -days 36500
```

```bash
Country Name (2 letter code) [AU]:JP
State or Province Name (full name) [Some-State]:
Locality Name (eg, city) []:
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Yuchesc
Organizational Unit Name (eg, section) []:
Common Name (e.g. server FQDN or YOUR name) []:www.yuchesc.com
Email Address []:
```

```bash
sudo openssl rsa -in private/cakey.pem -out newcerts/nopass_cakey.pem

```

## oracle-jdk

```bash
sudo add-apt-repository ppa:webupd8team/java
```

Execute below if ssl error happened.

```bash
sudo apt-get install --reinstall ca-certificates
```

And then

```bash
sudo apt-get update
sudo apt-get install oracle-java9-jdk
```

### inotify

```bash
sudo apt-get install inotify-tools
```

### node npm

```bash
curl -sL https://deb.nodesource.com/setup_9.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo npm install npm@latest -g
```

#### Stylus

```bash
sudo npm install stylus -g
```

stylus -w ~/Dropbox/yuchesc/html/style/model.styl -o /var/www/html/style/model.css

### apt

```bash
sudo apt-get install unzip
```

### Fluentd

#### Change log's permission for Nginx

/etc/logrotate.d/nginx

0640 -> 0644

#### install

```bash
curl -L https://toolbelt.treasuredata.com/sh/install-ubuntu-xenial-td-agent2.sh | sh
```

#### /etc/td-agent.conf

```text
<source>
  @type tail
  format nginx
  path /var/log/nginx/access.log
  pos_file /var/log/td-agent/nginx.access.pos
  tag nginx.access
</source>

<match nginx.access>
  @type stdout
</match>
```

#### elasticsearch

If there is not gcc or make, it may happened below.

/opt/td-agent/embedded/lib/ruby/2.1.0/mkmf.rb:467:in `try_do': The compiler failed to generate an executable file. (RuntimeError)

sh: 1: make: not found

```bash
sudo apt-get install gcc make
sudo td-agent-gem install elasticsearch

wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
sudo apt-get install apt-transport-https
echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-6.x.list
sudo apt-get update && sudo apt-get install elasticsearch
sudo systemctl daemon-reload
sudo systemctl enable elasticsearch.service
```

I realized elasticsearch needs so much memory space so I ended up continue setup...


