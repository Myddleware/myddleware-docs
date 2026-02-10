# Administration tasks

> Myddleware comes with a set of extra admin features that are not activated by default. If you would like to enable these features for your Myddleware user, you will need to promote your user to the Super Admin role.

## Add users

In your Myddleware root directory, type the following command in a terminal:

```bash
php bin/console myddleware:add-user
```

To add a user with the role Super Admin by default, simply type :

```bash
php bin/console myddleware:add-user --superadmin
```

![Add Myddleware User command prompt](images/add_user_command.png)

## Promote an existing user

Some advanced Myddleware features such as cancelling or deleting all documents from a rule are restricted to Super Admin users. To enable these privileges for a Myddleware user, once in the myddleware directory, type the following command :

```bash
php bin/console myddleware:promote-user <email> ROLE_SUPER_ADMIN
```

Or simply type this command, a command prompt will assist you:

```bash
php bin/console myddleware:promote-user
```

Type the user’s email address, press Enter then type ROLE_SUPER_ADMIN and press Enter again.

![Promote Myddleware User command prompt](images/promote_user_command.png)

## Demote a user

If you wish to remove some special roles from a user privileges (such as ROLE_SUPER_ADMIN), use the following command :

```bash
php bin/console myddleware:demote-user
```

![Demote Myddleware User command prompt](images/demote_user_command.png)

## Upgrading Myddleware

### Back up your current Myddleware install

Before doing anything else, create a backup of your Myddleware instance before updating.
Before launching this upgrade procedure, create a backup of all your Myddleware files in a safe spot.

### Upgrade

#### Technical requirements

##### Upgrade PHP

!> For security & compatibility reasons, please make sure your PHP version is 7.4+. Myddleware 3 is compatible with PHP 7.4 & 8.0, but not 8.1. However, Myddleware 4 will be compatible with PHP 8.0 & 8.1.

The following PHP extensions must be installed & enabled (they usually are by default):

- Ctype
- Iconv
- JSON
- PCRE
- Session
- SimpleXML
- Tokenizer

##### Upgrade Composer

You also need to upgrade Composer to Composer 2.x : https://getcomposer.org/download/
This can also potentially be achieved by running the following command in your current Myddleware directory:

````
composer self-update
````

##### Symfony CLI

You need to have the Symfony CLI installed as well. You can download the appropriate version [here](https://symfony.com/download). 

#### Check your server meets new requirements

Thanks to the Symfony CLI, you can check whether your server meets all the requirements to run Myddleware 3.

````
symfony check:requirements
````

Read the prompt and if needed, follow the instructions to install missing extensions or configurations.

#### Install yarn

Since Myddleware 3, JavaScript & CSS assets are now handled using Webpack Encore. In order to build your assets, you will now 
need to install [yarn](https://yarnpkg.com/getting-started/install#nodejs-1610-1) package manager (https://classic.yarnpkg.com/lang/en/docs/install/#windows-stable), which itself requires [Node.js version 14+]( https://nodejs.org/en/download/)

#### Stop all scheduled tasks

If you use linux, comment the line that runs Myddleware in your crontab. If you use Windows, stop Myddleware tasks in the job scheduler.

#### Clear cache files

````
php bin/console cache:clear --env=prod
php bin/console cache:clear --env=background
````

> Alternatively, if you encountered issues with this command, you can try to run ````rm -rf var/cache/*```` instead or manually delete the /myddleware/var/cache directory's content.

### Init & fetch from GitHub

We strongly recommend that you use git to ensure the upgrade process is smoother.
If you don’t have git on your server, [here are the instructions on how to install it](https://git-scm.com/download/linux).
If you've never used git with Myddleware, please run these commands from your Myddleware root directory:

```git
git init
git remote add -t main origin https://github.com/Myddleware/myddleware.git
git fetch
git checkout origin/main -ft
```

#### Automatic Myddleware upgrade

You can upgrade Myddleware to its latest version with this command, which will run a series of jobs in the background :

```
php bin/console myddleware:upgrade --env=background
```

### Upgrade (alternative)

If you encountered an issue during the upgrade you can do it step by step by following this tutorial instead.
This procedure details how to upgrade from Myddleware 2.x to Myddleware 3.x. 
Throughout this process, the core software of Myddleware will be upgraded from Symfony 3.4 to Symfony 4.4, which is the engine that allows Myddleware to run, however we will also upgrade Myddleware’s code itself


#### Fetch from GitHub

```git
git pull
```

**TODO: this section is still under construction**

If you get an error message below after trying to pull, you might have changed at least one file in the Myddleware standard code. 
Please refer to ``Ensuring your custom code is upgrade-safe in Myddleware``  in the **Developer's guide** section of this doc. It will help you manage conflicts & transferring your custom code safely. 
You can also delete these files, run ```git pull``` again and you will get the latest version of these files. However, if you do, you will probably lose your custom code & files.


#### Upgrade PHP dependencies

```
composer install
```

#### Environment variables

If it's not there yet, you need to create a .env.local file at the root of your myddleware subdirectory. 
This file will override the configuration defined in the .env file. 
Inside this file, add the following lines and put your database parameters that you can find in the file myddleware\app\config\parameters.yml of your Myddleware 2 instance :
Copy the secret from your old myddleware\app\config\parameters.yml and paste it there too.

```
DATABASE_URL= "mysql://username:password@host:port/dbname"
APP_ENV=prod
APP_DEBUG=false
APP_SECRET=<your secret from Myddleware2> 
```


#### (Optional) Import custom code

**This section is still under construction**

If you had custom code in Myddleware 2, coppy & paste your custom code.

#### Synchronise Myddleware database

````
php bin/console doctrine:schema:update --force --env=background
````

#### Synchronise Myddleware config inside the database

````
php bin/console doctrine:fixtures:load --append --env=background
````

#### Upgrade JavaScript libraries

!> If you do not have [yarn](https://yarnpkg.com/getting-started/install#nodejs-1610-1) package manager installed on your server, please do so as it is now required in Myddleware 3+.

Run the following command to update your JavaScript libraries.

````
yarn install
````

#### Build for prod

Once that's done, you now need to build your Myddleware instance for production using the following command : 

```
yarn build 
```

## Admin Rights

| Action                | Definition                                                                                       | User | Admin | Super Admin |
|-----------------------|--------------------------------------------------------------------------------------------------|------|-------|-------------|
| Delete the logs       | Delete the logs of your current environment, to make debugging easier                           |      |       | ✔           |
| Cancel a document     | In the document view section, a super admin can force a document to be canceled. The button is hidden if the user doesn't have the required rights. | | | ✔         |
| Rule detail commands  | In the rule detail view section, a super admin can cancel all documents at once, delete all documents, or delete the rule itself. Note that all documents must be deleted before deleting the rule. The buttons are hidden if the user doesn't have the required rights. | | | ✔       |
| Open a document       | Open a document for viewing                                                                      | ✔    | ✔     | ✔           |
| Delete a document     | Delete the selected document                                                               | ✔    | ✔     | ✔           |
| Cancel a document     | Cancel a document and change its status to "Cancel"                                                          | ✔    | ✔     | ✔           |
| View a rule           | Open a rule and see its information                                                                          | ✔    | ✔     | ✔           |
| Edit a rule         | Change important properties of a rule such as the fields or the relationships                                                                           | ✔    | ✔     | ✔           |
| Run a rule         | Launch the rule for all the documents based on the reference                                                                           | ✔    | ✔     | ✔           |





### Delete the logs
![Delete the logs button](images/admin_tasks/delete_logs_button.png)

### Cancel a Document
![Cancel a document](images/admin_tasks/cancel_document.png)

### Rule detail commands
![Rule detail commands](images/admin_tasks/rule_tasks.png)

## Admin Panel

The admin panel is accessible from the top-right user dropdown menu. Click on the user icon in the navigation bar to reveal the admin menu.

![Admin dropdown menu](images/advanced_usage/admin/admin-button.png)

From this menu, you can access the following sections:

- **Users**: Manage users (see [User Management](#add-users) above)
- **Settings**: Configure system-wide settings (see [System Settings](#system-settings) below)
- **Crontab**: Manage scheduled tasks (see [Job scheduler & cron tasks](cron_jobs.md))
- **Logout**: Log out of your Myddleware session

### My Information

Clicking on your username in the dropdown menu will take you to the **My Information** page, where you can manage your account. This page is divided into three tabs: **General**, **Security**, and **Preferences**.

#### General

The General tab allows you to view and update your basic account details.

![My Information - General tab](images/advanced_usage/admin/admin-menu-top.png)

Available fields:

- **Username**: Your display name used to log in to Myddleware
- **Email**: Your email address, used for notifications and password resets
- **Language**: Select your preferred language for the Myddleware interface (English or French)

Click **Save** to apply your changes.

At the bottom of this page, you will also find the **Logs** section, which provides two actions:

![My Information - Logs section](images/advanced_usage/admin/admin-bottom.png)

- **Download Latest Logs**: Download the latest application log file for debugging purposes
- **Empty Log File**: Clear the log file to free up space (requires Super Admin privileges)

#### Security

The Security tab allows you to manage your password and configure Two-Factor Authentication.

##### Password

![My Information - Security tab (Password)](images/advanced_usage/admin/security-password.png)

To change your password:

1. Enter your **Current password**
2. Enter your **New password**
3. Confirm it in the **Repeat new password** field
4. Click **Update Password**

##### Two-Factor Authentication

![My Information - Security tab (2FA)](images/advanced_usage/admin/security-2fa.png)

Myddleware supports email-based Two-Factor Authentication (2FA). When enabled, you will be asked to enter a verification code sent to your email address after logging in.

!> Two-Factor Authentication requires email configuration. You must configure either SMTP settings or a Brevo API key first. See the [System Settings](#system-settings) section below.

To enable 2FA, toggle the **Enable Two-Factor Authentication** switch.

#### Preferences

The Preferences tab allows you to customize display and export formats.

![My Information - Preferences tab](images/advanced_usage/admin/admin-preferences-tab.png)

Available settings:

- **Time Zone**: Set your preferred timezone (e.g., UTC, Europe/Paris, America/New_York). This affects how dates are displayed throughout the application.
- **Date Format**: Choose your preferred date format (e.g., DD/MM/YYYY, YYYY-MM-DD, MM/DD/YYYY).
- **Export Separator**: Select the separator used when exporting data to CSV files. Options include Semicolon (;), Comma (,), Tab, and Pipe (|).
- **Charset**: Choose the character encoding for exports (UTF-8, ISO-8859-1, or Windows-1252).

Click **Save** to apply your preferences.

---

## System Settings

The System Settings page is accessible from the admin dropdown menu by clicking **Settings**. It provides global configuration options that apply to the entire Myddleware instance. The page is divided into two tabs: **Preferences** and **SMTP Management**.

### Preferences

The Preferences tab allows administrators to configure table display settings that affect the entire application.

![System Settings - Preferences tab](images/advanced_usage/system-settings/system-settings-preferences.png)

Available settings:

- **Rows per page**: The number of rows displayed per page in data tables across the application (e.g., rules list, documents list). Default: `100`.
- **Maximum results**: The maximum number of results returned by search queries. Default: `100000000`. Lowering this value can improve performance on large datasets.

Click **Save** to apply the changes.

### SMTP Management

The SMTP Management tab allows you to configure email settings for Myddleware. Email configuration is required for features such as password resets, Two-Factor Authentication, and workflow notifications.

![System Settings - SMTP Management tab](images/advanced_usage/system-settings/system-settings-smtp-config.png)

#### Transport

Select your email delivery method from the **Transport** dropdown:

- **SMTP**: Standard SMTP server (requires host, port, username, password, encryption, and authentication mode)
- **Gmail**: Google SMTP (requires an app-specific password)
- **Sendmail**: Uses the server's local sendmail service (no additional configuration needed)
- **Sendinblue**: Brevo/Sendinblue API-based email delivery (requires an API key)

#### Configuration Fields

Depending on the selected transport, the following fields may appear:

| Field | Description | Used by |
|-------|-------------|---------|
| Host | Mail server address (e.g., smtp.example.com) | SMTP |
| Port | Server port (typically 25, 465, or 587) | SMTP |
| User | SMTP username | SMTP, Gmail |
| Password | SMTP password or app-specific password | SMTP, Gmail |
| Encryption | TLS or SSL | SMTP, Gmail |
| Auth Mode | Authentication method: Plain, Login, CRAM-MD5, or OAuth | SMTP |
| API Key | Brevo/Sendinblue API key | Sendinblue |
| **Sender** | **The "From" email address for all outgoing emails (required for all transports)** | **All** |

#### Actions

- **Save SMTP config**: Save the current email configuration
- **Send test mail**: Send a test email to your registered email address to verify the configuration
- **Cancel**: Discard changes and return to the main panel