def registry = 'http://18.217.120.92:8081/'
def imageName = 'http://18.217.120.92:8081/repository/docker-hosted/springapp'
def version   = '${BUILD_ID}'

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
                app = docker.build(imageName+":"version)
                    echo '<--------------- Docker Build Ends --------------->'
        }
      }
    }

    stage (" Docker Publish "){
        steps {
            script {
               echo '<--------------- Docker Publish Started --------------->'  
                docker.withRegistry(registry, 'nexus'){
                    app.push()
                }    
               echo '<--------------- Docker Publish Ended --------------->'  
            }
        }
    }
    }
}       