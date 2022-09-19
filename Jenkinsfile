node{
    stage('Checkout'){


        git branch: 'master', url: 'https://github.com/mbenguep/webapp.git'
    }
    stage('compile-package'){
        // getting maven home path

        sh "/opt/maven/bin/mvn package"
    }
    stage('Clean Package') {
        echo 'Code Quality'
        withSonarQubeEnv('sonar-2') { 
          sh "/opt/maven/bin/mvn clean package"
        }
    }
    stage('SonarQube Analysis') {
        echo 'Code Quality'
        withSonarQubeEnv('sonar-2') { 
          sh "/opt/maven/bin/mvn sonar:sonar"
        }
    }
