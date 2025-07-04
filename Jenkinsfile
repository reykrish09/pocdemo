pipeline {
  agent any

  tools {
    jdk 'jdk21'          // Ensure this matches your Jenkins JDK config
    sonar 'SonarScanner 7.1'    // Ensure this matches your Jenkins Tool name

  }

  environment {
    SONARQUBE        = 'sonar'                     // Sonar server name in Jenkins config
    SONAR_HOST_URL   = 'https://sonarcloud.io/'   // Use your Sonar host URL
    SONAR_AUTH_TOKEN = credentials('sonar-token') // Secret Text credential stored in Jenkins
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
          def scannerHome = tool 'SonarScanner 7.1'
          withSonarQubeEnv(SONARQUBE) {
            sh """
              ${scannerHome}/bin/sonar-scanner \
                -Dsonar.projectKey=reykrish09 \
                -Dsonar.sources=src \
                -Dsonar.java.binaries=target/classes \
                -Dsonar.host.url=${env.SONAR_HOST_URL} \
                -Dsonar.token=${env.SONAR_AUTH_TOKEN}
            """
          }
        }
      }
    }

    stage('Quality Gate') {   //  Added missing Quality Gate stage
      steps {
        timeout(time: 2, unit: 'MINUTES') {
          waitForQualityGate abortPipeline: true
        }
      }
    }
  }

  post {  // Ensure pipeline closes properly
    success { echo "Analysis and Quality Gate passed!" }
    failure { echo "Analysis failed – check logs." }
  }
}