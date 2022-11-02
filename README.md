## MONOREPO-DOCKER boilerplate

Install Docker-Desktop `https://www.docker.com/products/docker-desktop/` and run application.

## Angular-Client:
### Create necessary Docker files in your project root directory

`touch Dockerfile .dockerignore`

The `Dockerfile` file contains instructions on how the docker image should be generated.

```dockerfile
FROM node:lts-alpine
WORKDIR /usr/src/app

COPY ** *

EXPOSE 4200

CMD ng serve --host 0.0.0.0 --poll 2000 --disable-host-check
```

___
The `.dockerignore` decides which files should be ignored when copying files from your local project into the docker
image.

```gitignore
node_modules
Dockerfile
```

### Build docker image
To create our first image simply run `docker build .`

To create an image with a name and a tag `docker build -t angular-client:1.0.0 .`

Add missing instructions to Dockerfile:
```dockerfile
RUN npm i -g @angular/cli
COPY package.json .
RUN npm i
```

### Run docker containers

To create and run a new container from our images run `docker run angular-client -d` or `docker run --name angular-client -d angular-client`

To map an outside port with container port: `docker run --name angular-client -p 80:4200 -d angular-client
`


## NESTJS-API:

### Create necessary Docker files in your project root directory

`touch Dockerfile .dockerignore`

Dockerfile:
```dockerfile
FROM node:lts-alpine

RUN npm add -g @nestjs/cli

WORKDIR /usr/src/app

COPY package.json package.json
RUN npm i

COPY .. .

EXPOSE 3000

CMD nest start
```

.dockerignore:
```ignore
vendor
```

### Build docker image

To create an image with a name and a tag `docker build -t nestjs-api .`


### Run docker containers

Run container and map an outside port with container port: `docker run --name nestjs-api -p 3000:3000 -d nestjs-api`


## MONO REPO
### Initialize git

`git init monorepo-docker-boiler`

### Install PNPM globally and initialize

    ```
    npm i -g pnpm  
    pnpm init
    ```

### Create Monorepo

Create apps and libs folder:

`mkdir apps libs`

Create pnpm-workspace.yaml in root directory:

```

# pnpm-workspace.yaml

    packages:
    # executable/launchable applications

- 'apps/*'
  # all packages in subdirs of packages/ and components/
- 'packages/*'
  ```

### Git-Submodules and why to use them

Git-Submodules offer an advantage over split repositories:

    - Projects still exist individually but can be used in parent projects. A parent project example could manage docker
      images, handle shared dependencies and shared packages/libraries.

    - Child projects files are not commited in parent repository.


### Add Git-Submodules to parent repo

```shell
git submodule add <remote_url> <destination_folder>
git submodule add git@github.com:ravand1990/angular-client.git> ./apps/angular-client
```