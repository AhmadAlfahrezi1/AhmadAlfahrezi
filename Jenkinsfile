node {
    checkout scm

    stage('Build') {
        docker.image('composer:2').inside('-u root --entrypoint=""') {
            sh 'apt-get update || true'
            sh 'apt-get install -y git unzip libpng-dev || true'
            sh 'git config --global --add safe.directory /var/jenkins_home/workspace/laravel-dev'
            sh 'composer install --ignore-platform-reqs'
        }
    }

    stage('Test') {
        sh 'echo "Build berhasil"'
    }
}
