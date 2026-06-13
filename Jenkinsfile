pipeline {
    agent any // Modificado de 'docker' a 'any' para ejecución local

    stages {
        stage('Source') {
            steps {
                git 'https://github.com/ApoliJ28/unir-cicd' // Se toma el fork del proyecto
            }
        }

        // Se contruye el proyecto a partir del dockerfile utilizando el MakeFile
        stage('Build') {
            steps {
                echo 'Building stage!'
                sh 'make build'
            }
        }

        // Pruebas unitarias
        stage('Unit tests') {
            steps {
                echo 'Ejecutando pruebas unitarias...'
                sh 'make test-unit'
                archiveArtifacts artifacts: 'results/*.xml', allowEmptyArchive: true
            }
        }
        // Pruebas de API
        stage('API tests') {
            steps {
                echo 'Ejecutando pruebas de API...'
                sh 'make test-api'
                archiveArtifacts artifacts: 'results/*.xml', allowEmptyArchive: true
            }
        }
        // Pruebas End-to-End (E2E)
        stage('E2E tests') {
            steps {
                echo 'Ejecutando pruebas E2E...'
                sh 'make test-e2e'
                archiveArtifacts artifacts: 'results/*.xml', allowEmptyArchive: true
            }
        }
    }
    
    post {
        always {
            // Presentación de todos los informes (Unitarias, Api, e2e) junto con la limpieza del workspace
            junit testResults: 'results/*.xml', allowEmptyResults: true
            cleanWs() 
        }
        failure {
            // El echo sirve para validar las variables globales sin enviar el correo real
            echo "==========================================================="
            echo "Simulación del correo solo si falla algun stages."
            echo "Destinatario: equipo-devops@fintechsolutions.com"
            echo "Asunto: Fallo en el pipeline: ${env.JOB_NAME}"
            echo "Cuerpo: El trabajo '${env.JOB_NAME}' con el número de ejecución #${env.BUILD_NUMBER} ha fallado. Por favor, revisa los logs."
            echo "==========================================================="
            
            // Ejemplo Real...
            /*
            mail to: 'equipo-devops@fintechsolutions.com',
                 subject: "Fallo en el pipeline: ${env.JOB_NAME}",
                 body: "El trabajo '${env.JOB_NAME}' con el número de ejecución #${env.BUILD_NUMBER} ha fallado. Por favor, revisa los logs."
            */
        }
    }
}