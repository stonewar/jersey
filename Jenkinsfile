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
             steps {
                    script {
                        openshift.withCluster() {
                           
                                    openshift.withProject('myproject') {
                                        openshift.newBuild('--name=openshift-jee-sample', '--image-stream=openshift/wildfly:latest', '--binary')
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
