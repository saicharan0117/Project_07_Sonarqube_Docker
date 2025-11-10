pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "my-devops-app:latest"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/saicharan0117/Project_07_Sonarqube_Docker.git', branch: 'main'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {   // Jenkins -> Manage Jenkins -> Configure System -> SonarQube servers
                    script {
                        def scannerHome = tool 'SonarScannerCLI'   // Jenkins -> Manage Jenkins -> Tools -> SonarQube Scanner installations
                        sh 'ls -ltr'
                        sh '''mvn sonar:sonar \
                          -Dsonar.host.url=http://13.218.73.146:9000/ \
                          -Dsonar.login=squ_e1eab6b1c38aa05ef8b341bec43879b0ad077187'''
                
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "sudo docker build -t $DOCKER_IMAGE ."
            }
        }

        stage('Deploy App') {
            steps {
                sh """
                    if sudo docker ps -q --filter "name=my-devops-app" | grep -q .; then
                        sudo docker stop my-devops-app
                        sudo docker rm my-devops-app
                    fi
                    sudo docker run -d --name my-devops-app -p 5000:5000 $DOCKER_IMAGE
                """
            }
        }
    }

    post {
        always {
            echo "Pipeline finished!"
        }
        failure {
            echo "Pipeline failed ❌"
        }
        success {
            echo "Pipeline completed successfully ✅"
        }
    }
}
