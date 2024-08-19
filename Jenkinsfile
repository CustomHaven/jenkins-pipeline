pipeline {
	agent any
	// agent {
    //     docker {
    //         image 'maven:3.6.3'
    //     }
    // }
	environment {
		dockerHome = tool "myDocker"
		mavenHome = tool "myMaven"
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
	}
	stages {
		stage('Checkout') {
			steps {
				sh 'mvn --version'
				echo 'docker-here'
				sh 'docker --version'
				echo "Build"
				echo "Path: $Path"
				echo "Build Number: $env.BUILD_NUMBER"
				echo "Build ID: $env.BUILD_ID"
				echo "Build URL: $env.BUILD_URL"
				echo "Build Tag: $env.BUILD_TAG"
				echo "Job Name: $env.JOB_NAME"
			}
			post {
				always {
					echo 'I run at the end of the build stage'
				}
			}
		}
		stage('Compile') {
			steps {
				sh "mvn clean compile"
			}
		}
		stage('Test') {
			steps {
				sh "mvn test"
			}
		}
		stage('Package') {
			steps {
				sh 'mvn package -DskipTest'
			}
		}
		stage('Build Docker Image') {
			steps {
				script {
					dockerImage = docker.build("customhaven/currency-exchange-java:${env.BUILD_TAG}")
				}
				
			}
		}
		stage('Push Docker Image') {
			steps {
				script {
					docker.withRegistry('', 'dockerhub') {
						dockerImage.push()
						dockerImage.push("latest")
					}
				}
			}
		}
		
		stage('Integration Test') {
			steps {
				echo "Integration Test"
			}
		}
	}
	post {
		always {
			echo 'I always run'
		}
		success {
			echo 'I run when successful'
		}
		failure {
			echo 'I run when failed'
		}
	}
}