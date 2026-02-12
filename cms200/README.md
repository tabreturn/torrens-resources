# CMS200 Resources

## Add a new WordPress instance for Assessment 3

1. Log in to phpMyAdmin (as root), and use the SQL tab to create a new database and user:

```sql
CREATE DATABASE wordpress_assessment;
CREATE USER 'wordpress_assessment'@'%' IDENTIFIED BY 'wordpress_assessment';
GRANT ALL PRIVILEGES ON wordpress_assessment.* TO 'wordpress_assessment'@'%';
FLUSH PRIVILEGES;
```

2. Open your **docker-compose.yaml** and add a new WordPress service:

```yaml
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
      - ./wordpress_assessment/wp-content:/var/www/html/wp-content
      - ./uploads.ini:/usr/local/etc/php/conf.d/uploads.ini
```

Create this folder on your host:

```bash
mkdir -p wordpress_assessment/wp-content
```

3. Now, open your terminal and:

- `cd` to where your **docker-compose.yaml** file resides
- then run:

```bash
docker compose down
docker compose up -d
```

4. You now have a new WordPress instance running at:

```
http://localhost:8083
```

It has its own database and its own `wp-content` directory (you’ll need to complete a fresh WordPress installation).
