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
                bat 'npm install newman newman-reporter-htmlextra'
            }
        }
        
        stage ('API Test') {
        	steps {
        		script {
        			try {
                         echo 'Testing'
        				bat 'newman run "Newman.postman_collection.json" --environment "Test 001.postman_environment.json" --disable-unicode --reporters cli,junit,htmlextra --reporter-junit-export "d:\users\jluceroa\Documents\Pipeline_newman\report.xml" --reporter-htmlextra-export "d:\users\jluceroa\Documents\Pipeline_newman\report.html'
        				echo 'Ejecucion de pruebas sin errores'
        			}
        			catch (ex) {
        				echo 'Finalizo ejecucion con fallos'
        				error ('Failed')
                    }
                }
            }
        }
        
        stage ('Reporte') {
        	steps {
        		script {
                     try {
                         bat ("echo ${defTimestamp}") 
                    	publishHTML([allowMissing: true, alwaysLinkToLastBuild: true, keepAll: true, reportDir: "d:\users\jluceroa\Documents\Pipeline_newman", reportFiles: 'report.html', reportName: 'Evidencias de Prueba', reportTitles: 'Reporte de Pruebas'])                    	
                         echo 'Reporte realizado con exito'
                    }

                    catch (ex) {
                        echo 'Reporte realizado con Fallos'
                        error ('Failed')
                    }
                }
            }
        }
    }
}
