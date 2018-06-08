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

              openshift.withCluster( 'https://192.168.99.101:8443') {
                                    
                                      openshift.withCredentials( 'lG1xc2V-OrGhtN65FYwvTLx_h1QvvfOj8H6SQMRA24E' ) {
    /** Selectors are a core concept in the DSL. They allow the user to invoke operations **/
    /** on group of objects which satisfy a given criteria. **/
   openshift.withProject('myproject') {
    // Create a Selector capable of selecting all service accounts in mycluster's default project
    def saSelector = openshift.selector( 'serviceaccount' )

    // Prints `oc describe serviceaccount` to Jenkins console
    saSelector.describe()

    // Selectors also allow you to easily iterate through all objects they currently select.
    saSelector.withEach { // The closure body will be executed once for each selected object.
        // The 'it' variable will be bound to a Selector which selects a single
        // object which is the focus of the iteration.
        echo "Service account: ${it.name()} is defined in ${openshift.project()}"
    }

    // Prints a list of current service accounts to the console
    echo "There are ${saSelector.count()} service accounts in project ${openshift.project()}"
    echo "They are named: ${saSelector.names()}"

    // Selectors can also be defined using qualified names
    openshift.selector( 'deploymentconfig/frontend' ).describe()

    // Or Kind + Label information
    openshift.selector( 'dc', [ tier: 'frontend' ] ).describe()

    // Or a static list of names
    openshift.selector( [ 'dc/jenkins', 'build/ruby1' ] ).describe()
}
                                      }}
              }}
             }
        stage('Deploy') { 
            steps {
                 echo "Deploy stage"
            }
        }
    }
}
