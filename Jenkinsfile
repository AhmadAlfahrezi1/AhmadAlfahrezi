node {

  checkout scm

  stage("Build") {
    docker.image('my-php-laravel:8.2').inside('-u root') {
      sh '''
        set -eux
        composer install --no-interaction --prefer-dist --optimize-autoloader
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
