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
                CleanWs()
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
    }
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}