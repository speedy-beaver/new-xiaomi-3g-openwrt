pipeline {
  script{
        properties([[$class: 'HudsonNotificationProperty', endpoints: [[buildNotes: '', urlInfo: [urlOrId: 'Zapier Jenkins Integration', urlType: 'SECRET']]]]])
  }
  agent {
    docker {
      image 'vuepress_container:latest'
    }

  }
  stages {
    stage('Build') {
      steps {
        sh 'vuepress build'
      }
    }
    stage('Test') {
      steps {
        sh 'ls -l .vuepress/dist'
      }
    }
    stage('Deploy') {
      environment {
        GITHPW = credentials('mysecondgithub')
      }
      steps {
        sh '''cd .vuepress/
cd dist/
git config --global user.email "github@se.iler.de"
git config --global user.name "speedy-beaver"
git init
git add -A
git commit -m \'deploy\'
git push -f --set-upstream https://$GITHPW_PSW@github.com/speedy-beaver/speedy-beaver.github.io master
'''
      }
    }
    stage('Notify') {
      environment {
        PUSHOVER_USER = credentials('pushover_user')
        PUSHOVER_TOKEN = credentials('pushover_token')
      }
      steps {
        sh '''curl -s --form-string user=${PUSHOVER_USER} --form-string token=${PUSHOVER_TOKEN} --form-string message=Done-$BUILD_TAG https://api.pushover.net/1/messages.json
 '''
        sh 'env'
      }
    }
  }
}
