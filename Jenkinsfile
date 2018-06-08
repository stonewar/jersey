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
                openshift.withCluster( 'https://192.168.99.101:8443', 'CO8wPaLV2M2yC_jrm00hCmaz5Jgw...' ) {
    				openshift.withProject( 'myproject' ) {
        			echo "Hello from project ${openshift.project()} in cluster ${openshift.cluster()}"
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