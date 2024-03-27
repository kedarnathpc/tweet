def registry = 'https://miniproject3.jfrog.io'
def imageName = 'miniproject3.jfrog.io/miniproject3-docker-local/ttrend'
def version   = '2.1.2'

pipeline {
    agent {
        node {
            label 'maven'
        }
    }
environment{
    PATH = "/opt/apache-maven-4.0.0-alpha-10/bin:$PATH"
}
    stages {
        stage("build"){
            steps{
                echo "----------build test started-------------"
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo "----------build test completed-------------"
            }
        }

        stage("test"){
            steps{
                echo "----------unit test started-------------"
                sh 'mvn surefire-report:report'
                echo "----------unit test completed-------------"
            }
        }

        stage('SonarQube analysis') {
            environment {
                scannerHome = tool 'miniproject-sonar-scanner'
            }
            steps {
                withSonarQubeEnv('miniproject-sonarqube-server') {
                    sh "${scannerHome}/bin/sonar-scanner -X"
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

        stage (" Docker Publish "){
            steps {
                script {
                echo '<--------------- Docker Publish Started --------------->'  
                    docker.withRegistry(registry, 'artifact-cred'){
                        app.push()
                    }    
                echo '<--------------- Docker Publish Ended --------------->'  
                }
            }
        }
    }
}