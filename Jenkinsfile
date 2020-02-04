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

  }
}