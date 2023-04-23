pipeline {
  agent any
  tools {
        maven "3.8.6" 
   }

  stages {
      stage('Build Artifact') {
            steps {
              bat "mvn clean package -DskipTests=true"
              archive 'target/*.jar' 
            }  
       }
      stage('Test Maven - JUnit') {
            steps {
              bat "mvn test"
            }
            post{
              always{
                junit 'target/surefire-reports/*.xml'
              }
            }
        }
    
    stage('Code Analysis') {
      steps {
       
          bat 'mvn checkstyle:checkstyle'
        
       /*script {
          def checkName = "Checkstyle"
          def checkTitle = "Checkstyle Results"
          def checkFile = "target/site/checkstyle.html"
          publishChecks([
            [
              checkName: checkName,
              checkTitle: checkTitle,
              summary: readFile(checkFile)
            ]
          ])
        }*/
      }
    }
    stage('Security Scan') {
      steps {
        bat 'mvn dependency:resolve-plugins'
        bat 'mvn dependency:analyze-report'
        bat 'mvn clean compile spotbugs:check'
      }
    }
    stage('Deploy to Staging') {
      steps {
        bat 'echo "Deploying to Staging"'
      }
    }
    stage('Integration Tests on Staging') {
      steps {
         echo "Running Integration Tests on Staging"
      }
    }
    stage('Deploy to Production') {
      steps {
        echo "Deploying to Production"
      }
    }
        
/*
      stage('Sonarqube Analysis - SAST') {
            steps {
                  withSonarQubeEnv('SonarQube') {
           sh "mvn sonar:sonar \
                              -Dsonar.projectKey=maven-jenkins-pipeline \
                        -Dsonar.host.url=http://34.173.74.192:9000" 
                }
           timeout(time: 2, unit: 'MINUTES') {
                      script {
                        waitForQualityGate abortPipeline: true
                    }
                }
              }
        } */
     }
    
}
