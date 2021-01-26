pipeline {
  agent any
  tools {
    maven 'Maven'
  }
  stages {
    stage ('initialize') {
      steps {
        sh '''
                echo "PATH = ${PATH}"
                echo "M2_HOME = ${M2_HOME}"
            '''
      }
    }
    /*
    stage ('Check-Git-Secrets') {
      steps {
        sh 'rm trufflehog || true'
        sh 'docker run gesellix/trufflehog --json https://github.com/Hazraf/YogaSystem.git > trufflehog'
        sh 'cat trufflehog'
      }
    }
    
    stage ('Software Composition Analysis') {
      steps {
         sh 'rm -r dependency-check* || true' 
         sh 'wget https://github.com/jeremylong/DependencyCheck/releases/download/v6.0.3/dependency-check-6.0.3-release.zip'
         sh 'unzip dependency-check-6.0.3-release.zip'
         sh './dependency-check/bin/dependency-check.sh --scan ./src/* --enableRetired -f "ALL" '
      }
    }
    
    stage ('SAST') {
      steps {
        withSonarQubeEnv('sonar') {
          sh 'mvn sonar:sonar'
          sh 'cat target/sonar/report-task.txt'
        }
      }
    }
    */
    stage ('Build') {
      steps {
      sh 'mvn clean package'
      }
    }
    
    stage ('Deploy-To-Tomcat') {
            steps {
                sh 'cp target/*.war /prod/apache-tomcat-8.5.61/webapps/webapp.war'     
           }       
    }
  
    stage ('DAST') {
      steps {
         sh '"docker run -t owasp/zap2docker-stable zap-baseline.py -t http://localhost:8082/webapp/" || true'
      }
    }
  }
}
