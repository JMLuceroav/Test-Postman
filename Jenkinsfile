import java.text.SimpleDateFormat

def defDateFormat = new SimpleDateFormat("yyyyMMddHHmm")
def defDate = new Date()
def defTimestamp = defDateFormat.format(defDate).toString()
def pathXml = "newman/index.xml"
def pathHtml = "newman/index.html"

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
                         bat 'newman run "Newman.postman_collection.json" --environment "Test 001.postman_environment.json" --disable-unicode --reporters cli,junit,htmlextra --reporter-junit-export ${pathXml} --reporter-htmlextra-export ${pathHtml}' 			
                }
            }
             
            post {
                always {
                         echo "${defTimestamp}"
                         echo "Generando reporte"
                    	publishHTML([
                              allowMissing: true,
                              alwaysLinkToLastBuild: true, 
                              keepAll: true, reportDir: "${WORKSPACE}/newman",
                              reportFiles: 'index.html',
                              reportName: 'Evidencias de Prueba',
                              reportTitles: 'Reporte de Pruebas'
                         ])   
                }
            }   
        } 
    }
}
