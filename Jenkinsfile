node {

 checkout scm

 stage("Build"){
  docker.image('php:8.2-cli').inside('-u root') {
   sh '''
     apt-get update && apt-get install -y \
       git unzip libzip-dev zip curl openssh-client rsync \
       libpng-dev libjpeg62-turbo-dev libfreetype6-dev \
       libonig-dev libxml2-dev

     docker-php-ext-configure gd --with-freetype --with-jpeg
     docker-php-ext-install zip gd mbstring

     curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

     git config --global --add safe.directory $PWD

     composer install
   '''
  }
 }

 stage("Testing"){
  docker.image('ubuntu').inside('-u root') {
   sh 'echo "Ini adalah test"'
  }
 }

 stage("Deploy"){
  docker.image('agung3wi/alpine-rsync:1.1').inside('-u root') {
   sshagent(credentials: ['ssh-prod']) {
    sh 'mkdir -p ~/.ssh'
    sh 'ssh-keyscan -H "$PROD_HOST" > ~/.ssh/known_hosts'
    sh 'rsync -rav --delete ./ faris@$PROD_HOST:/home/faris/prod.kelasdevops.xyz/AhmadAlfahrezi/ --exclude=.env --exclude=storage --exclude=.git'
   }
  }
 }

}
