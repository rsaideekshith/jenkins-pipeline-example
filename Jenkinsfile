pipeline {

        agent any

        tools {
                maven "my_maven"
                jdk "my_jdk"
        }

	environment {
		DOCKERHUB_CREDENTIALS = credentials('dockerhub')
	}

        stages {
                stage('Initialise') {
                        steps {
                                echo "PATH = ${M2_HOME}/bin:${PATH}"
                                echo "M2_HOME = /opt/maven"
                        }
                }
                stage('Build') {
                        steps {
                                dir("/var/lib/jenkins/workspace/javabuildpipeline/my-app") {
                                        sh 'mvn -B -DskipTests clean package'
					sh 'echo "Jar file generated in the target folder....."'
				}
			}
		}
		stage('Build Docker Image') {
			steps {
				dir("${WORKSPACE}/my-app") {
					sh 'echo "Building Docker Image...."'
					sh 'docker build -t rsaideekshith123/myjavaapp:${BUILD_NUMBER} .'
					sh 'echo "Docker Image built successfully...."'
                                }
                        }
                }
		stage('Push Docker Image') {
			steps {
				sh 'echo $dockerhub_PSW | docker login -u $dockerhub_USR --password-stdin'
				sh 'docker push rsaideekshith123/myjavaapp:${BUILD_NUMBER}'

        }

    }

  }

 }



