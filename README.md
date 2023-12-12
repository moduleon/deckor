# Deckor
A developer tool to make you forget that Docker is running.

## Overview
**Docker** and **docker-compose** offer tremendous utility. However, their usage often entails composing lengthy commands for simple tasks.

For instance, in a pre-Docker scenario while working on a JavaScript project, launching your app was as simple as:

```bash
npm run dev
```
Yet, with Docker, it has become something more convoluted :sweat_smile::

```bash
docker compose run --rm -u 1000:1000 app npm run dev
```
Consequently, various scripts are concocted to manage these complexities—installing dependencies, running the app, or building it. As this approach isn't conventional, it's usually mentioned in a README, hoping that users take notice.

Deckor proposes an alternative solution: a reversal to the previous simplicity while still leveraging Docker. The idea is to interact with the terminal as if Docker weren't present. To achieve this, Deckor utilizes aliases that come into play exclusively within your project directory.

## Requirements

For now, Deckor is only supported on **Linux** and **Mac OS**.

## Installation:

1. **Obtain this project from GitHub**

Clone it locally, or download and uncompress it.

2. **Terminal Installation**

Run the following command in your terminal from the deckor project folder:
```bash
./install
```
This will install Deckor CLI globally on your machine.

Verify if installation was successful:
```bash
deckor
```

## Usage

### Setup aliases within Docker-Compose Files

1. **Alias declaration**

Declare aliases as labels in your docker-compose file. Ensure the syntax of the declared label follows this structure: `deckor.alias.<alias>=<command>`

For instance:
```yaml
# docker-compose.yml
version: '3.4'
services:
    app:
    image: node:16-alpine
    volumes:
        - ${PWD}:/app
    working_dir: /app
    labels:
        - "deckor.alias.node=docker compose run --rm -u $(id -u):$(id -g) app node"
```

> [!TIP]
> Check out our [examples](./example) for various other languages.

2. **Call Deckor**

From your project directory containing the docker-compose file, execute the following:
```bash
deckor alias init
```
Now, your alias is accessible from this directory (only):
```bash
node --version
```

### Manually setup aliases

You can also use Deckor outside of docker-compose.

For example:
```bash
deckor alias add node "docker run --rm -v ./:/app --workdir /app node:16-alpine node" --reload

```
Now, your alias is accessible from this directory (only):
```bash
node --version
```

### Other commands

1. **Managing aliases**

Deckor comes with others commands allowing to manage aliases in your terminal:

command | description
--------|------------
deckor alias ls | List all aliases for the current directory
deckor alias clean |  Remove all aliases for the current directory
deckor alias rm `<alias>` | Remove a specific alias for the current directory

2. **Docker compose overlay**

Instead of calling `deckor init` to set up aliases, then `docker compose` to start or stop your containers, you can directly call :

```bash
deckor compose up -d
deckor compose down
```

It's an overlay for doing both things.

## Roadmap
- [X] Add support for Linux
- [X] Add support for Mac OS
- [ ] Add support for Windows

## License
This project is distributed under [GPL licence v3](./LICENCE) for always keeping it open source.
