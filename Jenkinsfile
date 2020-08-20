pipeline {
  agent any
  stages {
    stage('build_img') {
      steps {
        script {
          learnimg = docker.build "bhavik0907/learn:v$BUILD_NUMBER"
        }
      }
    }

    stage('push_img') {
      steps {
        script {
          //docker.withRegistry('', 'Docker-hub') {
          docker.withRegistry('https://index.docker.io/v1/', 'Docker-hub') {
          //docker.withRegistry('bhavik0907/learn', 'Docker-hub') {  
           learnimg.push()
            }
          }
        }
      }

      stage('k8s_deploy') {
        steps {
          script {
            "kubectl set image deploy/learn-deployment learn=bhavik0907/learn:v$BUILD_NUMBER"
            "kubectl apply -f learn-node-port.yml"
          }
        }
      }

    }
  }
