# Projet_Dev_Ops

Commandes utilisées pour la création du projet ainsi que du runner : 

docker network create -d bridge gitlab_network

set GITLAB_HOME "$HOME/gitlab"

docker run --detach --hostname gitlab --platform=linux/amd64 --publish 8443:443 --publish 8080:80 --publish 8822:22 --name gitlab --restart always --volume $GITLAB_HOME/config:/etc/gitlab --volume $GITLAB_HOME/logs:/var/log/gitlab --volume $GITLAB_HOME/data:/var/opt/gitlab --shm-size 256m --net gitlab_network gitlab/gitlab-ce:latest

set GITLAB_RUNNER_HOME "$HOME/gitlab-runner"

docker run -d --name gitlab-runner --restart always --volume $GITLAB_RUNNER_HOME/config:/etc/gitlab-runner --volume /var/run/docker.sock:/var/run/docker.sock --net gitlab_network gitlab/gitlab-runner:latest

docker exec gitlab-runner gitlab-runner register -n --url http://gitlab/ --registration-token REPLACE_ME --description "Shared Docker Runner" --executor docker --docker-image "alpine:latest" --docker-volumes /var/run/docker.sock:/var/run/docker.sock --docker-network-mode gitlab_network
