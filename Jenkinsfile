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
		pwd
                chmod 755 *.sh
		chmod 755 *.py
		chmod 755 *.c
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
		 
      		if (env.$PARAM == 'BASH') {
			sh '''
				cd /home/slave/workspace/finel_project
				./bash.sh $PARAM
			'''
        	} else {
            		echo 'Is not bashing'
        	}
    
		
		sh '''
		    if [[ $PARAM == "ALL" ]]; then
			echo 'Execute ALL script'
			cd ${WORKSPACE}/
			python python.py $PARAM
                      	python python.py $PARAM >> /home/slave/results
			${WORKSPACE}/Cfile.c $PARAM
			./Cfile.c $PARAM >> /home/slave/results
			./bash.sh $PARAM
              		./bash.sh $PARAM >> /home/slave/results
			
		    elif [[ $PARAM == "PYTHON" ]]; then
		      	echo 'Execute python script'
		      	echo "Testing input string $PARAM" 
            	      	cd ${WORKSPACE}/
                      	python python.py $PARAM
                      	python python.py $PARAM >> /home/slave/results
		      
		    elif [[ $PARAM == "C" ]]; then
		    	echo 'Execute C script'
			echo "Testing input string $PARAM"
			cd ${WORKSPACE}/
			${WORKSPACE}/Cfile.c $PARAM
			./Cfile.c $PARAM >> /home/slave/results
			
		    elif [[ $PARAM == "BASH" ]]; then 
	            	echo 'Execute BASH script'
			echo "Testing input string $PARAM" 
              		cd /home/slave/workspace/finel_project
             		./bash.sh $PARAM
              		./bash.sh $PARAM >> /home/slave/results
		    else
		    	echo "$PARAM file not exsit"
		   fi
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
	      touch ${WORKSPACE}/scripts
	      touch ${WORKSPACE}/scripts/results
              echo "Build Number $BUILD_NUMBER" >> ${report_file}
	      cat ${WORKSPACE}/scripts/results >> ${report_file}
	      echo "#############################" >> ${report_file}
            '''
         }
      }	   
      
      
   }
}
