pipeline {
    agent any

    environment {
        PROD_HOST = '54.251.5.36'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    docker.image('composer:2').inside('-u root') {
                        sh 'git config --global --add safe.directory $WORKSPACE'
                        sh 'composer install --ignore-platform-reqs'
                    }
                }
            }
        }

        stage('Testing') {
            steps {
                sh 'echo Build berhasil'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    docker.image('agung3wi/alpine-rsync:1.1').inside('-u root') {
                        sshagent(credentials: ['ssh-prod']) {
                            sh '''
                                mkdir -p ~/.ssh
                                chmod 700 ~/.ssh
                                ssh-keyscan -H "$PROD_HOST" >> ~/.ssh/known_hosts

                                rsync -rav --delete ./ \
                                ubuntu@$PROD_HOST:/home/ubuntu/prod.kelasdevops.xyz/ \
                                --exclude=.env \
                                --exclude=storage \
                                --exclude=.git
                            '''
                        }
                    }
                }
            }
        }
    }
}
