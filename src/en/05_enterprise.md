# Enterprise

## Installation

Pull the latest PgManage Docker image
```
docker pull cmdpromptinc/pgmanage-enterprise:latest
```

## Prerequisites
Before running PgManage docker container create the data and certificate directories on the host machine (referred to as /private/appdata and /private/appdata in the following examples) and the directory its ownership to UID 900 GID 0 (these are the user and group IDs pgmanage service runs as inside the container)
```
sudo chown -R 900:0 /private/appdata
sudo chown -R 900:0 /private/certs
```
Files in /private/appdata and /private/certs should be accessible by pgmanage process running as UID 900 inside the container.

## Examples

### Plain HTTP, unlincensed (simplest setup)
```
docker run -p80:80  -v /private/appdata:/appdata cmdpromptinc/pgmanage-enterprise:latest
```

### SSL Enabled, unlicensed
```
docker run -p443:443 -v /private/appdata:/appdata -v /private/certs:/certs -e PGMANAGE_SSL_ENABLED=True -e PGMANAGE_SSL_CERT=/certs/test.pem -e PGMANAGE_SSL_KEY=/certs/test.key cmdpromptinc/pgmanage-enterprise:latest
```

> ℹ️ this example assumes that you have valid SSL certificate and key files specified in PGMANAGE_SSL_CERT PGMANAGE_SSL_KEY variables.

### SSL Enabled with enterprise feautures
```
docker run -p443:443 -v /private/appdata:/appdata -v /private/certs:/certs -e PGMANAGE_SSL_ENABLED=True -e PGMANAGE_SSL_CERT=/certs/test.pem -e PGMANAGE_SSL_KEY=/certs/test.key -e PGMANAGE_LICENSE_KEY=XXXXXX-XXXXXX-XXXXXX-XXXXXX-XXXXXX-XX cmdpromptinc/pgmanage-enterprise:latest
```
> ℹ️ this example assumes that you have a valid SSL certificate and key files specified in PGMANAGE_SSL_CERT PGMANAGE_SSL_KEY variables.
> ℹ️ this example assumes that you have a valid license file stored in /appdata/pge-license.lic file.  
> ℹ️ set PGMANAGE_LICENSE_KEY to the actual value provided with your PgManage Enterprise license

### With reverse-proxy and enterprise features
It is possible to run PgManage behind a reverse proxy. The following example shows how to set up PgManage in a combination with Nginx
```
docker pull cmdpromptinc/pgmanage-enterprise:latest
docker run -p8080:80 -v /private/appdata:/appdata -v /private/certs:/certs -e PGMANAGE_LICENSE_KEY=XXXXXX-XXXXXX-XXXXXX-XXXXXX-XXXXXX-XX cmdpromptinc/pgmanage-enterprise:latest
```

Nginx configuration:
```
{
    listen 80;
    server_name _;

    location / {
        proxy_set_header Host $host;
        proxy_pass http://localhost:8080/;
        proxy_redirect off;
    }
}
```


## Supported Environment Variables  

`PGMANAGE_LISTEN_ADDRESS`  
Default: [::]  
The address of the nework interface that pgmanage web service will listen for the connections.  

`PGMANAGE_LISTEN_PORT`  
Default: 80 (or 443 when SSL is enabled)  
The TCP port that pgmanage web service will listen for the connections.

`PGMANAGE_URL_PREFIX`  
Default: None which means that the app is seved from the root URI.
This configuration option allows to serve pgmanage from non-root URLs like https://myhost/pgmanage/

`PGMANAGE_SSL_ENABLED`  
Default: False  
If False or unset, pgmanage webservice will run in plain HTTP mode. In production mode this option is useful in scenarios when pgmanage is served via reverse proxy which handles SSL/TLS.  

> ⚠️ Don't run pgmanage via plain HTTP in standalone mode in production environments.
If SSL enabled, PGMANAGE_SSL_CERT and PGMANAGE_SSL_KEY config options must also be set, pointing to the valid certificate and key files.

`PGMANAGE_SSL_CERT` 
Default: None  
Specifies path to SSL certificate file within the container, required if SSL is enabled.

`PGMANAGE_SSL_KEY`  
Default: None  
Specifies path to corresponding SSL certificate key within the container, required if SSL is enabled.

`PGMANAGE_SECURE_COOKIES` 
Default: False (True when SSL is enabled)  
A boolean flag which is used to enable http cookie secure flag, should be set to True if pgmanage runs over plain HTTP with an external SSL-enabled front-end webserver.

`PGMANAGE_DEFAULT_USERNAME`  
Default: admin  
The login name of the built-in admin user account to be created during the first startup of the application.

`PGMANAGE_DEFAULT_PASSWORD`  
Default: admin  
The password of the built-in admin user account to be created during the first startup of the application.

`PGMANAGE_OAUTH2_AUTO_CREATE_USER`  
Default: True  
Tells pgmanage to automatically create local user accounts when authenticating via OAuth2. If not enabled, any users to be authenticated via OAuth2 must be manually created locally by a pgmanage admin before they can log in through the OAuth2 provider.

`PGMANAGE_LICENSE_KEY`  
Defaut: None  
A key used to decrypt the license file. Must be present to enabled Enterprise features  

`PGMANAGE_LICENSE_PATH`  
Default: None  
A path to a license file within the container. Optional. By default pgmanage will use **/appdata/pge-license.lic** for the license file path.


## Configuration Overrides File
For some scenarios when passing complex configuration options via environment variables may be impractical. For such cases it is possible to define te configuration in **/appdata/override.py** file. OAuth2 Provider configuration is a good example of such use case.

## Setting up OAuth2 Authentication
First of all, the OAuth2 client must be registered on your OAuth2 provider. A valid application URL must be specified.
Obtain OAuth2 client id and client secret from the OAuth2 Client registration page and replace the corresponding placeholders in the following configuration example.

Here is a sample configuration using Google OAuth2 provider (contents of  **/appdata/override.py**):
```
OAUTH2_CONFIG = [
    {
        # The name of the of the oauth provider, ex: github, google
        'OAUTH2_NAME': 'Google',
        # Oauth client id
        'OAUTH2_CLIENT_ID': '<YOUR_OAUTH2_CLIENT_ID_HERE>',
        # Oauth secret
        'OAUTH2_CLIENT_SECRET': '<YOUR_OAUTH2_CLIENT_SECRET_HERE>',
        # The claim which is used for the username. If the value is empty the
        # email is used as username, but if a value is provided,
        # the claim has to exist.
        'OAUTH2_USERNAME_CLAIM': None,
        # URL is used for authentication,
        # Ex: https://accounts.google.com/o/oauth2/v2/auth
        'OAUTH2_AUTHORIZATION_URL': 'https://accounts.google.com/o/oauth2/v2/auth',
        # server metadata url might optional for your provider, ex: https://accounts.google.com/.well-known/openid-configuration
        'OAUTH2_SERVER_METADATA_URL': 'https://accounts.google.com/.well-known/openid-configuration',
        # Oauth base url, ex: https://www.googleapis.com/
        'OAUTH2_API_BASE_URL': 'https://www.googleapis.com/',
        # Oauth scope, ex: 'openid email profile'
        # Note that an 'email' claim is required in the resulting profile
        "OAUTH2_SCOPE": 'email',
        # The display name, ex: Google
        'OAUTH2_DISPLAY_NAME': 'Google',
        # Font-awesome icon, ex: 'fa-brands fa-github'
        'OAUTH2_ICON': 'fa-brands fa-google',
    },
]
```

Once PgManage container is restarted with the new configuration, its login screen should display the Oauth2 button.

> ℹ️ It is possible to configure multiple OAuth2 providers for a single PgManage instance, just define multiple configuration entries like so
```
OAUTH2_CONFIG = [
    {
        # The name of the of the oauth provider, ex: github, google
        'OAUTH2_NAME': 'Google',
        # Oauth client id
        'OAUTH2_CLIENT_ID': '<YOUR_OAUTH2_CLIENT_ID_HERE>',
        # Oauth secret
        'OAUTH2_CLIENT_SECRET': '<YOUR_OAUTH2_CLIENT_SECRET_HERE>',
        # The claim which is used for the username. If the value is empty the
        # email is used as username, but if a value is provided,
        # the claim has to exist.
        'OAUTH2_USERNAME_CLAIM': None,
        # URL is used for authentication,
        # Ex: https://accounts.google.com/o/oauth2/v2/auth
        'OAUTH2_AUTHORIZATION_URL': 'https://accounts.google.com/o/oauth2/v2/auth',
        # server metadata url might optional for your provider, ex: https://accounts.google.com/.well-known/openid-configuration
        'OAUTH2_SERVER_METADATA_URL': 'https://accounts.google.com/.well-known/openid-configuration',
        # Oauth base url, ex: https://www.googleapis.com/
        'OAUTH2_API_BASE_URL': 'https://www.googleapis.com/',
        # Oauth scope, ex: 'openid email profile'
        # Note that an 'email' claim is required in the resulting profile
        "OAUTH2_SCOPE": 'email',
        # The display name, ex: Google
        'OAUTH2_DISPLAY_NAME': 'Google',
        # Font-awesome icon, ex: 'fa-brands fa-github'
        'OAUTH2_ICON': 'fa-brands fa-google',
    },
    {
        # The name of the of the oauth provider, ex: github, google
        'OAUTH2_NAME': 'Another OAuth2 provider',
        # Oauth client id
        'OAUTH2_CLIENT_ID': '<YOUR_OAUTH2_CLIENT_ID_HERE>',
        # Oauth secret
        'OAUTH2_CLIENT_SECRET': '<YOUR_OAUTH2_CLIENT_SECRET_HERE>',
        # The claim which is used for the username. If the value is empty the
        # email is used as username, but if a value is provided,
        # the claim has to exist.
        'OAUTH2_USERNAME_CLAIM': None,
        # URL is used for authentication,
        # Ex: https://accounts.google.com/o/oauth2/v2/auth
        'OAUTH2_AUTHORIZATION_URL': 'https://accounts.google.com/o/oauth2/v2/auth',
        # server metadata url might optional for your provider, ex: https://accounts.google.com/.well-known/openid-configuration
        'OAUTH2_SERVER_METADATA_URL': 'https://accounts.google.com/.well-known/openid-configuration',
        # Oauth base url, ex: https://www.googleapis.com/
        'OAUTH2_API_BASE_URL': 'https://www.googleapis.com/',
        # Oauth scope, ex: 'openid email profile'
        # Note that an 'email' claim is required in the resulting profile
        "OAUTH2_SCOPE": 'email',
        # The display name, ex: Google
        'OAUTH2_DISPLAY_NAME': 'Google',
        # Font-awesome icon, ex: 'fa-brands fa-github'
        'OAUTH2_ICON': 'fa-brands fa-google',
    },
]
```