# Frontend app

Section 6 - Creating a production - grade workflow

Docker volumes

## [React official readme](public/README.md)

## [Dockerfile.dev](Dockerfile.dev)

Build image in the root directly where Dockerfile.dev is placed.

>> docker build -f Dockerfile.dev .

Run the react app in the container

>> docker run -it -p 3000:3000 ImageId

Little issue occured when trying to run the react app

>> [reactjs - Docker container exiting immediately after starting when using npm init react-app - Stack Overflow](https://stackoverflow.com/questions/60790440/docker-container-exiting-immediately-after-starting-when-using-npm-init-react-ap)

## Duplicated dependencies

Remove node_modules folder in the root directly to make the docker image buiding faster.

So that a message similar to "Sending build context to Docker daemon  184.7MB" won't be seen in the terimal again.

## Docker Volumes

Docker Volumes allow developer to update the changes without rebuilding the images and re-run the containers.

After building the image:

>> docker run -it -p 3000:3000 -v /app/node_modules -v $(pwd):/app ImageNameOrImageId

>> -v /app/node_modules (This will put a bookmark on the node_modules folder)

>> -v $(pwd):/app (This map the pwd into the '/app' folder)

## Using docker-compose.yml to reduce the works

Since the commands for docker volumes is a bit long. To make live easier, use docker-compose.yml to reduce the typing work each time the container needs to be started.

## Issues using docker-compose.yml

1. [reactjs - Docker container exiting immediately after starting when using npm init react-app - Stack Overflow](https://stackoverflow.com/questions/60790440/docker-container-exiting-immediately-after-starting-when-using-npm-init-react-ap)
2. [React-Scripts v3.4.1 fails to start in Docker · Issue #8688 · facebook/create-react-app](https://github.com/facebook/create-react-app/issues/8688)

Solution for this:

``` text
Put 'stdin_open: true' in docker-compose.yml
```

>> In our case, see [docker-compose.yml](docker-compose.yml)

## Testing with changing test suites

``` text

First Approach:

1. Set up docker-compose.yml with volumes
2. Run docker-compose up
3. Run docker exec -it containerId npm run test

Second Approach:
(Create a second service in docker-compose.yml)

Anything change in the App.test.js will be refreshed in the running container, thus updated in the browser.

```

## Docker production phase

``` text

1. Create an production Dockerfile with nginx base
2. Run 'docker build .' in the root directory to build image
3. Run 'docker run -p 8080:80 image_id" to run the container
4. View the web app in the browser by typing in url, which is 'localhost:8080'
```

## questions

what does -it do in

>> docker run -it -p 3000:3000 ImageId
