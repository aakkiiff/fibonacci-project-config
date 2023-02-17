pipeline {
    agent any
    environment {

        GIT_REPO = "https://github.com/aakkiiff/fibonacci-project-config/" 
        CLIENT_APP_NAME = "fibonacci-client"
        SERVER_APP_NAME = "fibonacci-server"
        WORKER_APP_NAME = "fibonacci-worker"
    }

    stages{

        stage('CLEANUP WORKSPACE'){
            steps{
                script{
                    cleanWs()
                }
            }
        }

        stage("CHECKOUT GIT REPO"){
            steps{
                git "${GIT_REPO}"
            }
        }

        stage("UPDATE K8S DEPLOYMENT FILES AND PUSH TO THE GIT REPO"){
            steps{

                sh 'cat ./k8s/server-deployment.yaml'
                sh "sed -i 's/${CLIENT_APP_NAME}.*/${CLIENT_APP_NAME}:${IMAGE_TAG}/g' ./k8s/client-deployment.yaml"
                sh "sed -i 's/${SERVER_APP_NAME}.*/${SERVER_APP_NAME}:${IMAGE_TAG}/g' ./k8s/server-deployment.yaml"
                sh "sed -i 's/${WORKER_APP_NAME}.*/${WORKER_APP_NAME}:${IMAGE_TAG}/g' ./k8s/worker-deployment.yaml"
                sh 'cat ./k8s/server-deployment.yaml'

      


                sh 'git config --global user.email jackakif@gmail.com'
                sh 'git config --global user.name aakkiiff'
                sh 'git add ./k8s/'
                sh "git commit -m 'Updated deployment files to ${IMAGE_TAG}'"

                withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'pass', usernameVariable: 'uname')]) {
                    sh 'git push https://$uname:$pass@github.com/aakkiiff/fibonacci-project-config.git master'
                }
            }
        }
    }
}