pipeline {
    agent any
    tools {nodejs "nodejs"}
    stages {
        stage('Build') {
		
            steps {
		sh 'rm -rf react-tetris'
                sh 'git clone https://github.com/iwonapustulka/react-tetris.git'
		cd 'react-tetris'
		withNPM(npmrcConfig: '87b576b2-39af-47b4-bf8b-76ce74e0591b'){
		sh 'npm install'
		}
		sh 'npm run build'
            }

            post {
                failure {
                    script {
                        env.FAILED = true
                    }  

                    mail to: 'iwonapustulka@gmail.com',
                        subject: "Blad budowania",
                        body: "blad ${env.BUILD_URL} "      

                }
		    success {
			mail to: 'iwonapustulka@gmail.com',
                        subject: "Sukces budowania",
                        body: "sukces ${env.BUILD_URL} " 
                }
            }
        }
        stage('Test') {
             steps {
                 script {
                    if ( env.FAILED ) {
                    expression {
                        currentBuild.result = 'ABORTED'
                        error('Failed on build stage!')
                        }
                    }
                }
            }
            post {
                failure {
                     mail to: 'iwonapustulka@gmail.com',
                        subject: "Blad testowania",
                        body: "blad ${env.BUILD_URL} "    
                }
                success {
                    mail to: 'iwonapustulka@gmail.com',
                        subject: "Sukces testowania",
                        body: "sukces ${env.BUILD_URL} "    
                }
            }
        }
        stage('Deploy') {
            steps {
                bat 'docker build -t tetrisimage -f Dockerfile .'
                bat 'docker run tetrisimage'
                }
                post {
                    failure {
                        mail to: 'iwonapustulkay@gmail.com',
                            subject: "Blad wdrozenia",
                            body: "blad ${env.BUILD_URL} "         
                    }
                    success {
                        mail to: 'iwonapustulka@gmail.com',
                            subject: "Sukces wdrozenia",
                            body: "sukces ${env.BUILD_URL} "                        
                    }
                }
        }
    }
}
