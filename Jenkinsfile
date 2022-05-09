pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "MAVEN"
    }

    stages {
        stage('Check SCM') {
            steps {
                // Get some code from a GitHub repository
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/zahir012/java_app_deploy_tomcar_using_jenlkins_CI-CD.git']]])

            }
        }    
        
        stage('Build maven package') {
            steps {
                sh 'mvn package'   
            }
        }

        stage('Build docker image') {
            
            steps {
                
                script {
                    
                    sh 'docker build -t zahirul012/java-web-app-2.0 .'
                }
            }
        }
           
        stage('Deploy to docker') {
            
            steps {
                
                script {
                    
                    sh 'docker run -d --name myweb -p 8989:8080 zahirul012/java-web-app-2.0'
                }
            }
        }    
        
        stage('Push dokcer image to docker hub') {
            
            steps {
                
                script {
                    
                    withCredentials([string(credentialsId: 'docker', variable: 'docker')]) {
                        
                    sh 'docker login -u zahirul012 -p ${docker}'
                    
}                  
                    sh 'docker push zahirul012/java-web-app-2.0'
                    
                }
            }
     
        }
        
        
    }
        post {
            
            always {
                
                sh 'docker logout'
            }
       }
    
}
