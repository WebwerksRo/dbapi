- [dbAPI: Instant REST API for MySQL databases](#dbapi-instant-rest-api-for-mysql-databases)
  - [Features](#features)
  - [How it works](#how-it-works)
  - [Getting Started](#getting-started)
    - [Prerequisites](#prerequisites)
    - [Web server configuration](#web-server-configuration)
    - [Install](#install)
    - [Configure](#configure)
  - [Generate an API for an existing database](#generate-an-api-for-an-existing-database)
  - [Using the API](#using-the-api)
  - [Contributing](#contributing)
  - [License](#license)

# dbAPI: Instant REST API for MySQL databases

dbAPI is a middleware that instantly creates a REST API for any Mysql database  by dynamically adapting to the database schema, accelerating backend development exponentially compared to traditional API building methods

With dbAPI, developers can quickly and easily generate clean, well-documented APIs that are tailored to their database schema. This automation basically eliminates the need to develop the database backend, allowing developers to focus on other aspects of their projects.


## Features

- automatic API endpoints creation for CRUD operations based on the MySQL database schema
- implements [JSON:API](https://jsonapi.org/) specification to allow recursive retrieval, creation and update of linked resources 
- automatic API documentation  generation in OpenAPI format
- embeded Swagger UI to explore the API and test the endpoints
- authentication: exposes authentication endpoints which can be customized by defining the SQL query to match the user credentials.
- authorization: ACLs based on logged in user 
- error handling: provides informative error messages in case of invalid requests, server errors, or other exceptional conditions.
- scalability: being stateless, it can horizontally scale to any number of instances. 
- security: implements security best practices to protect against common threats such as injection attacks, cross-site scripting (XSS), and cross-site request forgery (CSRF). This involves input validation, parameterized queries, and encryption of sensitive data.
- multi-database: one dbAPI instance can be used to connect to multiple databases, each with their separate entry endpoint, access and authorization rules. This feature can be used to create multiple versions of the same API for different environments (dev, test, prod)
- configuration API: configuration of the APIs is performed through a dedicated API where new databases/APIs can be added, update their configuration and delete them.
- instant update of the API structure when the database struncture changes
- support for MySQL, MariaDB and Percona Server
 
## How it works

dbAPI works by analyzing the database schema and automatically generating the API endpoints based on the table structure.

## Getting Started

### Prerequisites

- PHP 7.4 or higher 
- a web server configured to run PHP scripts (Apache, Nginx, etc.)
- PHP Extensions:
  - mysqli
  - json 
  - mbstring

### Web server configuration

For Apache, ensure mod_rewrite is enabled.

For Nginx, ensure the following directives are present in the configuration file:
```
location ~ /instalation_path/ {
    try_files $uri $uri/ /instalation_path/index.php?$args;
}
```


### Install

```shell
mkdir dbapi
cd dbapi
git clone https://github.com/vsergione/dbapi .
chmod 777 dbconfigs
```

### Configure
Edit ```installation_path/application/config/dbAPI.php``` and update the configuration to suit your needs (the file is decently documented and you should be able to figure out the rest). You should at least update the following variables:

- ```$config['config_api_secret']```: the secret to be used for authenticating config API requests.
- ```$config['base_url']```: the base URL of the API.
- ```$config['configs_dir']```: the folder where the API configurations are stored.



## Generate an API for an existing database

Creating your first API from a preexisting MySQL database is as easy as making a POST request using your favorite HTTP client (ours is Postman ;) )to the  configuration API ```http(s)://hostname/installation_path/apis``` 

Example using CURL:
```shell
curl --location 'http://localhost/dbapi/apis' \
--header 'Content-Type: application/json' \
--header 'x-api-key: myverysecuresecret' \
--data '{
    "name":"demo",
    "connection":{
        "hostname": "db_host",
        "username": "db_user",
        "password": "db_pass",
        "database": "db_name"
    }
}'

{"result":"f64e0cb3-f2ff-4c1d-a9a4-1ebf22272a96"}
```

And that's it! Your API is now ready to use... but not for production! 

Do not forget to save the API secret which is being returned in the response. Please check the [API configuration](docs/configuration_api.md) for more details.


## Using the API
 
The easiest way to get started with the API is to use Swagger UI which is bundled within the installation and can be accessed at ```http(s)://hostname/installation_path/swagger.html?api=api_name```.

Please refer to the [Using the API](docs/using_the_api.md) documentation for more details.



## Contributing

We welcome contributions to dbAPI! Please feel free to submit a pull request or open an issue.


## License

dbAPI is licensed under the MIT License. See the [LICENSE](LICENSE.md) file for details.
