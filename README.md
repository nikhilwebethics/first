# ![drawing](https://prosal.io/img/logo_gray.png)

# PROSAL

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

A Prosal is a modern solution to the request for proposals (RFPs) Simply put, a Prosal is our take on the RFP - a standardized, predictable document optimized for readability, and responses. We have summarized the complexity of the RFP into a card, so that you know exactly what you’re looking at the very first time you see it.

When it comes to RFPs, the goal has always been simply stated, but less simply achieved: define a need or challenge, publish a request for consultants and freelancers to compete, and select the right vendor for the job. The RFP process is outdated and ineffective. It is often a closed undertaking that produces limited outreach and an even more limited number of respondents, disproportionately affecting firms led by women and people of color. It leads to time delays, increased and unexpected costs, and unsatisfactory project deliverables. Both issuers and respondents stand to benefit from a new solution.

## Installation
1. Install the dependencies and devDependencies and start the server.
    1. `git clone: https://nylemalik@github.com/nylemalik/prosal.git` 
    2. `git branch`
    3. `git checkout develop`
    4. `git pull origin develop`

 
2. Install docker and docker compose
3. From the root of the project
   ` Run : docker-compose up -d --build`
4. Add this file under this path : nginx\conf\app.conf  
   Add this data in this file :
   ```sh
   server {
   listen 80;
   index index.php index.html;
   error_log /var/log/nginx/error.log;
   access_log /var/log/nginx/access.log;
   client_max_body_size 100M;
   root /var/www/public;
   location ~ \.php$ {
   try_files $uri =404;
   fastcgi_split_path_info ^(.+\.php)(/.+)$;
   fastcgi_pass app:9000;
   fastcgi_index index.php;
   include fastcgi_params;
   fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
   fastcgi_param PATH_INFO $fastcgi_path_info;
   }
   location / {
   try_files $uri $uri/ /index.php?$query_string;
   gzip_static on;
   }
   }
   ```
5. `sudo docker exec nginx nginx -s reload`
6. Check docker container
   `docker-compose ps `
7. Run command inside the docker container
   1. `docker exec -it app /bin/bash    (app is container, You need to run this command then run all command that is below)`
   2. `composer install --ignore-platform-reqs`
   3. `composer update`
   4. `npm install`
   5. `php artisan optimize`
   6. `npm run dev`


## BACKUP FROM ATLAS LIVE DB

1. docker exec -it mongodb /bin/bash (this command take you inside the continer then you run the below command)
   ```sh
   mongodump --uri="mongodb+srv://URL" --out "path_to_store_mongodb"   (this is live site url,so csrfully run the commands)
   docker cp mongodb:path_to_store_mongodb/prosal  path_to_store_db_from_docker
   ```
2. Import MONGODB databse to atlas DB

```sh
   mongorestore --uri mongoatlas_uri/prosal  path_from_where_you_import_db
```
   If all is fine then your site is working fine. Then you can open your site on localhost :  [http://localhost:8100](http://localhost:8100)

## Steps to deploy changes to production.:

Let suppose you have made change in the file and you have checked all is fine and you are working in the deveop branch directly.
Then you need follow the steps to deploy you changes :
   1. `git add 'you need put file name'`
   2. `git commit -m 'Enter commit message'`
   3. `git push origin develop`

After that you need to connect the live server ssh

Connect to ssh and then run below command

#### Purge cache from Cloudfare

1. `sudo su`
2. `cd /var/html/prosal`
3. `git pull origin develop  (if issue in the pull then run these 2 command)`<br>
  i)`git stash`<br>
  ii)`git pull origin develop`
4. `docker exec -it app /bin/bash`
5. `rm -rf node_modules`
6. `rm -rf package-lock.json`
7. `cd public/js`
8. `rm -rf index.js`
9. `cd ..`

#### When You are on root directory of project inside the container then run below command.
1. `npm install`
2. `php artisan optimize`
3. `npm run dev`

These are the steps to deploy changes to production.
