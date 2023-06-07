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
       stage(" Docker Build ") {
         steps {
           script {
             echo '<--------------- Docker Build Started --------------->'
                app = docker.build(imageName+":"+version)
                    echo '<--------------- Docker Build Ends --------------->'
        }
      }
    }

    stage ('publish docker image') {
                steps{
                    script{
                        withCredentials([string(credentialsId: 'nexus', variable: 'nexus-cred')]) {
                            
                            sh 'docker login -u admin -p $nexus_cred 8.217.120.92:8083'
                            sh 'docker push 8.217.120.92:8083 imageName+":"+version'
                    }
                        
                         
            
                }
            }                
        }
    }
}       