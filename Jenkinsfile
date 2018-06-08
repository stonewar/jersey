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
                                openshift.withCredentials( 'openshift-token' ) {
                                    openshift.withProject('myproject') {
                                        return !openshift.selector('bc', 'openshift-jee-sample').exists();
                                    }
                                }          
                            }  
                        }
                    }
          
             steps {
                    script {
                        openshift.withCluster('mycluster') {
                              openshift.withCredentials( 'openshift-token' ) {
                                    openshift.withProject('myproject') {
                                        openshift.newBuild('--name=openshift-jee-sample', '--image-stream=openshift/wildfly:latest', '--binary')
                                    }
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
