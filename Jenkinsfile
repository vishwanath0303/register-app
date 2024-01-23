pipeline {
    agent any 
    
    tools{
        jdk 'jdk11'
        maven 'maven3'
    }
    
    stages{
        
        stage("Git Checkout"){
            steps{
                git branch: 'main', url: 'https://github.com/vishwanath0303/register-app.git'
            }
        }
        
        stage("Maven Build"){
            steps{
                sh "mvn clean install"
            }
        }
        
         stage("Test Cases"){
            steps{
                sh "mvn test"
            }
        }

     stage('Sonar-Analysis') {
            steps {
                sh 'mvn clean package'
                sh ''' mvn sonar:sonar  -Dsonar.url=http://13.48.147.238:9000/ -Dsonar.login=squ_c6d8c9a01b77015a648fe83324abcfa2cc6ae10a  -Dsonar.projectName=register \
                    -Dsonar.java.binaries=. \
                    -Dsonar.projectKey=register '''
            }
     }
    
       
      
         stage('Build docker image'){
            steps{
                script{
                       sh 'docker build -t register:$BUILD_NUMBER .'

                
            }
       }
      }
        

  
      stage('Start image'){
            steps{
                 script{
                  
                     sh 'docker run -d -p 8082:8082 --name register register:$BUILD_NUMBER '          
             }
           }
          }
