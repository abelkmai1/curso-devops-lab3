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
							echo "La version de la app es: ${env.APP_SEMANTIC_VERSION}"
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
		stage('CD - Construir Imagen ') {
			steps {
				sh 'docker build -t curso-devops-lab3:latest .'
				sh 'docker tag curso-devops-lab3 diegovilla123/curso-devops-lab3:latest'
				sh 'docker tag curso-devops-lab3 diegovilla123/curso-devops-lab3:0.0.1'
				sh 'docker tag curso-devops-lab3 diegovilla123/curso-devops-lab3:1'
				sh 'docker tag curso-devops-lab3 ghcr.io/abelkmai1/curso-devops-lab3:latest'
				sh 'docker tag curso-devops-lab3 ghcr.io/abelkmai1/curso-devops-lab3:0.0.1'
				sh 'docker tag curso-devops-lab3 ghcr.io/abelkmai1/curso-devops-lab3:1'
			}
		}	
		stage('CD - Distribuir Image dockerhub') {
			steps {
				script{
					docker.withRegistry('https://index.docker.io/v1/','dh-credencial') {				
					sh 'docker push diegovilla123/curso-devops-lab3:latest'
					sh 'docker push diegovilla123/curso-devops-lab3:0.0.1'
					sh 'docker push diegovilla123/curso-devops-lab3:1'
					}
				}
			}
		}	
		stage('CD - Distribuir Image github') {	
			steps {
				script{
					docker.withRegistry('https://ghcr.io','gh-credencial') {				
					sh 'docker push ghcr.io/abelkmai1/curso-devops-lab3:latest'
					sh 'docker push ghcr.io/abelkmai1/curso-devops-lab3:0.0.1'
					sh 'docker push ghcr.io/abelkmai1/curso-devops-lab3:1'
					}
				}
			}
		}	
	}	
}	
