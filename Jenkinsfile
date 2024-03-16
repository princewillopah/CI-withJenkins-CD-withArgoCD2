pipeline{
    agent{
        label "Jenkins-Slave-Agent-Node-Label"
    }
    tools {
        jdk 'Java17'
        maven 'maven3'
    }
    stages{
        stage("Cleanup Workspace"){
            steps{
                cleanWs()
            }
            post{
                always{
                    echo "========always========="
                }
                success{
                    echo "======== Cleanup Workspace executed successfully========"
                }
                failure{
                    echo "======== Cleanup Workspace execution failed========"
                }
            }
        }
        stage("Checkout From SCM"){
            steps{
               git branch: 'master', credentialsId: 'Github-Credentials', url: 'https://github.com/princewillopah/reg-app-CI-withJenkins-CD-withArgoCD2'
            }
            post{
                always{
                    echo "========always========"
                }
                success{
                    echo "======== Checkout From SCM executed successfully========"
                }
                failure{
                    echo "======== Checkout From SCM execution failed========"
                }
            }
        }
        stage("Build Application "){
            steps{
               sh "mvn clean package"
            }
            post{
                always{
                    echo "========always========"
                }
                success{
                    echo "======== Build Application  executed successfully========"
                }
                failure{
                    echo "======== Build Application  execution failed========"
                }
            }
        }

       stage("Test Application "){
            steps{
               sh "mvn test"
            }
            post{
                always{
                    echo "========always========"
                }
                success{
                    echo "======== Test Application  executed successfully========"
                }
                failure{
                    echo "======== Test Application  execution failed========"
                }
            }
        }
       stage("SonarQube Analysis  "){
            steps{
                script {
                    withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token'){ sh "mvn sonar:sonar"}
                   
                }
              
            }
            post{
                always{
                    echo "========always========"
                }
                success{
                    echo "======== Test Application  executed successfully========"
                }
                failure{
                    echo "======== Test Application  execution failed========"
                }
            }
        }

        //         stage("Quality Gate") {
        //             steps {
        //             //  script {
        //             //       waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'
        //             //    }

        //                 script {
        //                     def qg = waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'
        //                     if (qg.status == 'OK') {
        //                         echo 'Quality Gate passed - SonarQube analysis successful'
        //                     } else if (qg.status == 'ERROR') {
        //                         echo 'Quality Gate failed - SonarQube analysis did not meet the criteria'
        //                         // You can perform additional actions here if needed
        //                     } else {
        //                         echo "Quality Gate status is '${qg.status}', waiting for completion"
        //                         // You can choose to wait or proceed with other actions based on the status
        //                     }
        //                 }
        //             }
                
        //     post{
        //         always{
        //             echo "========always========"
        //         }
        //         success{
        //             echo "======== Test Application  executed successfully========"
        //         }
        //         failure{
        //             echo "======== Test Application  execution failed========"
        //         }
        //     }
        // }
    }// end stages 

}// end pipelines 