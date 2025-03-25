# CMS200 Resources

## Add a new WordPress instance for Assessment 3

1. Log in to phpMyAdmin (as root), and use the SQL tab to create a new database and user:
```
CREATE DATABASE wordpress_assessment;
CREATE USER 'wordpress_assessment'@'%' IDENTIFIED BY 'wordpress_assessment';
GRANT ALL PRIVILEGES ON wordpress_assessment.* TO 'wordpress_assessment'@'%';
FLUSH PRIVILEGES;
```

2. Open your **docker-compose.yaml** add a new WordPress service:
```
services:
 ...
  wordpress_assessment:
    image: wordpress:latest
    container_name: wordpress_assessment
    restart: always
    depends_on:
      - db
    ports:
      - "8083:80"
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: wordpress_assessment
      WORDPRESS_DB_PASSWORD: wordpress_assessment
      WORDPRESS_DB_NAME: wordpress_assessment
    volumes:
      - ./wordpress_assessment:/var/www/html
      - ./uploads.ini:/usr/local/etc/php/conf.d/uploads.ini
```

3. Now, open your terminal and --
   - `cd` to where your **docker-compose.yaml** file resides (alongside what we've been working on)
   - then `docker-compose down`
   - then `docker-compose up -d`

4. You now have a new WordPress instance running on `localhost:8083` with its own database (note: you'll need to run through a fresh installation).
