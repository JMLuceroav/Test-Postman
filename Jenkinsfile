pipeline {

    agent any
    
    stages {
    
        stage ('Build') {
            steps {
                bat ("newman --version")
            }
        }
        
        stage ('Run API Tests') {
        	steps {
        		script {
        			try {
        				bat (newman run "Newman.postman_collection.json" --environment "Test 001.postman_environment.json" --disable-unicode --reporters cli,junit,htmlextra --reporter-junit-export "newman/report.xml" --reporter-htmlextra-export "newman/report.html")
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
                    	publishHTML([allowMissing: true, alwaysLinkToLastBuild: true, keepAll: true, reportDir: "${WORKSPACE}/newman", reportFiles: 'report.html', reportName: 'Evidencias de Prueba', reportTitles: 'Reporte de Pruebas'])                    	
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
