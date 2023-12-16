pipeline {
    agent any
    stages {
    stage ("checkout"){
        steps{
            git url: "https://github.com/ghanatava/CI-CD-Todo.git", branch:"main"
        }
    }
    stage (build){
        steps{
            sh "docker build . -t todo"
        }
    }

    stage (push){
        environment{
            DOCKERHUB = credentials("DOCKERHUB")
        }
        steps{
            sh "docker login -u $DOCKERHUB_USR -p '$DOCKERHUB_PSW' "
            sh "docker tag todo $DOCKERHUB_USR/todo:latest"
            sh "docker push $DOCKERHUB_USR/todo:latest"
        }
    }

    stage (deploy){
        steps {
            sshagent(credentials: ['ubuntu']){
                sh "scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/todo/docker-compose.yaml ubuntu@ec2-3-27-228-183.ap-southeast-2.compute.amazonaws.com:/home/ubuntu/docker-compose.yaml"
                sh "ssh -o StrictHostKeyChecking=no ubuntu@ec2-3-27-228-183.ap-southeast-2.compute.amazonaws.com 'docker compose up' || true "
            }
        }
    }
    }

}
