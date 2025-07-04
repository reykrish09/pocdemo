pipeline {
  agent any

  tools {
    // Make sure you've configured this in Jenkins → Global Tool Configuration
    sonar 'SonarScanner 7.1'
    jdk21 'OpenJDK 21'
  }

  environment {
    SONARQUBE  = 'sonar'  // Name from your SonarQube config in Jenkins
    SONAR_HOST_URL = 'https://sonarcloud.io/'
  }

  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/reykrish09/pocdemo.git'  // Replace with your repo
      }
    }

    stage('SonarQube Analysis') {
      steps {
        script {
          def scannerHome = tool 'SonarScanner 7.1'
          withSonarQubeEnv(SONARQUBE) {
            sh """
              ${scannerHome}/bin/sonar-scanner \
                -Dsonar.projectKey=reykrish09 \
                -Dsonar.sources=src \
                -Dsonar.java.binaries=target/classes \
                -Dsonar.host.url=${env.SONAR_HOST_URL} \
                -Dsonar.login=${env.SONAR_AUTH_TOKEN}
            """
          }
        }
      }
    }

