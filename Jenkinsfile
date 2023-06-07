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
                            docker build -t http://8.217.120.92:8083/springapp:1.0 .
                            docker login -u admin -p $nexus_cred http://8.217.120.92:8083
                            docker push http://8.217.120.92:8083//springapp:1.0
                            """
                    }
                        
                         
            
                }
            }                
        }
    }
}       