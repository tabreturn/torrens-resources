# CMS200 Resources

## Add a new WordPress instance for Assessment 3

1. Log in to phpMyAdmin (as root), and use the SQL tab to create a new database and user:
```
CREATE DATABASE final_assessment;
CREATE USER 'final_assessment'@'%' IDENTIFIED BY 'final_assessment';
GRANT ALL PRIVILEGES ON final_assessment.* TO 'final_assessment'@'%';
FLUSH PRIVILEGES;
```

2. Open your **docker-compose.yaml** add a new WordPress service:
```
services:
 ...
  wordpress_assessment:
    image: wordpress:latest
    container_name: final_assessment
    restart: always
    depends_on:
      - db
    ports:
      - "8083:80"
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: final_assessment
      WORDPRESS_DB_PASSWORD: final_assessment
      WORDPRESS_DB_NAME: final_assessment
    volumes:
      - ./wordpress_assessment:/var/www/html
      - ./uploads.ini:/usr/local/etc/php/conf.d/uploads.ini
```
3. Now, open your terminal and --
   - `cd` to where your **docker-compose.yaml** file resides (alongside what we've been working on)
   - then `docker-compose down`
   - then `docker-compose up`
