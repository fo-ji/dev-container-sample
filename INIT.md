# Setup
## Step1
### ./docker-compose.yml
```yml
version: '3.8'

services:
  app:
    platform: linux/amd64
    build:
      context: .
      dockerfile: ./Dockerfile
    working_dir: /workspace
    volumes:
      - ./:/workspace:cached
      - node_modules:/workspace/node_modules

volumes:
  node_modules:
```
### ./Dockerfile
```Dockerfile
FROM node:18.14.0-slim
```

## Step2
```zsh
$ docker-compose run --rm client yarn create vite
$ mv dev-container-sample/* .
$ rm -rf dev-container-sample
```

## Step3
### ./.devcontainer/docker-compose.yml
```yml
version: '3.8'

services:
  app:
    command: sh -c 'yarn && yarn dev'
    ports:
      - 3000:5173
      - 8080:8080
```
### ./.devcontainer/devcontainer.json
```json
{
  "dockerComposeFile": [
    "../docker-compose.yml",
    "docker-compose.yml"
  ],
  "service": "app",
  "workspaceFolder": "/workspace"
}
```
### ./package.json
```diff
{
  "name": "dev-container-sample",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
-   "dev": "vite",
+   "dev": "vite --host",
    "build": "tsc && vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
  "devDependencies": {
    "@types/react": "^18.0.27",
    "@types/react-dom": "^18.0.10",
    "@vitejs/plugin-react": "^3.1.0",
    "typescript": "^4.9.3",
    "vite": "^4.1.0"
  }
}
```

## Step4
select Reopen in Container ...