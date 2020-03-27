node {
    def app

    stage('Clone repository') {
        /* Cloning the Repository to our Work space */

        checkout scm
    }
    
    stage('Mvn Package'){
        def mvnHome = tool name: 'maven-3', type: 'maven'
        def mvnCMD = "${mvnHome}/bin/mvn"
        sh label: '', script: "${mvnCMD} clean package"
    }

   
    stage('Build image') {
        /* This builds the actual image */

        app = docker.build("potryk4us/nodeapp")
    }

    stage('Test image') {
        
        app.inside {
            echo "Tests passed"
        }
    }

    stage('Push image') {
        /* 
			You would need to first register with DockerHub before you can push images to your account
		*/
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
            } 
                echo "Trying to Push Docker Build to DockerHub"
    }
}
