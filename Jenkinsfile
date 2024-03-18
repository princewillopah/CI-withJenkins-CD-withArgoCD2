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
        JENKINS_API_TOKEN = credentials("JENKINS_API_TOKEN")
    }
    stages{
        stage("Cleanup Workspace"){
            steps{
                cleanWs()
            }
        }//Cleanup Workspace
        stage("Checkout From SCM"){
            steps{
               git branch: 'master', credentialsId: 'Github-Credentials', url: 'https://github.com/princewillopah/reg-app-CI-withJenkins-CD-withArgoCD2'
            }
        }//Checkout From SCM
        stage("Build Application! "){
            steps{
               sh "mvn clean package"
            }
        }//Build Application 

       stage("Test Application "){
            steps{
               sh "mvn test"
            }
        }//Test Application
       stage("SonarQube Analysis !"){
            steps{
                script {
                    withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token'){ sh "mvn sonar:sonar"}
                   
                }
              
            }
        }//SonarQube Analysis 
       stage("Build & Push Docker Image "){
            steps{
                script {
                   docker.withRegistry('', DOCKER_PASS){ docker_image = docker.build "${IMAGE_NAME}"}
                   docker.withRegistry('', DOCKER_PASS){ 
                    docker_image.push("${IMAGE_TAG}")
                    docker_image.push('latest')
                    }
                }//script 
            }// steps
        }// Build & Push Docker Image 
       stage ('Cleanup Artifacts') {// this will clear previously created artifacts(docker-image) from workplace 
           steps {
               script {
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"
               }
          }
       }//Cleanup Artifacts
    //    stage("Trigger GitOps CD Pipeline!!") {
    //         steps {

    //             script {
    //                 sh """
    //                     curl -v -k --user admin:${JENKINS_API_TOKEN} -X POST \
    //                     -H 'cache-control: no-cache' \
    //                     -H 'content-type: application/x-www-form-urlencoded' \
    //                     --data "IMAGE_TAG=${env.IMAGE_TAG}" \
    //                     'ec2-13-51-159-78.eu-north-1.compute.amazonaws.com:8080/job/reg-app-gitops/buildWithParameters?token=gitops-token'
    //                 """
    //                 }

    //         }
    //    }//Trigger CD Pipeline


                stage("Trigger GitOps CD Pipeline!") {
                    steps {
                        script {
                            def response = sh(script: """
                                curl -v -k --user admin:${JENKINS_API_TOKEN} -X POST \
                                -H 'cache-control: no-cache' \
                                -H 'content-type: application/x-www-form-urlencoded' \
                                --data "IMAGE_TAG=${env.IMAGE_TAG}" \
                                'ec2-13-51-159-78.eu-north-1.compute.amazonaws.com:8080/job/reg-app-gitops/buildWithParameters?token=gitops-token'
                            """, returnStdout: true).trim()

                            echo "Response from CD pipeline: ${response}"
                        }
                    }
                }


    }// end stages
}// end pipelines 