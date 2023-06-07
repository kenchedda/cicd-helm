def registry = 'http://18.217.120.92:8081/repository/docker-hosted/'
def imageName = 'springapp'
def version   = '1.0.2'

pipeline{
    agent{
        label "build"
    }
   
    
   
    stages{
        stage("sonar quality status"){
            
            steps{
                script{
                       withSonarQubeEnv(credentialsId: 'sonar') {
                        sh 'mvn clean package sonar:sonar'
} 
                }
            }
        }
        stage("quality gate status"){
            steps{
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar'
                }
            }
        }
      

    stage ('publish docker image') {
                steps{
                    script{
                        withCredentials([string(credentialsId: 'nexus', variable: 'nexus_cred')]){
                            sh """
                            docker build -t 18.217.120.92:8083/springapp:1.0 .
                           
                            """
                    }
                        
                         
            
                }
            }                
       }
       stage ('nexus push') {
        steps{
                nexusArtifactUploader(
                   nexusVersion: 'nexus3',
                   protocol: 'http',
                   nexusUrl: '18.217.120.92:8081',
                   groupId: 'com.javatechie',
                   version: 1.0,
                   repository: 'docker-hosted',
                   credentialsId: 'nexus',
                   artifacts: [
                    [artifactId: devops-integration,
                     classifier: '',
                     file: 'target/devops-integration-v1.0.jar',
                     type: 'jar']
        ]
     )

            }
        }
       }
    }
      