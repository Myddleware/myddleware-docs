# Technical Requirements

Myddleware can be installed using [Docker](https://www.docker.com/) for a complete development and deployment environment.

To use Myddleware you need the following on your web server :

- A web server such as [Apache](https://httpd.apache.org/)
- [MySQL](https://www.mysql.com/downloads/) version 8.4 or above or [MariaDB](https://mariadb.org/download/)

| Myddleware versions  | [PHP version](https://www.php.net/downloads.php) | [Node.js version](https://nodejs.org/de/download/) | [MySQL version](https://www.mysql.com/downloads/) |
|----------------------|--------------------------------------------------|----------------------------------------------------|---------------------------------------------------|
| 3.2.0 and below      | 7.4 or 8.0                                       | 14, 15 and 16                                      | 5.7 or above                                      |
| 3.3.0                | 8.1 or 8.2.0                                     | 16 or above                                        | 5.7 or above                                      |
| 4.2.0 and above      | 8.1, 8.2, or 8.3                                 | 20 or above                                        | 8.4 or above                                      |

- The following PHP extensions need to be installed & enabled (they usually are by default):
  - Ctype
  - Iconv
  - JSON
  - PCRE
  - Session
  - SimpleXML
  - Tokenizer
- [composer](https://getcomposer.org/download/)
- the [Symfony CLI](https://symfony.com/download)
- [yarn](https://yarnpkg.com/getting-started/install) version [1.22.17](https://classic.yarnpkg.com/lang/en/docs/install/#windows-stable ) or above

Myddleware uses [Doctrine ORM](https://www.doctrine-project.org/projects/doctrine-orm/en/2.11/tutorials/getting-started.html#getting-started-with-doctrine) so you will need to have the PDO driver installed for the database server you intend to use.

It is possible that depending on your webserver configuration there might be some missing requirements. We strongly recommend running the following command in a terminal to ensure all requirements are met :

``` symfony check:requirements ```
