# Usage
- The `docker build` command builds an image from a `Dockerfile` and a _context_. 
- _context_ is the set of files at specific location `PATH` or `URL`. `PATH` is on local filesystem, `URL` is Git repository location. 
- First thing build process does is send the extire context (recursively) to the daemon. So add only the files needed for building the Dockerfile.
- `.dockerignore` file for excluding files and directories.
- `-t` options is to specify a repository and tag at which to save the new image if the build succeeds: 
```
docker build -t shykes/myapp .
```
- Note that each instruction is run independently, and causes a new image to be created - so `RUN cd / tmp` will not have any effect on the next instructions. 
- Dockerfile must start with a `FROM` insturction. The `FROM` instruction specifies the _BASE IMAGE_ from which you are building. 
- `.dockerignore` file
Lines starting with ! (exclamation mark) can be used to make exceptions to exclusions. The following is an example .dockerignore file that uses this mechanism:
```
*.md
!README.md
```
All markdown files except README.md are excluded from the context.

The placement of ! exception rules influences the behavior: the last line of the .dockerignore that matches a particular file determines whether it is included or excluded. Consider the following example:
```
*.md
!README*.md
README-secret.md
```
No markdown files are included in the context except README files other than README-secret.md.

Now consider this example:
```
*.md
README-secret.md
!README*.md
```
All of the README files are included. The middle line has no effect because !README*.md matches README-secret.md and comes last.
- `RUN` instruction will execute any commands in a new layer on top of the current image and commit the results. The resulting image will be used for the next step in the `Dockerfile`.
- `CMD` can only appear once in `Dockerfile`. If you list more than one, the last will take effect. 
- `RUN` and `CMD`: `RUN` runs a command and commits the result; `CMD` does not execute anything at build time, but specifies the intended command for the image. 
- `LABEL` addes metadata to an image. It taks in key-value pairs. To view an image's labels, use the `docker inspect` command. 
- `EXPOSE` informs Docker that the container listens on the specifies network ports at runtime. This only functions as a type of documentation about which ports are intended to be published. It does not actually publish the port. To publish to port, use `-p` flag on `docker run` or `-P` flag: 
```
docker run -p 80:80/tcp -p 80:80/udp ...
```
- `ENV` sets the environment variables. This will be in the environment for all subsequent instructions in the build stage. The environment variables set using `ENV` will persist when a container is run from the resulting image. You can view the values using `docker inspect`.
- `VOLUME` instruction creates a mount point with the specific name and marks it as holding externally mounted volumes from native host or other containers. 
- `USER` instruction sets the user to use when running the image and for any `RUN`, `CMD` and `ENTRYPOINT` instructions that follow it. 
- `WORKDIR` sets the working directory. The WORKDIR instruction can be used multiple times in a Dockerfile. If a relative path is provided, it will be relative to the path of the previous WORKDIR instruction. 
- The `ARG` instruction defines a variable that users can pass at build-time to the builder with the docker build command using the `--build-arg <varname>=<value>` flag. If a user specifies a build argument that was not defined in the Dockerfile, the build outputs a warning.
```
ARG <name>[=<default value>]
```
- Tag image: 
```
docker tag image username/repository:tag
docker tag friendlyhello gordon/get-started:part2
docker image ls
```
- For tagged and published images, use `docker run` to run the app on any machine with for example: 
```
docker run -p 4000:80 username/repository:tag
```
If the image is not in local, Docker pulls it from the repository. 

### Docker compose
- Edit `docker-compose.yml`
```
version: "3"
services:
  web:
    # replace username/repo:tag with your name and image details
    image: username/repo:tag
    deploy:
      replicas: 5
      resources:
        limits:
          cpus: "0.1"
          memory: 50M
      restart_policy:
        condition: on-failure
    ports:
      - "4000:80"
    networks:
      - webnet
networks:
  webnet:
```
- Before we can use the `docker stack deploy` command, first run:
```
docker swarm init
```
- After, run `stack` to deploy, meanwhile give the service a name: 
```
docker stack deploy -c docker-compose.yml getstartedlab
docker service ls
```
- If set more than 1 instaces in `docker-compose.yml` file (by setting `replica` value), docker starts services using a load-balancing strategy. One of the tasks (container) is chosen to reponde. 
- For scaling the app, change `replica` value in yml file, deploy the app again by running: 
```
docker stack deploy -c docker-compose.yml getstartedlab
```
Docker will perform an in-place update, no need to shutdown or kill any containers. 
- Shut down the app and the swarm:
```
docker stack rm getstartedlab
docker swarm leave --force
```