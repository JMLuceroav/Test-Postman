currentBuild.displayName="API-Automation-#"+currentBuild.number

def correo = 'jluceroav@gmail.com'

    pipeline{
              agent any
            
            stages{
                stage('Build'){
                    steps{
                		 bat 'newman --version'
                		 bat 'npm --version'
                		 bat 'node --version'
                    }
                }

                stage('Postman API Test'){
                    steps{
                        script{
                            try{
                            	bat 'newman run "Newman.postman_collection.json" \
                              		--environment "Test 001.postman_environment.json" \
                              		--disable-unicode \
                              		--reporters cli,junit,htmlextra \
                              		--reporter-junit-export "newman/index.xml" \
                              		--reporter-htmlextra-export "newman/index.html"' 

                                 echo 'Ejecucion de pruebas sin errores...'

                            }catch(Exception ex){
                                echo 'Finalizo ejecucion con fallos...'
                            }
                        }
                    }
                }
                stage("Generating Report"){
                   steps{
                    script {
                     try {
                    	bat ("echo ${WORKSPACE}")  
                        publishHTML([
                              allowMissing: true,
                              alwaysLinkToLastBuild: true, 
                              keepAll: true, reportDir: "${WORKSPACE}/newman",
                              reportFiles: 'index.html',
                              reportName: 'Evidencias de Prueba',
                              reportTitles: 'Reporte de Pruebas'
                         ])
                        junit allowEmptyResults: true, testResults: "${WORKSPACE}/newman/index.xml"
                        echo 'Reporte realizado con exito'
                       }  
                      catch (ex) {
                        echo 'Reporte realizado con Fallos'
                        error ('Failed')
                    }
                }   
                         
                    }
                    
               post {
                    
                failure {
                         echo 'I failed :('
                         echo "Send notifications for result: ${currentBuild.result}"                       
                         mail to: "${correo}",
                         subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
                         body: "${currentBuild.currentResult}: Job ${env.JOB_NAME}\nMore Info can be found here: ${env.BUILD_URL}"
               }
                 
               success {
                         echo 'I succeeded!'
                         echo "Send notifications for result: ${currentBuild.result}"                  
                         mail to: "${correo}",
                         subject: "Success Pipeline: ${currentBuild.fullDisplayName}",
                         body: "${currentBuild.currentResult}: Job ${env.JOB_NAME}\nMore Info can be found here: ${env.BUILD_URL}"
               }
            }       
                }                           
         }
   }
