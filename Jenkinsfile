## Docker Build and Push Stage
## replace  siddharth67 with your dockerhub username

pipeline {
  agent any

  stages {

    stage('Build Artifact - Maven') {
      steps {
        sh "mvn clean package -DskipTests=true"
        archive 'target/*.jar'
      }
    }

    stage('Unit Tests - JUnit and Jacoco') {
      steps {
        sh "mvn test"
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
          jacoco execPattern: 'target/jacoco.exec'
        }
      }
    }

    stage('Docker Build and Push') {
      steps {
        withDockerRegistry([credentialsId: "docker-myregistry-cred", url: "https://myregistry.def.local:5000"]) {
          sh 'printenv'
          sh 'docker build -t myregistry.def.local:5000/numeric-app:""$GIT_COMMIT"" .'
          sh 'docker push myregistry.def.local:5000/numeric-app:""$GIT_COMMIT""'
        }
      }
    }
  }
}
