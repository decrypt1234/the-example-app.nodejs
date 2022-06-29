pipeline {
    agent any

      stages {
          stage('build') {
              steps {
                  echo 'building the software'
                  sh 'npm install'
              }
          }
          
          stage('deploy') {
              steps {
                withCredentials([sshUserPrivateKey(credentialsId: "jenkins-ssh", keyFileVariable: 'sshkey')]){
                  echo 'deploying the software'
                  sh '''#!/bin/bash
                  echo "Creating .ssh"
                  mkdir -p /var/lib/jenkins/.ssh
                  ssh-keyscan 3.92.186.88 >> /var/lib/jenkins/.ssh/known_hosts
                  ssh-keyscan 3.84.165.174 >> /var/lib/jenkins/.ssh/known_hosts

                  rsync -avz --exclude  '.git' --delete -e "ssh -i $sshkey" ./ ubuntu@3.92.186.88:/app/
                  rsync -avz --exclude  '.git' --delete -e "ssh -i $sshkey" ./ ubuntu@3.84.165.174:/app/

                  ssh -i $sshkey ubuntu@3.92.186.88 "sudo systemctl restart nodeapp"
                  ssh -i $sshkey ubuntu@3.84.165.174 "sudo systemctl restart nodeapp"

                  '''
              }
          }
      }
    }
}
