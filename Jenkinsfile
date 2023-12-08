pipeline {
    agent any
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
            sh "docker login -u $DOCKERHUB_USR -p $DOCKERHUB_PSW"
            sh "docker tag todo $DOCKERHUB_USR/todo:latest"
            sh "docker push $DOCKERHUB_USR/todo:latest"
        }
    }

    stage (deliver){
        steps {
            sh "docker compose down && docker compose up"
        }
    }
}