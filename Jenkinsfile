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
                            docker build -t springapp:1.0 .
                           
                            """
                    }
                        
                         
            
                }
            }                
       }
        stage('upload jar to nexus'){
            steps {
                script {
                        
            

                    nexusArtifactUploader artifacts: 
                    [
                        [
                         artifactId: 'devops-integration', 
                         classifier: '', file: 'target/devops-integration.jar',
                         type: 'jar'
                         ]
                         
                    ]
                        credentialsId: 'nexus',
                        groupId: 'com.javatechie',
                        nexusUrl: '18.217.120.92:8081',
                        nexusVersion: 'nexus3',
                        protocol: 'http', 
                        repository: docker-hosted, 
                        version: 1.0

                
                }
            }
        }
    }
}  