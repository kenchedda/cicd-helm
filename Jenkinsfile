pipeline{
    agent{
        label "build"
    }
    stages{
        stage("sonar quality status"){
            agent{
                docker{
                    image 'maven'
                }
            }
            steps{
                script{
                       withSonarQubeEnv(credentialsId: 'sonar') {
                        sh 'mvn clean package sonar:sonar'
} 
                }
            }
        }
    }
}       