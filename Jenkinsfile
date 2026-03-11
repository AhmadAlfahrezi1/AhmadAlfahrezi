node {
    checkout scm

    stage('Build') {
        docker.image('composer:2').inside('-u root') {
            sh 'composer install'
        }
    }

    stage('Test') {
        docker.image('ubuntu').inside('-u root') {
            sh 'echo "Build berhasil"'
        }
    }
}
