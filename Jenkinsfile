node {

  checkout scm

  stage("Build") {
    docker.image('php:8.2-cli').inside('-u root') {
      sh '''
        set -eux
        apt-get update
        apt-get install -y git unzip zip curl libzip-dev libpng-dev libjpeg62-turbo-dev libfreetype6-dev libonig-dev libxml2-dev

        docker-php-ext-configure gd --with-freetype --with-jpeg
        docker-php-ext-install gd zip mbstring

        curl -sS https://getcomposer.org/installer | php
        mv composer.phar /usr/local/bin/composer

        composer install
      '''
    }
  }

  stage("Testing") {
    docker.image('ubuntu:22.04').inside('-u root') {
      sh '''
        set -eux
        echo "Ini adalah test"
      '''
    }
  }

  stage("Deploy") {
    docker.image('agung3wi/alpine-rsync:1.1').inside('-u root') {
      sshagent(credentials: ['ssh-prod']) {
        withEnv(['PROD_HOST=172.30.85.222']) {
          sh '''
            set -eux

            mkdir -p ~/.ssh
            chmod 700 ~/.ssh

            ssh-keyscan -H "$PROD_HOST" >> ~/.ssh/known_hosts 2>/dev/null || true
            chmod 600 ~/.ssh/known_hosts

            ssh -o StrictHostKeyChecking=no faris@$PROD_HOST 'mkdir -p /home/faris/prod.kelasdevops.xyz/AhmadAlfahrezi && echo SSH_OK'

            rsync -rav --delete ./ \
              faris@$PROD_HOST:/home/faris/prod.kelasdevops.xyz/AhmadAlfahrezi/ \
              --exclude='public/build' \
              --exclude='node_modules' \
              --exclude='vendor' \
              --exclude='storage' \
              --exclude='.git' \
              --exclude='.env'
          '''
        }
      }
    }
  }
}
