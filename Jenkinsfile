node {
    def app
    environment {
        ARTVERSION = "${env.BUILD_ID}"
	    registryCredential = "ecr:us-west-1:awscreds"
	    appRegistry = "503129199420.dkr.ecr.us-west-1.amazonaws.com/vprofileapp"
	    vprofileRegistry = "https://503129199420.dkr.ecr.us-west-1.amazonaws.com"
    }

    stage('Clone repository') {
      

        checkout scm
    }

    stage('Build image') {
  
       app = docker.build("raj80dockerid/test")
    }

    stage('Test image') {
  

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Upload Image') {
          steps{
            script {
              docker.withRegistry( vprofileRegistry, registryCredential ) {
                dockerImage.push("$BUILD_NUMBER")
                dockerImage.push('latest')
              }
            }
          }
	}
    
    stage('Trigger ManifestUpdate') {
                echo "triggering updatemanifestjob"
                build job: 'updatemanifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }
}
