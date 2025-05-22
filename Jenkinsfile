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
        sh '''ls
echo "WORKSPACE = $WORKSPACE"'''
        sh 'mvn clean install -Dmaven.repo.local=$WORKSPACE/.m2/repository'
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
        sh 'mvn clean test -Dmaven.repo.local=$WORKSPACE/.m2/repository'
        echo 'my custom message'
      }
    }

    stage('package') {
      parallel {
        stage('package') {
          agent {
            docker {
              image 'maven:3.8.8-eclipse-temurin-21-alpine'
            }

          }
          steps {
            echo 'package maven app'
            sh 'mvn package -DskipTests -Dmaven.repo.local=$WORKSPACE/.m2/repository'
            archiveArtifacts '**/target/*.jar'
          }
        }

        stage('Docker B&D') {
          agent any
          steps {
            script {
              docker.withRegistry('https://index.docker.io/v1/', 'dockerlogin') {
                def commitHash = env.GIT_COMMIT.take(7)
                def dockerImage = docker.build("xxxxxx/sysfoo:${commitHash}", "./")
                dockerImage.push()
                dockerImage.push("latest")
                dockerImage.push("dev")
              }
            }

          }
        }

      }
    }

  }
  tools {
    maven 'Maven 3.8.8'
  }
}