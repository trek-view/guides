---
description: Installation instructions from source code...
---

# Install

{% hint style="info" %}
[This documentation is designed for developers. If you want to use the Map the Path online web application, go here.](https://www.mapthepaths.com/)
{% endhint %}

## Cloud Services

### AWS Setup

In production we use AWS; IAM, S3, Route53, Certificate Manager and CloudFront

#### S3 Create Buckets

The app uses two buckets:

1. `AWS_S3_BUCKET` For static files \(e.g CSS, avatars, local scripts\)
2. `AWS_S3_MAPILLARY_BUCKET` For photo files \(downloaded from Mapillary\)

You need to create two buckets for this purpose.

The `AWS_S3_BUCKET` need to be public so that viewers can load files.

To do this go to the bucket and click permissions. Make sure "Block all public access" is set to false.

![AWS Bucket permissions](../../.gitbook/assets/8be45665-6c32-4367-bb0e-98cc37814803.png)

Under bucket policy set the following, making sure to replace the `"Resource"` value with you bucket name. Do not replace the trailing `/*`

```text
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*"
        }
    ]
}
```

Make sure to now set them in the app environmental variables as:

```text
AWS_S3_BUCKET
AWS_S3_MAPILLARY_BUCKET
```

#### Cloudfront

We use Cloudfront to resolve the bucket domains.

For example, making `https://s3.eu-west-2.amazonaws.com/mybucket.com` resolve to `mybucket.com`

First you'll need an SSL certificate.[ We use free certificates from AWS Certificate Manger that can be created here.](https://eu-west-2.console.aws.amazon.com/acm/home?)

Now create a new CloudFront Web distribution.

Set the origin domain name and path to your S3 bucket from the dropdown list.

Set the CNAME to the URL of the bucket you want to use \(e.g. `https://s3.eu-west-2.amazonaws.com/mybucket.com`

Select the certificate created previously and force HTTPS request.

Save the distribution.

As we use Route53 \(DNS\) from AWS, we can get AWS to update the DNS with the CNAME data. If you use another DNS service, you'll need to add a CNAME record manually like so:

`CNAME` `https://s3.eu-west-2.amazonaws.com/mybucket.com` `CLOUDFRONT_DOMAIN`

The CNAME chosen \(e.g. `https://s3.eu-west-2.amazonaws.com/mybucket.com` \) should then be added to the following app environment variable.

```text
AWS_S3_BUCKET_CNAME
```

#### IAM

[For security we can now create some users/groups/policies in AWS IAM to restrict who can interact with the buckets.](https://console.aws.amazon.com/iam/home)

**Create a new policy**

Create a custom policy we can use to restrict bucket access.

```text
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "s3:GetAccessPoint",
                "s3:PutAccountPublicAccessBlock",
                "s3:GetAccountPublicAccessBlock",
                "s3:ListAllMyBuckets",
                "s3:ListAccessPoints",
                "s3:ListJobs",
                "s3:CreateJob",
                "s3:HeadBucket"
            ],
            "Resource": "*"
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": "s3:*",
            "Resource": [
                "arn:aws:s3:::mapillary-images-bucket",
                "arn:aws:s3:::static-images-bucket",
                "arn:aws:s3:*:*:accesspoint/*",
                "arn:aws:s3:::*/*",
                "arn:aws:s3:*:*:job/*"
            ]
        }
    ]
}
```

Here you can see the two buckets under the "Resource" object.

```text
            "Resource": [
                "arn:aws:s3:::mapillary-images-bucket",
                "arn:aws:s3:::static-images-bucket",
```

Replace these with your own bucket names and create the policy.

#### Create new group

We will assign the new policy to a group.

Create a new group and attach the newly created policy.

#### Create new user

Now we can create the user.

Create the user, making sure to attach them to the newly created group.

At the end you will receive an access key and secret key. These values can be used for the following app environment variables:

```text
AWS_ACCESS_KEY
AWS_SECRET_KEY
```

### Mailgun Setup

{% hint style="info" %}
We use Mailgun. Any other SMTP provider \(Sendgrid, Mandrill, etc\) should work without issue.
{% endhint %}

Setup Mailgun \(they will guide you through requiements\) and create an SMTP user.

Once done you'll need to add the following to the app environment variables:

* `MAILGUN_PRIVATE_API_KEY`
* `SMTP_HOSTNAME`
* `SMTP_PASSWORD`
* `SMTP_PORT`
* `SMTP_USER`

You also need to set the following to the app environment variables related to emails:

* `SMTP_REPLY_TO` : this is added in case your `SMTP_USER` does not resolve to a real mailbox. When we send emails we use this as the `Reply-To` header value. [Here's a good explanation why](https://stackoverflow.com/questions/1235534/what-is-the-behavior-difference-between-return-path-reply-to-and-from) .
* `SMTP_FROM_NAME` : used as the display name in email clients when users receive emails

### Mailerlite Setup

When users sign up, we give them the opportunity to explicitly opt in to our email list.

[We user Mailerlite.](https://www.mailerlite.com/)

You'll first need to create a new list/group in Mailerlite and then grab you api key/token.

These values should then be added as the app environment variables:

```text
MAILERLITE_GROUPID
MAILERLITE_TOKEN
```

### Mapillary Setup

#### MTPW Web app

[The web app uses the Mapillary API for a variety of features.](https://www.mapillary.com/developer/api-documentation/)

[You'll first need to create a Mapillary OAuth app here.](https://www.mapillary.com/dashboard/developers)

It is important when creating the app you set the following two values:

1. Callback URL: `DOMAIN/accounts/check-mapillary-oauth` \(e.g. [https://myserver.com/accounts/check-mapillary-oauth\](https://myserver.com/accounts/check-mapillary-oauth\)\)
2. Scopes: Mark all

When the app is created, you will get a Client ID, Client secret and authentication URL.

These values should then be added as the app environment variables:

```text
MAPILLARY_AUTHENTICATION_URL
MAPILLARY_CLIENT_ID
MAPILLARY_CLIENT_SECRET
```

#### MTP Desktop uploader

If you want the app to be able to work with the MTPDU, you must also add the Mapillary Oauth app values used by the uploader. Without setting these, the API will not work.

These values should then be added as the app environment variables:

```text
MTP_DESKTOP_UPLOADER_CLIENT_ID
MTP_DESKTOP_UPLOADER_CLIENT_SECRET
```

### Mapbox Setup

For maps in the app we use Mapbox.

Mapbox is a paid service but has a generous free tier.

When you've created an account, [you need to create an access token here.](https://account.mapbox.com/access-tokens/c)

You should allow the default scopes

![Mapbox scopes](../../.gitbook/assets/edd26b1d-1a92-4564-b34f-80742adf656d.png)

It's a good idea to restrict the token to request from the domain you will be using.

Once created, the api key should then be added as the app environment variable:

```text
MAPBOX_TOKEN
```

### Font Awesome setup

We use Font Awesome free icons in the app.

You'll need to create a Font Awesome kit in order to use these icons.

[Sign up to create an account and a kit here.](https://fontawesome.com/start)

Once created, the kit id should then be added as the app environment variable:

```text
FONT_AWESOME_KIT
```

### Google Analytics setup

We use Google Analytics to track usage. You need to create a new property on Google Analytics to generate a site id \(e.g. `UA-123456789-0`\)

Once created, the id should then be added as the app environment variable:

```text
GOOGLE_ANALYTICS
```

## Deploying locally

### Clone

```text
git clone https://github.com/trek-view/mtp-web.git
```

### Set up environment

#### Prerequisites

* PostgreSql 10.0+
* Postgis package
* Python 3.5+

#### Set variables

Set `DJANGO_SETTINGS_MODULE` to `config.settings_local` in `manage.py:`

```text
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'config.settings_local')
```

Fill following values in `config/setting_local.py`

```text
DATABASES = {
    'default': {
        'ENGINE': 'django.contrib.gis.db.backends.postgis',
        'NAME': '',
        'USER': '',
        'PASSWORD': '',
        'HOST': 'localhost',
        'PORT': '5432'
    }
}

# SECURITY WARNING: keep the secret key used in production secret!
SECRET_KEY = ''

MAPBOX_PUBLISH_TOKEN = 'your_mapbox_token'

MAILERLIST_API_KEY = 'your_maillist_api_key'

MAILERLIST_GROUP_ID = 'your_maillist_aroup_id'

GOOGLE_ANALYTICS = 'your_google_analysis_key'

# EMAIL_BACKEND = "django.core.mail.backends.filebased.EmailBackend"
EMAIL_FILE_PATH = os.path.join(BASE_DIR, "sent_emails")

# SMTP settings
# EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_HOST = 'smtp_host'
DEFAULT_FROM_EMAIL = 'xxx@your_domain'
EMAIL_HOST_USER = 'xxx@your_domain'
EMAIL_HOST_PASSWORD = 'smtp_password'
EMAIL_USE_TLS = True
EMAIL_PORT = 587

SMTP_REPLY_TO = DEFAULT_FROM_EMAIL
```

If you want to enable two auth for admin users, please add this in config/urls.py

```text
admin.site.__class__ = AdminSiteOTPRequiredMixinRedirSetup
```

#### Build

The following command will build Map the Paths.

```text
pip install -r requirements.txt
python manage.py makemigrations
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

## Deploying to Digital Ocean

We host Map the Paths on Digital Ocean droplets. The following steps should work on any cloud Iaas.

**1. Create ssh key and add to DO**

`$ ssh-keygen -f KEYNAME -C noname`

**2. Choose droplet deploy with SSH key**

Setup a droplet, using Ubuntu 20.04 \(LTS\) x64.

**3. Connect to your droplet**

`$ ssh -i "YOURPRIVATEKEYNAME" ROOT@SERVERIP`

**4. Update packages**

`$ sudo apt-get update`

**5. Add more users**

`$ sudo adduser USERNAME`

**6. Make user sudo if needed**

`sudo usermod -aG sudo USERNAME`

**7. Add users public key**

`su USERNAME`

`mkdir ~/.ssh/`

`cd`

`nano .ssh/authorized_keys`

Now paste in their public key.

**8. Harden security**

`nano /etc/ssh/sshd_config`

Then set the following

```text
PermitRootLogin no
StrictModes yes
PubkeyAuthentication yes
PasswordAuthentication no
```

**9. Now restart SSH**

`sudo service ssh restart`

_**10. Set up Firewall rules**_

All non-essential ports should be blocked using ufw.

First switch off ufw \(to ensure we don't kick ourselves off server when blocking all ports\): `sudo ufw disable`

Then block all ports: `sudo ufw default deny`

The open required ports:

* SSH `sudo ufw allow 22`
* HTTP `sudo ufw allow 80`
* HTTPS `sudo ufw allow 443`
* \`\`

To ensure config is saved when changed install iptables-persistent:

`sudo apt-get install iptables-persistent`

And save iptables set above

`sudo service netfilter-persistent save`

Then turn on ufw to enable rules: `sudo ufw enable`

**11. Deploy app to server**

You can

1. set up auto deployments

{% embed url="https://www.digitalocean.com/community/tutorials/how-to-set-up-automatic-deployment-with-git-with-a-vps" caption="" %}

OR 

2. Manually clone from the repository:

`git clone` [`https://github.com/trek-view/mtp-web.git`](https://github.com/trek-view/mtp-web.git)\`\`

**12. Install PostgreSQL**

Docs here: https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-20-04

```text
sudo apt install postgresql postgresql-contrib;

sudo -i -u postgres;

psql;

ALTER USER postgres PASSWORD 'myPassword';

exit;

sudo apt install postgis postgresql-12-postgis-3
```

**13. Install Python Packages**

```text
sudo -H pip3 install virtualenv

virtualenv venv

source venv/bin/activate

pip install -r requirements.txt
```

**14. Update .env file**

[Earlier in this documentation, the cloud services needed to run the app are explained.](install.md#cloud-services)

Now create an `.env` file in the root directory of the install.

We've create a sample with all the values \(most are required\):

{% embed url="https://github.com/trek-view/mtp-web/blob/staging/.env.example" %}

_Note on 2FA. You can enforce two factor authentication for admin using the 2fa setting in the env file. You can then modify and manage the site settings in django admin._

**15. Install DB tables**

```text
python manage.py migrate
```

**16. Create first staff user for django admin**

```text
python manage.py createsuperuser
```

**17. exit venv**

```text
deactivate
```

**18. Install Nginx**

```text
apt-get update
apt-get install nginx -y
systemctl enable nginx
systemctl start nginx
```

create nginx config file.

```text
sudo nano /etc/nginx/sites-enabled/myproject.conf
```

fill the file following contents.

```text
server {
    server_name $YOUR_DOMAIN_NAME;

    location = /favicon.ico { access_log off; log_not_found off; }
    location /static/ {
        root /home/mpt-web;
    }

    location / {
        include proxy_params;
        proxy_pass http://127.0.0.1:8000/;
    }
    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/staging.mapthepaths.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/staging.mapthepaths.com/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
    client_max_body_size 20M;
}
server {
    if ($host = $YOUR_DOMAIN_NAME) {
        return 301 https://$host$request_uri;
    } # managed by Certbot
    listen 80;
    server_name $YOUR_DOMAIN_NAME;
    return 404; # managed by Certbot
}
```

Note, this controls the limit of files that can be uploaded using the web app. Default is 20MB. These are for things like avatars and guidebook images. It **does not** have an impact on the size of sequence images.

**19. Install Encrypt SSL**

```text
snap install core 
sudo snap refresh core
snap install --classic certbot
ln -s /snap/bin/certbot /usr/bin/certbot
certbot --nginx

certbot --nginx  --agree-tos --register-unsafely-without-email -d $YOUR_DOMAIN_NAME www.$YOUR_DOMAIN_NAME
```

**20 Create Python Service File**

```text
sudo nano /etc/systemd/system/python.service
```

fill the file following content.

```text
[Unit]
Description=Python daemon
After=network.target

[Service]
User=root
Group=root
WorkingDirectory=$APP_ROOT_PATH
ExecStart=$APP_ROOT_PATH/venv/bin/python3 $APP_ROOT_PATH/manage.py runserver

[Install]
WantedBy=multi-user.target
```

**21 Run MTPW APP**

```text
systemctl daemon-reload
systemctl start python.service
systemctl enable python.service
```

Check MTPW Service

```text
systemctl status python.service
```

Stop MTPW Service

```text
systemctl stop python.service
```

Restart MTPW Service

```text
systemctl restart python.service
```

**\* Install Nginx, SSL and Run Service \(Quick Run\)**

**This is a hacky script for running the final setup steps \(18,19,20\)**

```text
chmod +x nginx+SSL+python.txt

./nginx+SSL+python.txt
```

