[![Rate at redmine.org](https://img.shields.io/badge/rate%20at-redmine.org-blue.svg?style=flat)](http://www.redmine.org/plugins/omniauth_oauth2_isu)

> This plugin is no longer maintained. Please, feel free to fork and update :)

## Redmine omniauth OAuth2 / CAS / ISU

This plugin is used to authenticate Redmine users using [CAS OAuth2](https://apereo.github.io/cas/5.0.x/installation/OAuth-OpenId-Authentication.html) provider ("Authorization Code" grant type).
It is primarily intended to work with [ITMO university](http://www.ifmo.ru) ISU system [https://isu.ifmo.ru](https://isu.ifmo.ru).

Version of [CAS](https://apereo.github.io/cas): 5.0

Version of [Redmine](http://www.redmine.org/): 3.2.3 (as of publish date, other versions should work as well), 3.3, 3.3.2

### Features

+ login
+ logout
+ automatic user creation

### Installation

1. Download the plugin and install required gems:

```console
cd /path/to/redmine/plugins
git clone https://github.com/pbelikov/redmine-omniauth-oauth-cas.git
mv redmine-omniauth-oauth-cas redmine_omniauth_isu
cd /path/to/redmine
bundle install
```

2. __IMPORTANT!__ Plugin is used to work without proxy and to override issues with SSL-certificate.
So, if you use proxy, please go to `app/controllers/redmine_oauth_controller.rb` and comment line 7 
(which disables proxy). And if your SSL is OK, go to the same file and comment code in line 5 and part of code in line 39.
Yes, I know that this is BAD codestyle, but it'll work for sure.

3. Restart the app
```console
touch /path/to/redmine/tmp/restart.txt
```

### Configuration

* Login as a user with administrative privileges. 
* In top menu select "Administration".
* Click "Plugins"
* In plugins list, click "Configure" in the row for "Redmine Omniauth ISU plugin"
* Enter CAS URL
* Enter the Сlient ID & Client Secret, which you entered for your CAS Service (see more [here](https://apereo.github.io/cas/5.0.x/installation/OAuth-OpenId-Authentication.html)).
* Check the box near "Oauth authentication"
* Click Apply. 
 
Users can now use their CASified Account to log in to your instance of Redmine.

### Authentication Workflow

1. An unauthenticated user requests the URL to your Redmine instance.
2. User clicks the "Login via ..." buton.
3. The plugin redirects them to a CAS sign in page if they are not already signed in to their CAS account.
4. CAS redirects user back to Redmine, where the CAS OAuth plugin's controller takes over.


### Profile format

User information in CAS `/cas/oauth2.0/profile` for successful login or creation of user has following format:

```
{
    "attributes": {
        "redmine_login":"ivan",
        "redmine_attrs":"Ivan|Ivanov|ivan@example.com"
    },
    "id": 123456789
}
```

### Additional info

This plugin overrides Redmine's autoregistration feature so user is created automatically if all required fields
are provided (login, firstname, lastname, email). Uniqueness of user is checked against login.

### Known issues

Unfortunately, this plugin somehow conflicts with another plugin, called "Redmine Wiki Extensions Plugin" by r-labs.

### Inspiration

This plugin is inspired by [twinslash](https://github.com/twinslash) plugin [Redmine omniauth google](https://github.com/twinslash/redmine_omniauth_google).

### Contribution

Please do not hesitatate to contribute to this project. As I know, there is still no official CAS OAuth2 (as client) support in Redmine, so maybe this plugin could help many people. Also I know that code of this plugin is far from good, maybe you could use your Ruby skills to make it better.
