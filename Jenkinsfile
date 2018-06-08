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
              sh 'mvn install'
            }
        }
        stage('Test') { 
            steps {
                script {
                openshift.withCluster( 'https://192.168.99.101:8443', 'lG1xc2V-OrGhtN65FYwvTLx_h1QvvfOj8H6SQMRA24E' ) {
    				openshift.withProject( 'myprojectd' ) {
        			echo "Hello from project ${openshift.project()} in cluster ${openshift.cluster()}"
    			}
    			}
            }
          }
        }
        stage('Deploy') { 
            steps {
                 echo "Deploy stage"
            }
        }
    }
}