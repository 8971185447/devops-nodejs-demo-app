pipeline {
    agent any

      stages {
          stage('build') {
              steps {
                  echo 'building the software'
                  sh 'npm install'
              }
          }
          stage('test') {
              steps {
                  echo 'testing the software'
                  sh 'npm test'
              }
          }

          stage('deploy') {
              steps {
                withCredentials([sshUserPrivateKey( keyFileVariable: 'sshkey')]){
                  echo 'deploying the software'
                  sh '''#!/bin/bash
                  echo "Creating .ssh"
                  mkdir -p /var/lib/jenkins/.ssh
                  ssh-keyscan 35.154.119.164 >> /var/lib/jenkins/.ssh/known_hosts

                  rsync -avz --exclude  '.git' --delete -e "ssh -i $sshkey" ./ubuntu@35.154.119.164:/app/

                  ssh -i $sshkey ubuntu@35.154.119.164 "sudo systemctl restart nodeapp"
                  '''
              }
          }
      }
    }
}

