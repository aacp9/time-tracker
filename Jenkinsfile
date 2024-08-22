pipeline {
    agent any

    tools{
        maven "Maven3"
        jdk "Java11"
    }
    
    environment{
        FLAG_GREEN ='true'
        FLAG_RED='false'
    }
    stages {
        stage('Hello') {
            steps {
                echo "Valor bandera VERDE: ${FLAG_GREEN}"
                echo "Valor bandera ROJA : ${FLAG_RED}"
                echo "Ejemplos de variables de entorno"
                echo "Job Name: ${env.JOB_NAME}"
                echo "JAVA_HOME Value: ${env.JAVA_HOME}"
                echo "MAVEN_HOME Value: ${env.MAVEN_HOME}"
            }
        }
        stage('descar_código_Git_Polling'){
            steps{
                //Acceso público
                //git branch: 'main', url:'https://github.com/aacp9/time-tracker.git'
                
                //acceso con tunel ssh
                git branch: 'main', credentialsId: 'jenkins-Github',
                url: 'git@github.com:aacp9/time-tracker.git'
                
            }
        }
        stage('Build con Maven'){
            steps{
                //para windows
                //bat "mvn clean package"
                // ignorar test
                //bat "mvn clean package -DskipTests"
                
                //para Ubuntu e ignora test
                sh "mvn -Dmaven.test.failure.ignore=true clean package "
            }
            post{
                //los artefactos son war,ear,jar, archivos comprimidos que se utilizan para el despliegue de la aplicación
                //condicioneles de post
                //always, chaged, fixed, regression, aborted, failure, success
                success{
                    echo 'Archivar Artefactos'
                    archiveArtifacts "core/target/*.jar"
                    archiveArtifacts "web/target/*.war"
                }
            }
        }
        stage('Test Maven'){
            steps{
                //windows
                //bat "mvn test"
                
                //Ubuntu
                sh "mvn test"
            }
        }
    }
}
