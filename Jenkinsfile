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
                stage('Build Docker Image') {  
                steps{                     
                   bat 'docker-compose up'   
                    }
                }
            }
            }
            stage('SonarQube Analysis') {
                steps {
                    script {
                        def mvnHome = tool 'M3'
                        withSonarQubeEnv() {
                            bat "${mvnHome}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=mysql-sonarqube"
                        }
                    }
                }
            }
            stage('Packaging') {
                steps {
                    step([$class: 'ArtifactArchiver', artifacts: '**/target/*.jar', fingerprint: true])
                }
            }
            
            stage ("Artifactory Publish"){
                steps{
                    script{
                            def server = Artifactory.server 'artifactory'
                            def rtMaven = Artifactory.newMavenBuild()
                            //rtMaven.resolver server: server, releaseRepo: 'jenkins-devops', snapshotRepo: 'jenkins-devops-snapshot'
                            rtMaven.deployer server: server, releaseRepo: 'mysql-1', snapshotRepo: 'mysqlsnap'
                            rtMaven.tool = 'M3'
                            
                            def buildInfo = rtMaven.run pom: '$workspace/pom.xml', goals: 'clean install'
                            rtMaven.deployer.deployArtifacts = true
                            rtMaven.deployer.deployArtifacts buildInfo

                            server.publishBuildInfo buildInfo
                    }
                }
        }
        }
}
