def dockerImage

pipeline{
  agent any

  triggers {
    pollSCM('* * * * *')
  }

  environment {
    registry = 'navapon/frontendcms'
    registryCredential = 'dockerhub'
    scannerHome = tool 'sonar'
    node = tool 'node'
    pem = credentials('namecredential'
  }

  stages {
  
    stage('Dependency Install') {
      steps {
        nodejs(nodeJSInstallationName: 'node') {
          sh "mkdir -p ./AppCms/ClientApp/node_modules"
          sh "npm install --prefix ./AppCms/ClientApp"
        }
      }
    }

    stage('SonarQube analysis') {
      steps {
        withSonarQubeEnv('SONAR_SERVER') {
          sh "${scannerHome}/bin/sonar-scanner"
        }
      }
    }

    stage('Bulding Docker Image') {
      steps {
        echo 'Building ...'
        // sh "docker build ./AppCms/ClientApp/ -t frontand-web:${env.BUILD_ID}"
        script {
          dockerImage = docker.build(registry + ":dev",'./AppCms/ClientApp/')
        }
      }
    }

    stage("Shiping Docker Image") {
      steps {
        script {
          docker.withRegistry('',registryCredential) {
            dockerImage.push()
          }
        }
      }
    }

    stage("Deploying ") {
      steps {
        script {
          echo 'Deploying '
        }
      }
    }
  }
}
  
