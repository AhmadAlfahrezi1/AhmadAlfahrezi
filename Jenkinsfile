stage("Deploy") {
  docker.image('agung3wi/alpine-rsync:1.1').inside('-u root') {
    sshagent(credentials: ['ssh-prod']) {
      sh '''
        set -eux
        echo "PROD_HOST=$PROD_HOST"
        mkdir -p ~/.ssh
        ssh-keyscan -H "$PROD_HOST" >> ~/.ssh/known_hosts
        ssh -o StrictHostKeyChecking=no faris@$PROD_HOST 'mkdir -p /home/faris/prod.kelasdevops.xyz/AhmadAlfahrezi && echo SSH_OK'
        rsync -rav --delete --exclude=".env" ./ faris@$PROD_HOST:/home/faris/prod.kelasdevops.xyz/AhmadAlfahrezi/
      '''
    }
  }
}
