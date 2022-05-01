import java.text.SimpleDateFormat

def defDateFormat = new SimpleDateFormat("yyyyMMddHHmm")
def defDate = new Date()
def defTimestamp = defDateFormat.format(defDate).toString()

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
                         echo "Send notifications for result: ${currentBuild.result}"
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
                         mail bcc: '', body: "<b>Example</b><br>\n\<br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "ERROR CI: Project name -> ${env.JOB_NAME}", to: "jluceroav@gmail.com";
               }
            }   
        } 
    }
}
