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
            sh '''
            mkdir -p ~/.ssh
            chmod 700 ~/.ssh
            ssh-keyscan -T 10 -H $PROD_HOST >> ~/.ssh/known_hosts || true

            ssh -o StrictHostKeyChecking=no -o ConnectTimeout=10 ubuntu@$PROD_HOST "mkdir -p /home/ubuntu/prod.kelasdevops.xyz"

            rsync -avz --delete ./ ubuntu@$PROD_HOST:/home/ubuntu/prod.kelasdevops.xyz/ \
            --exclude=.git \
            --exclude=node_modules \
            --exclude=vendor
            '''
        }
    }
}
