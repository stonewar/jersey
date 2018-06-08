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
        stage('Create Image Builder') {
              when {
                expression {
                  openshift.withCluster('mycluster') {
                    return !openshift.selector("bc", "jersey").exists();
                  }
                }
              }      
             steps {
                script {
                    openshift.withCluster('mycluster') {
                        openshift.withProject('myproject') {
                            openshift.newBuild("--name=jersey", "--image-stream=openshift/wildfly:latest", "--binary")
                        }
                    }
                }
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
        stage('Deploy') { 
            steps {
                 echo "Deploy stage"
            }
        }
    }
}
