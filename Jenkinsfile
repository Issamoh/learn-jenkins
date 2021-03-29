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

    stage('deployment') {
      steps {
        bat 'gradle publish'
      }
    }

    stage('slack notification') {
      steps {
        slackSend(baseUrl: 'https://hooks.slack.com/services/', token: 'TSGEF9MGR/BSUBPE4BS/BM05OeSl0OtgT627ddwu9aXp', channel: '#déploiement-tp-gradle', message: 'Jenkins : new build and deployment', sendAsText: true, teamDomain: 'gradledeployments')
        slackSend(baseUrl: 'https://hooks.slack.com/services/', token: 'T01M53BC6G6/B01SNHK2Q13/xC0ZFE8jnHYcnGUgGvbI9vNN', channel: '#jenkins', message: 'Jenkins : new build and deployment', sendAsText: true, teamDomain: 'gradledeployments', notifyCommitters: true)
      }
    }

  }
}