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

       stage('Deploy') {
              steps{
            
                  withAWS(credentials: 'aws', region: 'us-east-1') {
                      sh "aws eks --region us-east-1 update-kubeconfig --name cluster"
                      sh "kubectl config use-context arn:aws:eks:us-east-1:799365621121:cluster/cluster"
                      sh "kubectl set image deployments/frontend frontend=silvermoonlight/frontend:latest"
                      sh "kubectl apply -f deployment.yml"
                      sh "kubectl get nodes"       
                  }
              }
        }

  }

}