currentBuild.displayName="Prod-API-AUtomation-#"+currentBuild.number
    pipeline{
              agent any
            
            stages{
                stage('Build'){
                    steps{
                         echo 'Building'
                		 bat 'newman --version'
                		 bat 'npm --version'
                		 bat 'node --version'
                    }
                }

                stage('Postman Test'){
                    steps{
                        script{
                            try{

                            	echo 'Ejecucion de pruebas'
                            	bat 'newman run "Newman.postman_collection.json" \
                              		--environment "Test 001.postman_environment.json" \
                              		--disable-unicode \
                              		--reporters cli,junit,htmlextra \
                              		--reporter-junit-export "$WORKSPACE/newman/index.xml" \
                              		--reporter-htmlextra-export "$WORKSPACE/newman/index.html"' 


                                currentBuild.result="SUCCESS"

                            }catch(Exception ex){
                                currentBuild.result="FAILURE"
                            }
                        }
                    }
                }
                stage("Generating Report"){
                    steps{
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
