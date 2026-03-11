node {
    checkout scm

    stage('Build') {
        docker.image('php:8.4-cli').inside('-u root --entrypoint=""') {
            sh 'apt-get update'
            sh 'apt-get install -y git unzip libzip-dev libpng-dev libonig-dev libxml2-dev curl'
            sh 'docker-php-ext-install zip gd'
            sh 'curl -sS https://getcomposer.org/installer | php'
            sh 'mv composer.phar /usr/local/bin/composer'
            sh 'git config --global --add safe.directory /var/jenkins_home/workspace/laravel-dev'
            sh 'composer install'
        }
    }

    stage('Test') {
        docker.image('ubuntu').inside('-u root --entrypoint=""') {
            sh 'echo "Build berhasil"'
        }
    }
}
