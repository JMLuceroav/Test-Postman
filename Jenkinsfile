import java.text.SimpleDateFormat

def defDateFormat = new SimpleDateFormat("yyyyMMddHHmm")
def defDate = new Date()
def defTimestamp = defDateFormat.format(defDate).toString()
def correo = 'jluceroav@gmail.com'

pipeline {
     
    agent any
    
    stages {
        stage ('Build') {
            steps {
                echo 'Building'
                bat 'newman --version'
                bat 'npm --version'
                bat 'node --version'
            }
        }
        
        stage ('Postman Test') {
        	steps {
        		script {    
                         echo 'Ejecucion de pruebas'
                         bat 'newman run "Newman.postman_collection.json" \
                              --environment "Test 001.postman_environment.json" \
                              --disable-unicode \
                              --reporters cli,junit,htmlextra \
                              --reporter-junit-export "newman/index.xml" \
                              --reporter-htmlextra-export "newman/index.html"' 			
                }
            }
             
            post {
                always {                  
                         echo "Generando reporte"
                         echo "${defTimestamp}"
                    	publishHTML([
                              allowMissing: true,
                              alwaysLinkToLastBuild: true, 
                              keepAll: true, reportDir: "${WORKSPACE}/newman",
                              reportFiles: 'index.html',
                              reportName: 'Evidencias de Prueba',
                              reportTitles: 'Reporte de Pruebas'
                         ])   
                }
                 
                failure {
                         echo 'I failed :('
                         echo "Project: ${env.JOB_NAME}" 
                         echo "Build Number: ${env.BUILD_NUMBER}"
                         echo "URL de build: ${env.BUILD_URL}"
                         echo "Project name: ${env.JOB_NAME}"
                         echo "Send notifications for result: ${currentBuild.result}"
                         echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
                         echo "Descripcion del proyecto: ${env.JOB_DESCRIPTION}"
                         echo "Cambios de compilacion: ${env.CHANGES}"
                     
                         mail to: "${correo}",
                         subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
                         body: "${currentBuild.currentResult}: Job ${env.JOB_NAME}\nMore Info can be found here: ${env.BUILD_URL}"
               }
                 
               success {
                         echo 'I succeeded!'
                         echo "Send notifications for result: ${currentBuild.result}"
               }
            }   
        } 
    }
}
