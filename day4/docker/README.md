# Docker

* Single predictable dependencies (only docker).
* Safer environment variables with ECS/Elastic Beanstalk (no files going back and forward).
* Easy application management.
	* Start App? No worries about weird config, or if application uses `npm`/`yarn`/`rb`, just `docker run`.
	* Crash? Just restart container.
	* Update? Just pull new image and re-build.
* More efficient devleopment environment.
	* No need to manually set up RDBMS (Postgres, docker).
	* No need to deal with port/networking configuration.
	* Simulate complex networks between containers.
	* Fast and simple tear up of databases.

## Configuration

Always check [latest aws (or favorite provider) documentation](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/docker-basics.html).

It would usually just be:

```
sudo yum update -y
sudo yum install docker
sudo service docker start
sudo usermod -a -G docker ec2-user // so you can execute commands without sudo
docker info // verify
```

## No need for lots of changes...

From the development aspect, there's not that many changes to 'dockerize' a project. Just a set a `Dockerfile` in place, or `docker-compose.yml` if needed.

### Node, easy

```dockerfile
FROM node:12.16.2

WORKDIR /usr/app

# Install dependencies
COPY ./package.json ./
RUN npm install --only=prod

# Copy everything else
COPY ./ ./

# Deployment port
EXPOSE 80
EXPOSE 443

# Default commands
CMD ["npm", "run", "prod:start"]
```

### Local Database

```yaml
version: "3"
services:
  db:
    container_name: "database"
    restart: always
    build:
      context: ./db
      dockerfile: Dockerfile
    ports:
      - "5432:5432"
    #volumes:
    #  - ./postgres-data:/var/lib/postgresql/data
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DB=mydb
  server:
    container_name: "server"
    restart: always
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "80:80"
#    Volumes conflict with coverage files from nyc
    volumes:
      - ./src:/usr/app/src
    env_file:
      - '.env.local'
    depends_on:
      - db
```