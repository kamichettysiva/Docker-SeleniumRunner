# Docker-SeleniumRunner
A sample **docker-compose** file and **GitHub Workflow actions** file run dockerised selenium tests of a flight booking application without adding any dependencies=

## Pre-requisite:
- To run the docker selenium runner, we should be having a docker image with automated test cases fully dockerised
- As an example, we are using docker image **kamichettysiva/dockertest:latest** with automated selenium test cases
- Please refer to https://github.com/kamichettysiva/Docker-SeleniumTestNG project containing the source code of docker image **kamichettysiva/dockertest:latest** and details on how to build the docker image


## SeleniumRunner Github Actions detail:

```
name: AirAsia docker test pipeline
on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Run AirAsia sample E2E tests in a docker container
      run: |
        docker-compose up -d selenium-hub chrome firefox
        docker-compose up search-module
        docker-compose down               
```
**name: Docker-SeleniumRunner** --This can be any name based on your preference <br> **on:** --This section is to specify the trigger point of your pipeline <br> **runs-on: ubuntu-latest** --Your docker will be running on this image <br> **uses: actions/checkout@v2** --Latest version of checkout action to checkout the workspace so that workflow can access it **run:** --This is the critical secton to set up the grid, run the tests and bring down the grid <br> **docker-compose up -d selenium-hub chrome firefox** --We should be running this in detached mode only by using (-d) as selenium hub will be triggered as a server and it will not automatically go-down after the tests are completed. If we miss -d here, pipeline will run forever. As selenium-hub, chrome, firefox are the pre-requisites for selenium, we should be starting them first as a grid

```
  selenium-hub:
    image: selenium/hub:3.141.59-bismuth
    container_name: selenium-hub
  chrome:
    image: selenium/node-chrome:3.141.59-bismuth
    depends_on:
      - selenium-hub
    environment:
      - HUB_HOST=selenium-hub
  firefox:
    image: selenium/node-firefox:3.141.59-bismuth
    depends_on:
      - selenium-hub
    environment:
      - HUB_HOST=selenium-hub
```

**docker-compose up search-module** --docker-compose.yml contains search module with parameters like browser, host and testng.xml (configurable), this command executes MODULE xml configured in docker compose file and once execution is completed successfully,kamichettysiva/dockertest:latest will be exited with code 0, if exited with code non 0, please check errors in volume mapping

```
search-module:
    image: kamichettysiva/dockertest:latest
    container_name: airasia-login-search
    depends_on:
      - firefox
      - chrome
    environment:
      - MODULE=airasia-sample-tests.xml
      - BROWSER=chrome
      - HUB_HOST=selenium-hub
```

**docker-compose down** --This is to stop all the running containers. i.e. selenium-hub,chrome and firefox>
        
        
Both docker-compose.yml file and airasia-docker-tests.yml (Under workflows) are in the repo for reference
