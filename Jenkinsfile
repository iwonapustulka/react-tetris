pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                git url: 'https://github.com/iwonapustulka/react-tetris.git'
                sh 'npm install'
                sh 'git pull origin master'
            }

            post {
                failure {
                    echo 'blad budowania'    
                }
                success {
                    echo 'sukces budowania'
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
                    echo 'blad testow'     
                }
                success {
                    echo 'sukces testow'
                }
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker build -t tetrisgame -f Dockerfile .'
                sh 'docker run tetrisgame'
                }
                post {
                    failure {
                         echo 'blad wdrozenia'    
                    }
                    success {
                        echo 'sukces wdrozenia'                       
                    }
                }
        }
    }
}
