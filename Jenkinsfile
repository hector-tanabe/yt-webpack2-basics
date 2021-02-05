pipeline {
  // agent { label 'ubuntu-node-01' }
  agent any

  options {
    buildDiscarder(logRotator(numToKeepStr: '1', artifactNumToKeepStr: '1'))
  }

  triggers {
    pollSCM('0 */1 * * 1-5')
  }

  environment {
    NEXUS_REGISTRY_URL = 'http://192.168.0.7:8081/repository/backend/'
    NEXUS_AUTH_TOKEN   = 'amVua2lucy11c2VyOjEyMzQ1'
  }

  stages {

    stage('Build') {
      steps {
        checkout scm
        withGradle {
          sh '''
            ./gradlew registrySetup \
                      nodeSetup \
                      npmInstall \
                      npm_run_build \
                      -PregistryUrl=$NEXUS_REGISTRY_URL \
                      -PathToken=$NEXUS_AUTH_TOKEN
          '''
        }
      }
    }

    stage('Publish') {
      steps {
        withGradle {
          sh '''
            ./gradlew npm_publish
          '''
        }
      }
    }

    stage('Deploy') {
      steps {
                echo 'Deploying....'
            }
    }
  }
}
//
