pipeline {
  agent any
  stages {
    stage('error') {
      steps {
        sh 'gradle build'
      }
    }

    stage('Mail Notification') {
      steps {
        mail(subject: 'Build Result', body: 'the build has been done', to: 'ga_mazrou@esi.dz')
      }
    }

    stage('Code analysis') {
      parallel {
        stage('test report') {
          steps {
            jacoco()
          }
        }

        stage('Code Analysis') {
          steps {
            sh 'gradle sonarqube'
          }
        }

      }
    }

    stage('Depoly') {
      when {
        branch 'master'
      }
      steps {
        sh 'gradle publishing'
      }
    }

    stage('slack notification') {
      when {
        branch 'master'
      }
      steps {
        slackSend(baseUrl: 'https://hooks.slack.com/services/', teamDomain: 'tp6-outil', token: 'TRQ5N6NJH/BRCM572RH/NxnK9GViNA6CDT0wVcota4N8')
      }
    }

  }
}