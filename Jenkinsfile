pipeline {

	agent any

    stages {
		stage('CI - Integración continua') {
			agent {
					docker {
						image "node:24"
						reuseNode true	
					}
			} 
			stages {
				stage('CI - obtener version de app'){
					steps{
						script{
							env.APP_SEMANTIC_VERSION = sh(
								script: 'npm pkg get version | tr -d \'"\'',
								returnStdout: true
							).trim()

							env.APP_BUILD_NUMBER = env.BUILD_NUMBER

							echo "La version semantica del app es: ${env.APP_SEMANTIC_VERSION}"
							echo "El build number del app es: ${env.APP_BUILD_NUMBER}"
							
						}
					}
				}
				stage('CI - instalar dependencias'){
					steps {
						sh 'npm install'
					}
				}
				stage('CI - ejecutar el linter'){
					steps {
						sh 'npm run lint'
					}
				}
				stage('CI - ejecutar los test'){
					steps {
						sh 'npm run test'
					}
				}
				stage('CI - construir o build'){
					steps {
						sh 'npm run build'
					}
				}
			}
		}
		stage('Quality Assurance'){
			agent {
				docker{
					image "sonarsource/sonar-scanner-cli'
					reuseNode true	
				}		
			}
		
		stages{
			stage('Validacion de codigo'){
				steps {
					withSonarQubeEnv('sonarqube') {
						sh 'sonar-scanner'
						}
					}
				}
			}

		}
		stage('CD - Construir Imagen ') {
			steps {
				sh 'docker build -t curso-devops-lab3:latest .'
				sh 'docker tag curso-devops-lab3 diegovilla123/curso-devops-lab3:latest'
				sh "docker tag curso-devops-lab3 diegovilla123/curso-devops-lab3:${env.APP_SEMANTIC_VERSION}"
				sh "docker tag curso-devops-lab3 diegovilla123/curso-devops-lab3:${env.APP_BUILD_NUMBER}"
				sh 'docker tag curso-devops-lab3 ghcr.io/abelkmai1/curso-devops-lab3:latest'
				sh "docker tag curso-devops-lab3 ghcr.io/abelkmai1/curso-devops-lab3:${env.APP_SEMANTIC_VERSION}"
				sh "docker tag curso-devops-lab3 ghcr.io/abelkmai1/curso-devops-lab3:${env.APP_BUILD_NUMBER}"
			}
		}	
		stage('CD - Distribuir Image dockerhub') {
			steps {
				script{
					docker.withRegistry('https://index.docker.io/v1/','dh-credencial') {				
					sh 'docker push diegovilla123/curso-devops-lab3:latest'
					sh "docker push diegovilla123/curso-devops-lab3:${env.APP_SEMANTIC_VERSION}"
					sh "docker push diegovilla123/curso-devops-lab3:${env.APP_BUILD_NUMBER}"
					}
				}
			}
		}	
		stage('CD - Distribuir Image github') {	
			steps {
				script{
					docker.withRegistry('https://ghcr.io','gh-credencial') {				
					sh 'docker push ghcr.io/abelkmai1/curso-devops-lab3:latest'
					sh "docker push ghcr.io/abelkmai1/curso-devops-lab3:${env.APP_SEMANTIC_VERSION}"
					sh "docker push ghcr.io/abelkmai1/curso-devops-lab3:${env.APP_BUILD_NUMBER}"
					}
				}
			}
		}	
	}	
}	
