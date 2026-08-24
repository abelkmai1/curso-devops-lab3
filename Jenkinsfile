pipeline {

	agent any

    stage {
		stage('CI - Integración continua') {
			agent {
					docker {
						image "node:24"
						reuseNode true	
					}
			} 

			stages {
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
		stage('CD - Distribuir Image docker') {
			steps {
				sh 'docker build -t curso-devops-lab3:latest .'
			}
		}	
	}	
}	
