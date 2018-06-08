pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
    stages {
        stage('Build') { 
            steps {
              sh "mvn install"
            }
        }
        stage('Test') { 
            steps {
                echo "Test stage"
            }
        }
        stage('Deploy') { 
            steps {
                 echo "Deploy stage"
            }
        }
    }
}