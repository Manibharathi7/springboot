// multistage
pipeline {
    agent any

        stages {
            stage('Source') {
                steps {
                    git url: 'https://github.com/Manibharathi7/springboot.git'
                }
            }
            stage('Build') {
                steps {
                    script {
                        def mvnHome = tool 'M3'
                        bat "${mvnHome}\\bin\\mvn -B verify"
                    }
                }
            }
        }
            
