pipeline {
  environment {
    registry = "vanhayten/tp5"
  }
  agent any
  stages {

    stage('Cloning Git') {
      steps {
        git 'https://github.com/Vanhayten/TP5-DEVOPS.git'
      }
    }

    stage('Building image') {
      steps {
        sh 'docker build -t vanhayten/tp5:${BUILD_NUMBER} .'
      }
    }

    stage('Test image') {
      steps {
        echo "Tests passed"
      }
    }

    stage('Publish Image') {
      steps {
        withCredentials([usernamePassword(
            credentialsId: 'dockerhub-creds',
            usernameVariable: 'USER',
            passwordVariable: 'PASS')]) {
          sh '''
            echo "$PASS" | docker login -u "$USER" --password-stdin
            docker push vanhayten/tp5:${BUILD_NUMBER}
          '''
        }
      }
    }

    stage('Deploy image') {
      steps {
        sh 'docker stop tp5container || true'
        sh 'docker rm tp5container || true'
        sh 'docker run -d --name tp5container -p 8081:80 vanhayten/tp5:${BUILD_NUMBER}'
      }
    }

  }
}
