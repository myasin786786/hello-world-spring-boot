pipeline {
  agent any
  tools {
    maven '3.6.3'
  }
  stages {
    stage('Quality Gate Status Check'){
      steps{
        script{
          withSonarQubeEnv('sonarserver'){
            sh "mvn sonar:sonar"
          }
          timeout(time: 1, unit: 'HOURS'){
            def qg = waitForQualityGate()
            if (qg.status != 'OK'){
              error "Pipeline aborted due to quality gate failure : ${qg.status}"
            }
          }
          sh 'mvn clean package'
          sh 'cp target/myproject-0.0.1-SNAPSHOT.jar $HOME/data/'
        }
      }
    }
  }

  post {
    always {
      cleanWs()
    }
  }
}
