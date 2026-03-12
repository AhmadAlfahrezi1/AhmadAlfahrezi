node {

  checkout scm

  stage("Build") {
    docker.image('shippingdocker/php-composer:7.4').inside('-u root') {
      sh '''
        set -eux
        composer install
      '''
    }
  }

  stage("Testing") {
    docker.image('ubuntu').inside('-u root') {
      sh '''
        set -eux
        echo "Ini adalah test"
      '''
    }
  }

  stage("Deploy") {
    docker.image('agung3wi/alpine-rsync:1.1').inside('-u root') {
      sshagent(credentials: ['ssh-prod']) {
        sh '''
          set -eux
          mkdir -p ~/.ssh
          ssh-keyscan -H "$PROD_HOST" >> ~/.ssh/known_hosts
          rsync -rav --delete --exclude=".env" ./laravel/ faris@$PROD_HOST:/home/faris/prod.kelasdevops.xyz/AhmadAlfahrezi/
        '''
      }
    }
  }
}
