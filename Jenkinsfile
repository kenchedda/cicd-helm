

pipeline{
    agent{
        label "build"
    }
    
     environment {
        VERSION ="${env.BUILD_ID}"
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
      

    stage ('docker build and docker push') {
        steps{
            script{
                withCredentials([string(credentialsId: 'nexus', variable: 'nexus_cred')]) {
                    sh """
                    docker build -t 18.223.255.119:8083/springapp:${VERSION} .
                    docker login -u admin -p $nexus_cred 18.223.255.119:8083
                    docker push 18.223.255.119:8083/springapp:${VERSION} 
                    docker rmi  18.223.255.119:8083/springapp:${VERSION} 
                    """
         }       
            }
        }
        stage('identifying misconfigs using datree in helm'){
            steps{
                script{
                    dir('kubernetes/myapp') {
                      sh 'helm datree test .'
}
                }
            }
        }

        post {
		  always {
			mail bcc: '', body: "<br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "${currentBuild.result} CI: Project name -> ${env.JOB_NAME}", to: "kenappiah@gmail.com";  
		}
	}
     
    }
}
}  