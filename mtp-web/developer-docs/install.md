---
description: Installation instructions from source code...
---

# Install

{% hint style="info" %}
[This documentation is designed for developers. If you want to use the Map the Path online web application, go here.](https://mtp.trekview.org/)
{% endhint %}

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

#### Build

The following command will build Map the Paths.

```text
pip install -r requirements.txt
python manage.py makemigrations
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

### Deploying with git on Heroku

We host Map the Paths on Heroku. We use an auto-deployment pipeline from the Github repo:

* Production branch \(`production`\): deploys to production `https://mtp.trekview.org`
* Staging branch \(`staging`\): deploys to staging `https://map-the-paths-develop.herokuapp.com/`

To deploy on your own Heroku environment.

1. Create a new app on Heroku
2. Connect your Github repository to the created app \(and make sure correct branch is connected if using auto deployment\)
3. Set `DJANGO_SETTINGS_MODULE` to `config.settings_heroku` in `manage.py` and `config/wsgi.py`.

```text
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'config.settings_heroku')
```

4. Make sure Heroku environemental vars are set to match those listed in `config/setting_local.py.` 5. Push code to Github

