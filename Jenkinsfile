

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
    }
}  