pipeline {
    agent any
    environment {
        AWS_ACCESS_KEY_ID = credentials('AWS secret access ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS sceret access key')
        KEY = credentials('Muthur')
    }
    stages{
        stage('docker-build'){
            steps{
                dir('codebase') {
                    script{
                        def imagename = "muthuinc/webapp:latest"
                        def dockerimage = docker.build("${imagename}", ".")
                    }
                }
            }
        }

        stage('docker-push'){
            steps{
                script{
                    def appname = "muthuinc/webapp:latest"
                    withDockerRegistry(credentialsId: 'dockerid') {
                        docker.image(appname).push()
                    }
                }
            }
        }
        stage('k8 deployment'){
            steps{
                dir('K8s'){
                    sh 'aws eks update-kubeconfig --region ap-southeast-2 --name Ava'
                    sh 'kubectl apply -f deployment.yaml'
                    sh 'kubectl apply -f service.yaml'
                }
            }
        }
    }

    post{
        success{
            sh 'docker rmi muthuinc/webapp:latest'
        }
    }
}