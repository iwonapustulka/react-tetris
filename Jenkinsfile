pipeline {
    agent any
    tools {nodejs "nodejs"}
    stages {
        stage('Build') {
		
            steps {
		sh 'rm -rf react-tetris'
                sh 'git clone https://github.com/chvin/react-tetris.git'
		sh 'cd react-tetris'
		withNPM(npmrcConfig: '876d69e2-718f-4ac3-87ea-5ba59d53c060'){
		sh 'npm install'
		}
		sh 'npm run build'
            }

            post {
                failure {
                    
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
