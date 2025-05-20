pipeline {
  agent none
  stages {
    stage('build') {
      agent {
        docker {
          image 'maven:3.8.8-eclipse-temurin-21-alpine'
        }

      }
      steps {
        echo 'compile maven app'
        sh 'mvn compile'
      }
    }

    stage('test') {
      agent {
        docker {
          image 'maven:3.8.8-eclipse-temurin-21-alpine'
        }

      }
      steps {
        echo 'test maven app'
        sh 'mvn clean test'
        echo 'my custom message'
      }
    }

    stage('package') {
      agent {
        docker {
          image 'maven:3.8.8-eclipse-temurin-21-alpine'
        }

      }
      steps {
        echo 'package maven app'
        sh 'mvn package -DskipTests'
        archiveArtifacts '**/target/*.jar'
      }
    }

  }
  tools {
    maven 'Maven 3.8.8'
  }
}