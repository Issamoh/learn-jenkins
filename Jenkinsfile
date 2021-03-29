pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        bat 'gradle build'
        bat 'gradle javadoc'
        archiveArtifacts 'build/libs/*.jar'
        archiveArtifacts 'build/docs/javadoc/*'
        junit 'build/test-results/test/*'
      }
    }

    stage('Mail notification') {
      steps {
        mail(subject: 'build', body: 'build is done', cc: 'hm_ben_messaoud@esi.dz')
      }
    }

    stage('Code Analysis') {
      parallel {
        stage('Code Analysis') {
          steps {
            bat 'gradle sonarqube'
            waitForQualityGate true
          }
        }

        stage('Test Reporting') {
          steps {
            cucumber '\'reports/example-report.json\''
          }
        }

      }
    }

  }
}