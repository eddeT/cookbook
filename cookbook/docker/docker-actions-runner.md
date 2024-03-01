# docker actions runner
Using: https://github.com/myoung34/docker-github-actions-runner

## Single runner for specific repo
```yaml
---
name: <your project name>
services:
    github-runner:
        restart: always
        container_name: <container_name>
        environment:
            - REPO_URL=<repo_url>
            - RUNNER_NAME=<runner_name>
            - RUNNER_TOKEN=<runner_token>
            - RUNNER_WORKDIR=/tmp/github-runner-<name>
            - RUNNER_GROUP=Default
            - LABELS=docker-test
            - TZ=Europe/Berlin
        volumes:
            - /var/run/docker.sock:/var/run/docker.sock
            - /tmp/github-runner-your-repo:/tmp/github-runner-your-repo
        image: myoung34/github-runner:latest
```
---