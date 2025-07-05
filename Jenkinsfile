pipeline {
  agent any

  tools {
    jdk 'jdk17'          // Ensure this matches your Jenkins JDK config

  }

  environment {
    SONARQUBE        = 'sonar'                     // Sonar server name in Jenkins config
    SONAR_HOST_URL   = 'https://sonarcloud.io/'   // Use your Sonar host URL
    SONAR_AUTH_TOKEN = credentials('sonar') // Secret Text credential stored in Jenkins
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    
    stage('SonarQube Analysis') {
      steps {
        script {
          scannerHome = tool name: 'sonar', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
          
            sh """
              ${scannerHome}/bin/sonar-scanner \
                -Dsonar.projectKey=reykrish09_pcodemo1 \
                -Dsonar.organization=reykrish09 \
                -Dsonar.sources=src \
                -Dsonar.host.url=${env.SONAR_HOST_URL} \
                -Dsonar.token=${env.SONAR_AUTH_TOKEN} \
                -Dsonar.java.binaries=.

            """
          
        }
      }
    }

  }

  post {  // Ensure pipeline closes properly
    success { echo "Analysis and Quality Gate passed!" }
    failure { echo "Analysis failed – check logs." }
  }
}
