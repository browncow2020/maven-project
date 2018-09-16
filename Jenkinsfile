pipeline {
  agent any
  tools {
    maven 'localMaven'
  }
  stages {
    stage ('Build'){
      steps {
        sh 'mvn clean package -e'
      }
      post {
        success {
          echo 'Now Archiving...'
          archiveArtifacts artifacts: '**/target/*.war'
        }
      }
    }
    stage ('Deploy to Staging'){
      steps {
        build job: 'deploy-to-staging'
      }
      post {
        success {
          echo 'Code deployed to Staging.'
        }
        failure {
          echo 'Staging deployment failed.'
        }
      }
    }
    stage ('Deploy to Production'){
      steps {
        timeout(time:5, unit:'DAYS'){
          input message:'Approve PRODUCTION Deployment?'
        }

        build job: 'deploy-to-prod'
      }
      post {
        success {
          echo 'Code deployed to Production.'
        }
        failure {
          echo 'Production deployment failed.'
        }
      }
    }
  }
}
