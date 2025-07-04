pipeline {
  agent any

  tools {
    jdk 'jdk21'          // Ensure this matches your Jenkins JDK config
    maven "Maven"
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

      stage("mvn build") {
            steps {
                script {
                    // If you are using Windows then you should use "bat" step
                    // Since unit testing is out of the scope we skip them
                    bat(/${MAVEN_HOME}\bin\mvn -Dmaven.test.failure.ignore clean package/)
                }
            }
        }

    stage('SonarQube Analysis') {
      steps {
        script {
          scannerHome = tool 'sonar'
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
