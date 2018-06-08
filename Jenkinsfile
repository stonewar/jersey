def s2i_builder_name = 'redhat-openjdk18-openshift' 
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
        stage('Connection to openshift') { 
            steps {
                script {
                openshift.withCluster( 'mycluster' ) {
    				openshift.withProject( 'myproject' ) {
        			echo "Hello from project ${openshift.project()} in cluster ${openshift.cluster()}"
    			  }
    			}
            }
          }
        }
        stage('build') {
            steps {
                script {
                    openshift.withCluster('mycluster') {
                    openshift.withProject('myproject') {
                    openshift.newBuild("--name=jersey", "--image-stream=redhat-openjdk18-openshift", "--binary")
                   
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
