# Dockerized PHP Environment with Nginx, MySQL/PostgreSQL, Redis, and Composer

This repository sets up a development environment using Docker for a PHP application with Nginx, MySQL or PostgreSQL, Redis, and Composer. This environment is suitable for developing applications like Laravel and Moodle.

## Prerequisites

- Docker
- Docker Compose

## Getting Started

### 1. Clone the Repository

```sh
git clone https://github.com/your-repository.git
cd your-repository
```

### 2. Copy the `.env.example` to `.env`
```sh
cp .env.example .env
```

### 3. Customize the .env File
Update the .env file with your preferred settings. You can set PHP version, database configuration, and PHP extensions as needed.

### 4. Build and Run the Containers
```sh
docker compose up --build
```
This command will build the Docker images and start the containers for Nginx, PHP, MySQL/PostgreSQL, and Redis.

## Directory Structure
```bash
project-root/
├── bin/
│   ├── php/
│   │   └── Dockerfile
├── composer/
├── config/
│   ├── nginx/
│   │   └── default.conf
│   ├── php/
│   │   └── php.ini
├── data/
│   ├── mysql/
│   └── postgres/
├── www/
│   ├── index.php
│   ├── composer.json
│   └── composer.lock
├── .env.example
├── .env
├── .gitignore
├── .dockerignore
└── docker-compose.yml
```

## Running Composer
### Install Dependencies
To install the dependencies defined in composer.json, run:
```sh
docker-compose run --rm php composer install
```
### Update Dependencies
To update the dependencies, run:
```sh
docker-compose run --rm php composer update
```

## Switching Between MySQL and PostgreSQL
By default, the **`.env`** file is configured to use MySQL. To switch to PostgreSQL:

1. Change the **`DB_CONNECTION`** variable in the **`.env`** file to **`postgres`**.
2. Change the **`DB_HOST`** variable to **`postgres`**.
3. Change the **`DB_PORT`** variable to **`5432`**.
4. Comment out or delete the MySQL service definition in **`docker-compose.yml`**.
5. Uncomment or add the PostgreSQL service definition in **`docker-compose.yml`**.

Example **`.env`** for **`PostgreSQL`**
```env
DB_CONNECTION=postgres
DB_HOST=postgres
DB_PORT=5432
DB_DATABASE=my_database
DB_USERNAME=user
DB_PASSWORD=password

POSTGRES_DB=my_database
POSTGRES_USER=user
POSTGRES_PASSWORD=password
```

Example **`.env`** for **`MySQL`**
```env
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=my_database
DB_USERNAME=user
DB_PASSWORD=password

MYSQL_ROOT_PASSWORD=root
MYSQL_DATABASE=my_database
MYSQL_USER=user
MYSQL_PASSWORD=password
```

## Customizing PHP Configuration
The PHP configuration file is located at **`config/php/php.ini`**. You can modify this file to change PHP settings such as memory limit, upload size, etc.

## Nginx Configuration
The Nginx configuration file is located at **`config/nginx/default.conf`**. You can modify this file to change the server settings as needed.

## Adding HTTPS Support

To enable HTTPS support using Let's Encrypt, follow these steps:

### 1. Update the .env File

Open the `.env` file and set `ENABLE_HTTPS=true`. Make sure to also set `LETSENCRYPT_DOMAIN` to your domain name and `LETSENCRYPT_EMAIL` to your email address.

Example:
```env
ENABLE_HTTPS=true
LETSENCRYPT_DOMAIN=yourdomain.com
LETSENCRYPT_EMAIL=your-email@example.com
```

### 2. Update nginx and docker-compose configuration
Uncoment https configuration section in **`config/nginx/default.conf`**
Uncoment https port in nginx section **`docker-compose.yml`**

### 3. Build and Run the Containers
Run the following command to build and start the containers:
```sh
docker-compose up --build
```
Certbot will automatically generate Let's Encrypt certificates for the specified domain. Nginx will be configured to use these certificates to enable HTTPS.

## Data Persistence
The database data is persisted in the data directory:
- MySQL data is stored in data/mysql.
- PostgreSQL data is stored in data/postgres.

## License
This project is licensed under the MIT License. See the LICENSE file for details.

## Contributing
Contributions are welcome! Please open an issue or submit a pull request for any improvements or bug fixes.

## Author
This project is maintained by **Betuah Anugerah**.
