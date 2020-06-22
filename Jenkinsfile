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

        stage ('Code Quality') {

            steps {
                    sh '/usr/local/bin/sonar-scanner-3.3.0.1492-linux/bin/sonar-scanner'
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
