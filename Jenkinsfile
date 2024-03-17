pipeline{
    agent{
        label "Jenkins-Slave-Agent-Node-Label"
    }
    tools {
        jdk 'Java17'
        maven 'maven3'
    }
    environment{
        APP_NAME = "registration-app-pipeline"
        RELEASE = "1.0.0"
        DOCKER_USER = "princewillopah"
        DOCKER_PASS = "DockerHub-credential-for-Jenkins"
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    }
    stages{
        stage("Cleanup Workspace"){
            steps{
                cleanWs()
            }
        }
        stage("Checkout From SCM"){
            steps{
               git branch: 'master', credentialsId: 'Github-Credentials', url: 'https://github.com/princewillopah/reg-app-CI-withJenkins-CD-withArgoCD2'
            }
        }
        stage("Build Application "){
            steps{
               sh "mvn clean package"
            }
        }

       stage("Test Application "){
            steps{
               sh "mvn test"
            }
        }
       stage("SonarQube Analysis  "){
            steps{
                script {
                    withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token'){ sh "mvn sonar:sonar"}
                   
                }
              
            }
        }

       stage("Build & Push Docker Image "){
            steps{
                script {
                   docker.withRegistry(', DOCKER_PASS'){ docker_image = docker.build "${IMAGE_NAME}"}
                   docker.withRegistry(', DOCKER_PASS'){ 
                                                    docker_image.push("${IMAGE_NAME}")
                                                    docker_image.push('latest')
                                                    }
                }
              
            }
        }

    }// end stages 

}// end pipelines 