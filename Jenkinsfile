pipeline{
    agent{
        label "jenkins-agent"
    }
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }

    stages{
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }

        }
    
        stage("Checkout from SCM"){
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/Sefyuuu/complete-prodcution-e2e-pipeline'
            }

        }
        stage("Build Application"){
            steps {
                sh"mvn clean package"
            }

        }
        stage("Test application"){
            steps {
                sh"mvn test"
            }

        }

        stage("sonarqube Analysis"){
            steps {
                script{
                        withSonarQubeEnv(credentialsID: 'jenkins-sonarqube-token'){
                        sh"mvn sonar:sonar"
                    }
                }
                
                
            }
        

        }
        stage("Quality Gate"){
            steps {
                script{
                        waitForQualityGate abortPipeline: false, credentialsID: 'jenkins-sonarqube-token'
                }
                
                
            }
        

        }
        
    }

        
}
