# docker-astro-jenkinsagent

Contain build for jenkins swarm. Acts as self-registering node to jenkins

Can execute docker containers on swarm via docker-compose

We mount workspace persistently
We mount docker TLS certs and keys in .docker
Either export or set the following env variables

* COMMAND_OPTIONS
* USER_NAME_SECRET
* PASSWORD_SECRET
* DOCKER_HOST
* DOCKER_TLS_VERIFY

An example service start-up using secrets for jenkins auth

docker service create \
--name "jenkins-agent" \
--mount "type=bind,src=/var/lib/dockervols/jenkins-agent,dst=/workspace" \
--mount "type=bind,src=/var/lib/dockervols/.docker,dst=/root/.docker" \
--secret jenkins-user \
--secret jenkins-pass \
-e "USER_NAME_SECRET=/run/secrets/jenkins-user" \
-e "PASSWORD_SECRET=/run/secrets/jenkins-pass" \
-e "COMMAND_OPTIONS=-master <your jenkins url> -labels 'docker' -executors 5" \
-e "DOCKER_HOST=tcp://<remote docker host ip>:2376" \
-e "DOCKER_TLS_VERIFY=1" \
usgsastro/jenkinsagent
