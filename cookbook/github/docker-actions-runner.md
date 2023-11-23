# docker actions runner
Using: https://github.com/myoung34/docker-github-actions-runner

## Single runner for specific repo
```
sudo docker run -d --restart always --name <container_name> \
  -e REPO_URL="<repo_url>" \
  -e RUNNER_NAME="<runner_name>" \
  -e RUNNER_TOKEN="<runner_token>" \
  -e RUNNER_WORKDIR="/tmp/github-runner-<name>" \
  -e RUNNER_GROUP="Default" \
  -e LABELS="<optional labels>" \
  -e TZ="Europe/Berlin" \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v /tmp/github-runner-your-repo:/tmp/github-runner-your-repo \
  myoung34/github-runner:latest
```