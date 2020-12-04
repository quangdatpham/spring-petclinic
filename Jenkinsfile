pipeline {
  agent {
    label "ubuntu-1604"
  }
  environment {
    GOOGLE_PROJECT_ID = "dsc-fptu-hcmc-orientation"
  }
  stages {
    stage("Build") {
      steps {
        sh("./mvnw -DskipTests package")
      }
    }
    stage("Deploy") {
      steps {
        withCredentials([file(credentialsId: 'dsc-fptu-hcmc-orientation-credentials-file', variable: 'GC_KEY')]) {
          sh("gcloud auth activate-service-account --key-file=$GC_KEY")
          sh("./mvnw -DskipTests appengine:deploy")
        }
      }
    }
    stage("Upload artifacts") {
      steps {
        sh '''
          gsutil cp ./target/spring-petclinic-2.4.0.BUILD-SNAPSHOT.jar gs://$GOOGLE_PROJECT_ID-jenkins-artifacts/$JOB_NAME/$BUILD_NUMBER/spring-petclinic-2.4.0.BUILD-SNAPSHOT.jar
        '''
      }
    }
  }
  post {
    always {
      echo "========always========"
    }
    success {
      echo "========pipeline executed successfully ========"
    }
    failure {
      echo "========pipeline execution failed========"
    }
  }
}