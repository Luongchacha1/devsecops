pipeline {
  agent any
  stages {
    stage('Build Artifact - Maven') {
      steps {
        sh "mvn clean package -DskipTests=true"
        archive 'target/*.jar'
      }
    }
    stage('Docker Build and Push') {
      steps {
        withDockerRegistry([credentialsId: 'docker-myregistry-cred', url: 'https://myregistry.def.local:5000']) {
          sh 'printenv'
          sh 'docker build -t myregistry.def.local:5000/numeric-app:latest .'
          sh 'docker push myregistry.def.local:5000/numeric-app:latest'
        }
      }
    }
  }
}
