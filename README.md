# Canadian Legislative Scrapers

[![Dependency Status](https://gemnasium.com/opennorth/scrapers_ca_app.png)](https://gemnasium.com/opennorth/scrapers_ca_app)

This Django project runs the [Canadian legislative scrapers](http://github.com/opencivicdata/scrapers-ca), displays the status of each scraper, and returns the scraped data as JSON.

## Development

Follow the instructions in the [Python Quick Start Guide](https://github.com/opennorth/opennorth.ca/wiki/Python-Quick-Start%3A-OS-X) to install Homebrew, Git, PostgreSQL, Python and virtualenv.

    mkvirtualenv scrapers_ca_app
    git clone git@github.com:opennorth/scrapers_ca_app.git
    cd scrapers_ca_app

Set up the submodule and switch it to master:

    git submodule init
    git submodule update
    cd scrapers
    git checkout master
    cd ..

Install the requirements:

    pip install -r requirements.txt

Create a database (`dropdb pupa` if it already exists):

    dropdb pupa
    createdb pupa
    python manage.py syncdb --noinput

Run all the scrapers:

    python manage.py update

Or run specific scrapers:

    python manage.py update ca_ab_edmonton ca_ab_grande_prairie_county_no_1

Install the foreman gem:

    gem install foreman

Start the web app:

    foreman start

## Deployment

Add configuration variables (replace `REPLACE`):

    heroku config:set PRODUCTION=1
    heroku config:set AWS_ACCESS_KEY_ID=REPLACE
    heroku config:set AWS_SECRET_ACCESS_KEY=REPLACE
    heroku config:set DJANGO_SECRET_KEY=REPLACE
    heroku config:set DATABASE_URL=`heroku config:get REPLACE`

You can [generate a secret key in Python](https://github.com/django/django/blob/master/django/core/management/commands/startproject.py):

```python
from django.utils.crypto import get_random_string
get_random_string(50, 'abcdefghijklmnopqrstuvwxyz0123456789!@#$%^&*(-_=+)')
```

Setup the database (replace `DATABASE`):

    heroku pg:reset DATABASE
    heroku run python manage.py syncdb --noinput

Add `python manage.py update` to the [Heroku Scheduler](https://scheduler.heroku.com/dashboard).

### Eliminating duplicates

If, while developing your scraper, you create duplicates, you may need to:

    python manage.py flush JURISDICTION-ID-OR-DIVISION-ID

## Troubleshooting

* Make sure PostgreSQL is running. If you use Homebrew, you can find instructions with:

        brew info postgres

## Bugs? Questions?

This repository is on GitHub: [http://github.com/opennorth/scrapers_ca_app](http://github.com/opennorth/scrapers_ca_app), where your contributions, forks, bug reports, feature requests, and feedback are greatly welcomed.

Copyright (c) 2013 Open North Inc., released under the MIT license
