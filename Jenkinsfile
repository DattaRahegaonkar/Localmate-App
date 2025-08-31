pipeline {
    agent any
    environment {
        SONAR_HOME = tool "Sonar"
    }
    stages {
        stage("Cleaning the Workspace") {
            steps {
                cleanWs()
            }
        }
        stage('Cloning The Code From Github') {
            steps {
                git url: "https://github.com/DattaRahegaonkar/localmate-app.git", branch: "demo"
            }
        }
        stage('Scanning the Code Using SonarQube') {
            steps {
                withSonarQubeEnv("Sonar") {
                    sh "${SONAR_HOME}/bin/sonar-scanner -Dsonar.projectName=Django-App -Dsonar.projectKey=Django-App "
                }
            }
        }
        stage("OWASP Dependency Check") {
            steps {
                dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'OWASP'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        stage("Scanning File System Using Trivy") {
            steps {
                sh "trivy fs . --format table -o trivy-fs-report.html"
            }
        }
        stage("Building the Docker Image") {
            steps {
                sh 'docker build -t django-app .'
            }
        }
        stage("Docker Image Scanning Using Trivy") {
            steps {
                sh 'trivy image django-app --format table -o trivy-img-report.html --exit-code 0 --severity HIGH,CRITICAL || true'
            }
        }
        stage("Pushing the Image on Docker Hub Repository") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "docker_hub_creds",
                    usernameVariable: "docker_hub_user",
                    passwordVariable: "docker_hub_password"
                    )]) {
                        sh "docker login -u ${docker_hub_user} -p ${docker_hub_password}"
                        sh "docker tag django-app ${docker_hub_user}/django-app:latest"
                        sh "docker push ${docker_hub_user}/django-app:latest"
                    }
            }
        }
        stage("Deploying Using Docker Compose") {
            steps {
                sh "docker rm -f django-app || true"
                sh "docker-compose up -d"
                
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
