pipeline {
  agent any

  tools {
    jdk 'jdk21'          // Ensure this matches your Jenkins JDK config
    maven "Maven"
  }
    parameters {
        choice(name: 'scanOnly',
            choices: 'no\nyes',
            description: 'This will scan your application'
        )
        choice(name: 'buildOnly',
            choices: 'no\nyes',
            description: 'This will Only Build your application'
        )
        choice(name: 'dockerPush',
            choices: 'no\nyes',
            description: 'This Will build dockerImage and Push'
        )
        choice(name: 'deployToDev',
            choices: 'no\nyes',
            description: 'This will Deploy the app to Dev env'
        )
        choice(name: 'deployToTest',
            choices: 'no\nyes',
            description: 'This will Deploy the app to Test env'
      
        )
        choice(name: 'deployToStage',
            choices: 'no\nyes',
            description: 'This will Deploy the app to Stage env'
        )
        choice(name: 'deployToProd',
            choices: 'no\nyes',
            description: 'This will Deploy the app to Prod env'
        )
    }
  environment {
    SONARQUBE        = 'sonar'                     // Sonar server name in Jenkins config
    SONAR_HOST_URL   = 'https://sonarcloud.io/'   // Use your Sonar host URL
    SONAR_AUTH_TOKEN = credentials('sonar-token') // Secret Text credential stored in Jenkins
  }

  stages {
        stage("clone code") {
            steps {
                script {
                    // Let's clone the source
                    git 'https://github.com/reykrish09/pocdemo.git';
                }
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

  }

}
