pipeline {

    agent {
        label 'worker-linux'
    }

    stages {
        stage('Compilacion') {
            steps {
               sh 'mvn -DskipTests clean install package'

               script {
                    def props
                    props = readProperties file: 'versions/version.properties'

                    echo "My tag is ${props['version']}"
               }

            }
        }

        stage('Unit test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                   junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Procesos docker - Build Tag y Push') {
            steps {
                sh 'mvn compile jib:build'
            }
        }
        

    }
}
