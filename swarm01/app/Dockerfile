# syntax=docker/dockerfile:1.4

FROM --platform=$BUILDPLATFORM php:8.0.9-apache as builder

CMD ["apache2-foreground"]

FROM builder as dev-envs

RUN apt-get update
RUN apt-get install -y --no-install-recommends git


RUN useradd -s /bin/bash -m vscode
RUN groupadd docker
RUN usermod -aG docker vscode

# install Docker tools (cli, buildx, compose)
COPY --from=gloursdocker/docker / /

COPY . /var/www/html 

CMD ["apache2-foreground"]
