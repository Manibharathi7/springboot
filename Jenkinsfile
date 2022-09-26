// multistage
pipeline {
    agent any

        stages {
            stage('Source') {
                steps {
                    git url: 'https://github.com/Manibharathi7/springboot.git'
                }
            }
            stage('Build Docker Image') {  
                steps{                     
                   bat 'docker-compose up'   
                    }
                }
            }
}
