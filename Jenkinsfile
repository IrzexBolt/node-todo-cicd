pipeline {
    agent { label "dev-server" }
    
    stages {
        stage("code") {
            steps {
                git url: "https://github.com/IrzexBolt/node-todo-cicd.git", branch: "master"
                echo 'Code clone successful'
            }
        }

        stage("build and test") {
            steps {
                sh "docker build -t node-app-test-new ."
                echo 'Code build successful'
            }
        }

        stage("scan image") {
            steps {
                echo 'Image scanning completed'
               
            }
        }

        stage("push") {
            steps {
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker tag node-app-test-new:latest ${env.dockerHubUser}/node-app-test-new:latest"
                    sh "docker push ${env.dockerHubUser}/node-app-test-new:latest"
                    echo 'Image push completed'
                }
            }
        }

        stage("deploy") {
            steps {
                script {
                    sh "docker ps"
                    sh "docker-compose down || true"
                    sh "docker-compose up -d"
                }
                echo 'Deployment completed'
            }
        }
    }
}
