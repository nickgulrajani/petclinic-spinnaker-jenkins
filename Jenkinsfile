pipeline {
    agent any
       triggers {
        pollSCM "* * * * *"
       }
    stages {
        stage('Build Application') { 
            steps {
                echo '=== Building Petclinic Application ==='
                sh 'mvn -B -DskipTests clean package' 
            }
        }
        stage ('Vulnerability Test'){
          steps{
              dir('/home/ubuntu/GITHUBREPOS/JAVA-MVN/NEWPETCLINIC/petclinic-spinnaker-jenkins')
            {
              sh "pwd"
              sh "/usr/local/bin/snyk monitor"
              snykSecurity organisation: 'nickgulrajani', snykInstallation: 'snyk', snykTokenId: 'snyktoken'
            }
      }
      }
        stage('Test Application') {
            steps {
                echo '=== Testing Petclinic Application ==='
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Build Docker Image') {
            when {
                branch 'master'
            }
            steps {
                echo '=== Building Petclinic Docker Image ==='
                script {
                    app = docker.build("nicholasgull/petclinic-spinnaker-jenkins")
                }
            }
        }
    }
}
