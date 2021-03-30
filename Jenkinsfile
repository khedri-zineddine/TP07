pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        bat 'gradle build'
        bat 'gradle javadoc'
        bat 'gradle archiveJar  archiveDoc'
        bat 'gradle archiveTestsResults'
      }
    }

    stage('mail') {
      steps {
        mail(subject: 'Build code', body: 'Le code et builded', from: 'hz_khedri@esi.dz', to: 'hz_khedri@esi.dz')
      }
    }

    stage('Code Analysis') {
      parallel {
        stage('Code Analysis') {
          steps {
            withSonarQubeEnv('sonar') {
              bat 'gradle sonarqube'
            }

            waitForQualityGate true
          }
        }

        stage('test reporting') {
          steps {
            cucumber 'reports/*.json'
          }
        }

      }
    }

    /*stage('Deployement') {
      when {
        branch 'master'
      }
      steps {
        bat 'gradle publish'
      }
    }*/

    stage('Slack notification') {
      when {
        branch 'master'
      }
      steps {
        slackSend(baseUrl: 'https://hooks.slack.com/services/', token: 'T01T5DPV9SM/B01TGQ21RHN/AafBXVeGzdpSb2t527rlQxwr', attachments: '.', blocks: '.', channel: '#général', message: 'Deployement', sendAsText: true)
      }
    }

  }
}