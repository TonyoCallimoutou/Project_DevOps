pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        sh 'docker build -t my-image .'
      }
    }

    stage('Deploy') {
      steps {
        sshagent(['my-ssh-credentials']) {
          sh 'ssh vagrant@192.168.56.2 "cd /projet/docker && sudo docker-compose up --build -d"'
        }
      }
    }
  }
}
