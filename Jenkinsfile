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
		chmod 755 *.py
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
		      echo "Testing input string $PARAM" 
            	      cd ${WORKSPACE}
                      python python.py $PARAM
                      python python.py $PARAM >> /home/slave/results
		      
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
         }
      }
      
      
   }
}
