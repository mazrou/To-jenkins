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

    stage('deploiment') {
      when {
        branch 'master'
      }
      steps {
        sh 'gradle uploadArchives'
      }
    }

  }
}