Stouts.sentry
=============

[![Build Status](https://travis-ci.org/Stouts/Stouts.sentry.png)](https://travis-ci.org/Stouts/Stouts.sentry)

Ansible role which install and setup [Sentry](https://getsentry.com)

### Requirements

- [Stouts.nginx](https://github.com/Stouts/Stouts.nginx)
- [Stouts.python](https://github.com/Stouts/Stouts.python)
- [Stouts.redis](https://github.com/Stouts/Stouts.redis)
- [Stouts.supervisor](https://github.com/Stouts/Stouts.supervisor)

#### Recomended

- [Stouts.postfix](https://github.com/Stouts/Stouts.postfix)


#### Variables
```yaml
sentry_enabled: yes                                       # Enable the role
sentry_home: /usr/lib/sentry                              # Deploy sentry to the folder
sentry_user: sentry                                       # Run as user
sentry_hostname: "{{inventory_hostname}}"                 # Bind to hostname
sentry_secret_key: 1LsmGR1DIyCJ5n2bRG5IVOFHdzEPkTKlW0RzxZVe9S0vc

# Initial data
sentry_admin_username: admin                              # Creates admin user with credentials, set blank for skip
sentry_admin_email: admin@{{sentry_hostname}}
sentry_admin_password: admin
sentry_team_name: sentry                                  # Creates team for admin user, set blank for skip
sentry_project_name: sentry                               # Creates project for admin user, set blank for skip
sentry_project_platform: python

# Setup gunicorn
sentry_web_host: 127.0.0.1
sentry_web_port: 9000
sentry_web_options: { workers: 3, limit_request_line: 0, secure_scheme_headers: {'X-FORWARDED-PROTO': 'https'} }

# Setup databases
sentry_db_engine: django.db.backends.sqlite3
sentry_db_name: "{{sentry_home}}/sentry.sqlite"
sentry_db_user: postgres
sentry_db_password: ""
sentry_db_host: ""
sentry_db_port: ""
sentry_db_options: {}

# Setup cache
sentry_cache_backend: "redis_cache.RedisCache"
sentry_cache_location: "{{redis_bind}}:{{redis_port}}"

# Queue settings
sentry_broker_url: "redis://{{redis_bind}}:{{redis_port}}"

# Buffer settings
sentry_buffer: "sentry.buffer.redis.RedisBuffer"
sentry_redis_host: "{{redis_bind}}"
sentry_redis_port: "{{redis_port}}"

# Social auth settings
sentry_twitter_consumer_key: ""
sentry_twitter_consumer_secret: ""
sentry_facebook_app_id: ""
sentry_facebook_api_secret: ""
sentry_google_oauth2_client_id: ""
sentry_google_oauth2_client_secret: ""
sentry_github_app_id: ""
sentry_github_api_secret: ""
sentry_trello_api_key: ""
sentry_trello_api_secret: ""
sentry_bitbucket_consumer_key: ""
sentry_bitbucket_consumer_secret: ""

# Email settings
sentry_server_email: 'sentry@{{sentry_hostname}}'         # From email
sentry_email_settings: []                                 # Ex. sentry_email_settings:
                                                          #       - EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
                                                          #       - EMAIL_HOST = 'localhost'
                                                          #       - EMAIL_HOST_PASSWORD = ''
                                                          #       - EMAIL_HOST_USER = ''
                                                          #       - EMAIL_PORT = 25
                                                          #       - EMAIL_USE_TLS = False
```

#### Usage

Add `Stouts.sentry` to your roles and set vars in your playbook file.

Example:

```yaml

- hosts: all

  roles:
  - Stouts.foundation
  - Stouts.postfix
  - Stouts.sentry

  vars:
    postfix_smtp_sasl_user: "username@gmail.com"
    postfix_smtp_sasl_password: "password"

```

#### License

Licensed under the MIT License. See the LICENSE file for details.

#### Feedback, bug-reports, requests, ...

Are [welcome](https://github.com/Stouts/Stouts.sentry/issues)!
