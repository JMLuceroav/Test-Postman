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
                         mail to: jluceroav@gmail.com, subject: 'The Pipeline failed :('
               }
            }   
        } 
    }
}
