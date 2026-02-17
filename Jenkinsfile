pipeline{
    agent { label "dev" }
    
    stages{
        stage("code"){
            steps{
                git url : "https://github.com/jkpatelnx/two-tier-flask-app.git", branch: "main"
                echo "clone the code from github"
            }
        }
        stage("Trivy File System Scan"){
            steps{
                trivy fs . -o results.json
                echo "Trivy File System Scan Output Saved in results.json"
            }
        }
        stage("build"){
            steps{
                sh "docker build -t two-tier-flask-app -f docker/Dockerfile ."
                echo "build the Docker image"
            }
        }
        stage("test"){
            steps{
                echo "developer / tester will write this"
            }
        }
        stage("push to Docker hub"){
            steps{
                withCredentials([usernamePassword(
                        credentialsId: "dockerHubCred",
                        passwordVariable: "DOCKER_PASS",
                        usernameVariable: "DOCKER_USER",
                    )]){
                    
                    sh "docker login -u ${env.DOCKER_USER} -p ${env.DOCKER_PASS}"
                    sh "docker image tag two-tier-flask-app ${env.DOCKER_USER}/two-tier-flask-app"
                    sh "docker push ${env.DOCKER_USER}/two-tier-flask-app"
                    
                }
                echo "push the docker image to docker hub"
            }
        }
        stage("deploy"){
            steps{
                sh "docker compose -f compose/docker-compose.yml up -d"
                echo "deploy using docker compose"
            }
        }
    }

post{
    success{
        script{
            emailext from: 'jkpatelnx@gmail.com',
            to: 'jkpatelnx@gmail.com',
            body: 'Build success from two-tier-flask-app',
            subject: 'Build success from two-tier-flask-app'
        }
    }
    failure{
        script{
            emailext from: 'jkpatelnx@gmail.com',
            to: 'jkpatelnx@gmail.com',
            body: 'Build failed from two-tier-flask-app',
            subject: 'Build failed from two-tier-flask-app'
        }
    }

}
    
}

