@Library("Shared")_
pipeline{
    agent { label "dev" }
    
    stages{
        stage("code"){
            steps{
                script{
                    clone("https://github.com/jkpatelnx/two-tier-flask-app.git","main")
                }
            }
        }
        stage("Trivy File System Scan"){
            steps{
                script{
                    trivy_scan()
                }
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
                script{
                    docker_push("dockerHubCred","two-tier-flask-app")
                }
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

