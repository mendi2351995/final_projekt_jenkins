pipeline {
  agent { node { label 'slave01' } }

   stages {
      stage('Clone Sources') {
        steps {
          checkout scm
        } 
      }
      stage('Build') {
         steps {
            echo 'Build process..'            
            sh '''
                cd ${WORKSPACE}/
                chmod 755 *.sh
            '''
         }
      }      
        stage("Env Variables") {
            steps {
                sh "printenv"
            }
        }    
      stage('Test') {
         steps {
            echo 'Test process..'
		sh '''
		    if [[ $PARAM == "ALL" ]]; then
			echo 'Execute ALL script'
		elif [[ $PARAM == "PYTHON" ]]; then
		      echo 'Execute python script'
		      chmod 755 ${WORKSPACE}/python.py
		      ${WORKSPACE}/python.py $PARAM
		elif [[ $PARAM == "C" ]]; then
		 echo 'Execute C script'
		 chmod 755 ${WORKSPACE}/Cfile.c
		 ${WORKSPACE}/Cfile.c $PARAM
		elif [[ $PARAM == "BASH" ]]; then 
		  echo 'Execute BASH script'
		  chmod 755 ${WORKSPACE}/bash.sh
		  ${WORKSPACE}/bash.sh $PARAM
		else
		 echo "$PARAM file not exsit"
		fi
		'''  
            sh '''
              echo "Testing input string $PARAM" 
              cd ${WORKSPACE}
              ./reverse.sh $PARAM
              ./reverse.sh $PARAM > results
            '''
         }
      }
      stage('Saving Results') {
         steps {
            echo 'Saving Results process..'
            sh '''
	      report_file="${HOME}/Documents/Deployment/report"
              mkdir -p ${HOME}/Documents/Deployment/              
              if [ -f "${report_file}" ]; then
                echo "file ${report_file} exists"
              else
	              touch ${report_file}
              fi              
              echo "Build Number $BUILD_NUMBER" >> ${report_file}
              cat ${WORKSPACE}/scripts/results >> ${report_file}
	      echo "#############################" >> ${report_file}
            '''
         }
      }
      
   }
}
