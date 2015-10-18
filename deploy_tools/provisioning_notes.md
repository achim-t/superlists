Provisioning a new site
=======================

## Required packages:

* nginx
* Python 3
* Git
* pip
* virtualenv

eg, on Ubuntu

  sudo apt-get install nginx git python3 python3-pip
  sudo pip3 install virtualenv

## Nginx Virtual Host config

* see nginx.template.conf
* replace SITENAME with, eg, staging.my-domain.com

## Upstart Job

* see gunicorn-upstart.template.conf
* replace SITENAME with, eg, staging.my-domain.com

## Folder structure:
Assume we have a user account at /home/username

/home/username
└── sites
    └── SITENAME
         └── database
         └── source
         └── static
         └── virtualenv

## Automated
* locally
  fab deploy:host=ubuntu@54.218.118.108

* on server
  sed "s/SITENAME/54.218.118.108/g" \
    deploy_tools/nginx.template.conf | sudo tee \
    /etc/nginx/sites-available/54.218.118.108
  sudo ln -s ../sites-available/54.218.118.108 \
    /etc/nginx/sites-enabled/54.218.118.108
  sed "s/SITENAME/54.218.118.108/g" \
    deploy_tools/gunicorn-upstart.template.conf | sudo tee \
    /etc/init/gunicorn-54.218.118.108.conf
  sudo service nginx reload
  sudo start gunicorn-54.218.118.108.eu
