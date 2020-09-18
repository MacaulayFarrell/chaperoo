pipeline{
        agent any
        environment {
            app_version = 'v1'
            rollback = 'true'
        }
        stages{
            stage('Build Image'){
                steps{
                    script{
                        if (env.rollback == 'false'){
                            image = docker.build("maccpr7/chaperoo-frontend")
                        }
                    }
                }          
            }
            stage('Tag & Push Image'){
                steps{
                    script{
                        if (env.rollback == 'false'){
                            docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials'){
                                image.push("${env.app_version}")
                            }
                        }
                    }
                }          
            }
          stage('Back-end') {
            agent {
                docker { image 'python:3.7' }
            }
            steps {
                sh 'python --version'
            }
        }
            stage('Deploy App'){
                steps{
                    sh "docker-compose up -d"
                }
            }
        }    
}
