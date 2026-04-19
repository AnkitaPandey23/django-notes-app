pipeline{
    agent { label "dev"};
    stages{
        stage("Code"){
            steps{
                echo "Get the latest code"
                git url: "https://github.com/AnkitaPandey23/django-notes-app.git", branch: "main"
            }
        }
        stage("Build"){
            steps{
                echo "Build the image"
                sh "docker build -t notesapp ."
            }
        }
        stage("Push the image to docker hub"){
            steps{
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag notesapp ${env.dockerHubUser}/notesapp:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/notesapp:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "Deploying the conatiner"
                sh "docker-compose down && docker-compose up -d --build"
            }
        }
    }

post{
        success{
            script{
                emailext from: 'itsakku09@gmail.com',
                to: 'itsakku09@gmail.com',
                body: 'Build success for Django Notes App',
                subject: 'Build success for Django Notes App'
            }
        }
        failure{
            script{
                emailext from: 'itsakku09@gmail.com',
                to: 'itsakku09@gmail.com',
                body: 'Build Failed for Django Notes App',
                subject: 'Build Failed for Django Notes App'
            }
        }
    }
}
