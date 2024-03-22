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
    }
}