pipeline {
  agent {
    docker {
      reuseNode 'true'
      registryUrl 'https://coding-public-docker.pkg.coding.net'
      image 'public/docker/nodejs:20-2024.01'
      args '-v /root/.npm:/root/.npm'
    }

  }
  stages {
    stage('Checkout') {
      steps {
        checkout([
          $class: 'GitSCM',
          branches: [[name: GIT_BUILD_REF]],
          userRemoteConfigs: [[
            url: GIT_REPO_URL,
            credentialsId: CREDENTIALS_ID
          ]]])
        }
      }
      stage('Setup') {
        steps {
          script {
            sh 'npm init -y'
            sh 'npm install hexo-seo-submit'
          }
        }
      }
      stage('Push Search Engines') {
        steps {
          script {
            sh "npx hexo-seo-submit baidu -t $env.BAIDU_TOKEN -s https://ksh7.com -f hexo-seo-submit/baidu.txt"
            sh "npx hexo-seo-submit bing -k $env.BING_APIKEY -f hexo-seo-submit/bing.json"
            sh "npx hexo-seo-submit google -f hexo-seo-submit/google-url.txt -mail $env.GOOGLE_CLIENT_EMAIL -key $env.GOOGLE_PRIVATE_KEY"
          }
        }
      }
    }
  }
