# CMS200 Resources

## Add a new WordPress instance for Assessment 3

### 1. Create a new database and user

Log in to phpMyAdmin as `root`, open the **SQL** tab, and run:

```sql
CREATE DATABASE wordpress_assessment;

CREATE USER 'wordpress_assessment'@'%' IDENTIFIED BY 'wordpress_assessment';

GRANT ALL PRIVILEGES ON wordpress_assessment.* 
TO 'wordpress_assessment'@'%';

FLUSH PRIVILEGES;
```

### 2. Open your **docker-compose.yaml** add a new WordPress service
```
services:
  ...
  wordpress_assessment:
    image: wordpress:6.5-apache
    restart: unless-stopped
    depends_on:
      - db
    ports:
      - "8083:80"
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress_assessment
      WORDPRESS_DB_PASSWORD: wordpress_assessment
      WORDPRESS_DB_NAME: wordpress_assessment
      WORDPRESS_CONFIG_EXTRA: |
        define('DISABLE_WP_CRON', true);
    volumes:
      - ./wordpress_assessment_data:/var/www/html
      - ./php.ini:/usr/local/etc/php/conf.d/custom.ini:ro
    logging:
      driver: json-file
      options:
        max-size: "10m"
        max-file: "3"
```

### 3. Re-run docker-compose
1. `cd` to where your **docker-compose.yaml** file resides (alongside what we've been working on)
2. then `docker-compose down`
3. then `docker-compose up -d`


### 4. Install WordPress
You now have a new WordPress instance running on `localhost:8083` with its own database.

