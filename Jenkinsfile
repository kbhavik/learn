pipeline {
  agent any
  stages {
    stage('build_img') {
      steps {
        sh 'learnimg = docker.build bhvaik0907/learn:$(BUILD_NUMBER)'
      }
    }

    stage('push_img') {
      steps {
        sh '''docker.withRegistry(\'https://index.docker.io/v1/\', \'Docker-hub\') {
           learnimg.push()
          }'''
        }
      }

      stage('k8s_deploy') {
        steps {
          sh 'kubectl set image deploy/learn-deployment learn=bhavik0907/learn:$(BUILD_NUMBER)'
          sh 'kubectl apply -f learn-node-port.yml'
        }
      }

    }
  }