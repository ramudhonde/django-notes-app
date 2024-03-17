pipeline{
    agent any
    
    stages{
        stage("Clone") {
            steps{
                echo "Cloning the code"
                git url: "https://github.com/ramudhonde/django-notes-app.git", branch:"main"
            }
        }
        stage("Build") {
            steps{
                echo "Building the image"
                sh "docker build -t notes-app ."
            }
        }
        stage("Push") {
            steps{
                echo "Pushing image build container"
                withCredentials([usernamePassword(credentialsId:"hubDocker",passwordVariable:"hubDockerPass",usernameVariable:"hubDockerUser")]){
                sh "docker tag notes-app ${env.hubDockerUser}/notes-app"
                sh "docker login -u ${env.hubDockerUser} -p ${env.hubDockerPass}"
                sh "docker push ${env.hubDockerUser}/notes-app"
                }
            }
        }
        stage("Deploy") {
            steps{
                echo "Deploying image"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
