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

    stage('slack notification') {
      steps {
        slackSend(baseUrl: 'https://hooks.slack.com/services/', token: 'TSGEF9MGR/BSUBPE4BS/BM05OeSl0OtgT627ddwu9aXp', channel: '#déploiement-tp-gradle', message: 'Jenkins : new build and deployment', sendAsText: true, teamDomain: 'gradledeployments')
      }
    }

  }
}