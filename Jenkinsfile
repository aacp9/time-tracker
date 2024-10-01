pipeline{
    agent any
    
    tools{
        jdk "Java11"
    }
    
    environment{
        FLAG_GREEN = 'true'
        FLAG_RED    = 'false'
    }
    
    stages{
        stage('Hello'){
            steps {
                echo "Valor bandera VERDE: ${FLAG_GREEN}"
                echo "Valor bandera ROJA: ${FLAG_RED}"
                echo "Ejemplos de variables de entorno"
                echo "Valor Job Name: ${env.JOB_NAME}"
                echo "Valor JAVA_HOME: ${env.JAVA_HOME}"
//                echo "Valor MAVEN_HOME: ${env.MAVEN_HOME}"
            }
        }
        stage ('decargar_codigo_Git_Polling'){
            steps{
                //Acceso público
                //git branch: 'main', url;"https://github.com/aacp9/time-tracker.git"
                
                //acceso con tunel ssh
                git branch: 'main', credentialsId: 'GitHub_aacp9User',
                url: 'git@github.com:aacp9/time-tracker.git'
            }
        }
        stage ('Build con Maven'){
            steps{
                //para Windows
                //bat "mvn clean package"
                //ignorar test
                //bat "mvn clean package -DskipTests"
                
                //para Ubuntu e ignorar test
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        post{
            //los artefactos son war. ear,jar, archivo comprimidos que se utilizan para el despliege de la aplicacion.
            //Condicionales de post
            //always, chaped, fixed, regression, aborted, failure, success
            success{
                echo 'Archivar Artefactos'
                archiveArtifacts "core/target/*.jar"
                archiveArtifacts "web/target/*.war"
            }
            
        }    
        }
        stage ('Test Maven'){
            steps{
                //Windows
                //bat "mvn test"
                
                //Ubuntu
                sh "mvn test"
            }
        }
    }
}
