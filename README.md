## :notebook_with_decorative_cover: Dockerize  a Symfony 5 Project

## Installation of the project

 _**You can found the context and the description of the project after the installation part below**_


# What do you need :
 
- :whale: Docker

# Clone the project

You can clone the project into a directory
`git clone https://github.com/borin92/TP_DOCKER.git
cd TP_DOCKER`

We will need fiew folder for the project just run the .sh file like this  `./makeFolder.sh`


# .env files

You need to create a .env.local file at the root of the Symfony project

`cd project 
touch .env.local
`
then you need to add this line :  DATABASE_URL="mysql://MYSQL_USER:MYSQL_PASSWORD@db:3306/MYSQL_DATABASE?serverVersion=5.7"

this will be the connection string to MY_SQL. In the string you can see "MYSQL_USER/MYSQL_PASSWORD..."
For this you need to fill up the .env.sample file at the root of the repo and make them match with the element in the connection string. If you on a local machine you can put what ever you want the important thing is that they need to match your connection string. Dont forget here we talk about the value for exemple in the .env file you have MYSQL_USER = foo you will put **foo** in the connection string as MYSQL_USER.


# launch the container

Next thing to do is to run your container with the docker compose file.

For this you just need docker install on your computer, then navigate to the root of the repo where you can see the docker-compose.yaml file.

Then just launch: `docker-compose up` 

Lot's of logs will appear but it's ok just wait until you see this line :  Version: '5.7.35'  socket: '/var/run/mysqld/mysqld.sock'  port: 3306  MySQL Community Server (GPL) --- This mean everything it's set up and ready to use

Your containers are now running we need a fiew more steps to do so we can get the front page in the navigator

# Build the project with composer

You dont need composer install on your computer, that the whole point of using docker in the project.

First we need to go in the container where apache is running. The container build with the dockerfile store in the src folder.
We are not going to describe what's in it we only need to know the name of the container present in the docker-compose.yaml
and here its www_docker_symfony.

So to enter the container when he is running just type : `docker exec -it www_docker_symfony bash`

if everything is right the path in the terminal will now be /var/www so we need to go in project : `cd project` and `composer install`

This operation may take some time it will install dependancies.

# Make sure the database is sync with the schema

The last step is now to check the databse and the schema.

php has a command to do this. In the project folder run : `php bin/console doctrine:schema:update --dump-sql`
There is two scenarios here 
 

- You have a green message good job you dont need to do anythings

- You have a SQL request function sample as message with a create table : you just need to run this `php bin/console doctrine:schema:update --force`

# Final

You now have everything set up just go to your navigator and look for http://localhost:8741/product/

The database can also be found at http://localhost:8080 this will provide you phpMyAdmin.

# Pre-prod and Pro environment

To switch from prod env and pre-prod env you just need to go in the .env file in the root of the symfony project there is a field named : APP_ENV if dev it's a preprod environment if it prod will be a prod environment.

## Objectifs

- [ ] Create a Symfony project with 1 simple CRUD or Use an existing Symfony Project
- [ ] Create a development environment for the project
- [ ] Create a preprod environment for the project

## :red_circle: Obligations
- :octocat: Github Flow (PR mandatory)
- :open_book: Respect Github open source guidelines
- :wavy_dash: Comment each line of the Dockerfile or docker-compose files
- :100: Complete README  to initialize the project
- :lock: Use only official Docker Hub images and lock versions of your images
- :no_entry: Mandatory services :
	- Php
	- Composer
	- Database
	- Apache or Nginx
- :envelope_with_arrow: Push your image on Docker Hub or Github (follow the mandatory rules for each platform)

## :gift: Bonus:
- :sob: Add a command to launch your tests
- :see_no_evil: Create 3 tests for the CRUD
- :outbox_tray: Create a prod environment for the project
- :scream: Push your docker on a vps or on heroku 
- :muscle: Services :
	- Testing email service (MailHog)
	- Proxy (Traefik)
	- SSL certificat (Let's Encrypt) :warning: You need a domain name
