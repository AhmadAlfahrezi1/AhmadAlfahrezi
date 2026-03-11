node {
    checkout scm

    stage('Build') {
        docker.image('composer:2').inside('-u root --entrypoint=""') {
            sh 'git config --global --add safe.directory /var/jenkins_home/workspace/laravel-dev'
            sh 'composer install --ignore-platform-reqs'
        }
    }

    stage('Test') {
        sh 'echo "Build berhasil"'
    }

    stage('Deploy') {
        docker.image('composer:2').inside('-u root --entrypoint=""') {
            sshagent(credentials: ['prod-key']) {
                sh 'mkdir -p ~/.ssh'
                sh 'ssh-keyscan -H $PROD_HOST >> ~/.ssh/known_hosts'
                sh '''
                rsync -avz --delete ./ ubuntu@$PROD_HOST:/home/ubuntu/prod.kelasdevops.xyz/ \
                --exclude=.git \
                --exclude=node_modules \
                --exclude=vendor
                '''
            }
        }
    }
}
