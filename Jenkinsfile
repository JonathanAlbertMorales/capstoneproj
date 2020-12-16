pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/JonathanAlbertMorales/capstoneproj.git', branch:'master'
      }
    }
    stage('Lint HTML'){
                steps{
                    sh 'tidy -q -e *.html'
                }
            }
      stage("Build image") {
            steps {
                script {
                    myapp = docker.build("silvermoonlight/frontend:${env.BUILD_ID}")
                }
            }
        }
    
      stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

  }

}