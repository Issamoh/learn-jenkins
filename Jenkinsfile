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

    stage('deployment') {
      steps {
        bat 'gradle publish'
      }
    }

  }
}