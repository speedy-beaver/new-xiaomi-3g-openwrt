pipeline {
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
    stage('Test 2') {
      steps {
        sh 'cat .vuepress/dist/index.html'
        sh 'cat .vuepress/dist/404.html'
        sh '''echo This might be it myfirstgithub
echo "TOTAL $myfirstgithub"'''
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
  }
}
