pipeline {
  agent any

  tools {
    jdk 'Java21'
    maven 'Maven3'
  }

  environment {
    SONARQUBE_ENV = 'SonarQube'
  }

  stages {
    stage('Build y Test') {
      steps {
        dir('authapp') {
          sh 'mvn clean test'
        }
      }
    }

    stage('SonarQube Analysis') {
      steps {
        dir('authapp') {
                    withSonarQubeEnv("${SONARQUBE_ENV}") {
                        withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
            sh '''
              mvn sonar:sonar \
                -Dsonar.projectKey=authapp \
                -Dsonar.host.url=http://sonarqube:9000 \
                -Dsonar.login=$SONAR_TOKEN
            '''
          }
        }
      }
    }
  }
}
