pipeline {
    agent any
	tools {nodejs "nodejs"}
    stages {
        stage('Build') {
		
            steps {
		sh 'rm -rf react-tetris'
                sh 'git clone https://github.com/iwonapustulka/react-tetris'
		sh 'cd react-tetris'
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
                        subject: "Failure on building ${currentBuild.fullDisplayName}",
                        body: "Failure on building ${env.BUILD_URL} "      

                }
		    success {
			mail to: 'iwonapustulka@gmail.com',
                        subject: "Success on building ${currentBuild.fullDisplayName}",
                        body: "Success on building ${env.BUILD_URL} " 
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
                        subject: "Failure on testing ${currentBuild.fullDisplayName}",
                        body: "Failure on testing ${env.BUILD_URL} "    
                }
                success {
                    mail to: 'iwonapustulka@gmail.com',
                        subject: "Success on testing ${currentBuild.fullDisplayName}",
                        body: "Success on testing ${env.BUILD_URL} "    
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
                            subject: "Failure deploy: ${currentBuild.fullDisplayName}",
                            body: "Failure deploy ${env.BUILD_URL} "         
                    }
                    success {
                        mail to: 'iwonapustulka@gmail.com',
                            subject: "Success deploy: ${currentBuild.fullDisplayName}",
                            body: "Success deploy ${env.BUILD_URL} "                        
                    }
                }
        }
    }
}
