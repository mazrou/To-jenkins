pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'gradle build'
        sh 'gradle javadoc'
        archiveArtifacts 'build/libs/**/*.jar'
        archiveArtifacts 'build/docs/**'
        junit 'build/test-results/test/*.xml'
      }
    }

    stage('Mail Notification') {
      steps {
        mail(subject: 'Build Result', body: 'the build has been done', to: 'ga_mazrou@esi.dz')
      }
    }

    stage('Code analysis') {
      parallel {
        stage('Test report') {
          steps {
            jacoco(execPattern: 'build/jacoco/*.exec', exclusionPattern: '**/test/*.class')
          }
        }

        stage('Code Analysis') {
          steps {
            withSonarQubeEnv('sonar') {
              sh 'gradle sonarqube'
            }

            waitForQualityGate true
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

    stage('Slack notification') {
      when {
        branch 'master'
      }
      steps {
        slackSend(baseUrl: 'https://hooks.slack.com/services', channel: '#gradle', color: '#ff0000', failOnError: true, message: 'The deplointment sucful', sendAsText: true, teamDomain: 'esi-8qh2328', token: 'TRCM87RQT/BTJV12813/DXgawbOtQL4CQmrm9yoTjVov', username: 'Mazrou Ayoub')
      }
    }

  }
}