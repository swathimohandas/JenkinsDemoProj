pipeline {
   agent any
	
    stages {
        stage('Checkout') {
            steps {
		    
               echo 'Checkout'
            }
        }
        stage('Build') {
            steps {
	      
                echo 'Clean Build'
                sh 'mvn clean compile'
                   }
        }
        stage('Test') {
            steps {
                echo 'Testing'
                sh 'mvn surefire:test'
            }
        }
        stage('JaCoCo') {
            steps {
                echo 'Code Coverage'
                jacoco(
		     execPattern:  'target/*.exec',
	             classPattern:  'target/classes',
	             sourcePattern:  'src/main/java',
                     exclusionPattern: 'src/test*'			 
		)
            }
        }
        stage('Sonar') {
            steps {
                echo 'Sonar Scanner'
               	//def scannerHome = tool 'SonarQube Scanner 3.0'
			    withSonarQubeEnv('Sonarqube') {
			    	sh '/opt/sonar-scanner/bin/sonar-scanner'
			    }
            }
        }
        stage('Package') {
            steps {
                echo 'Packaging'
                sh 'mvn package -DskipTests'
            }
        }
        stage('Deploy') {
            steps {
                echo '## TODO DEPLOYMENT ##'
		sh 'mvn clean deploy -e -X'    
            }
        }
    }
    
    post {
        always {
            echo 'JENKINS PIPELINE'
        }
        success {
            echo 'JENKINS PIPELINE SUCCESSFUL'
        }
        failure {
            echo 'JENKINS PIPELINE FAILED'
		emailext body: "$(err)",subject :'Failure', to :'swathimohandas18@gmail.com'
		}
        unstable {
            echo 'JENKINS PIPELINE WAS MARKED AS UNSTABLE'
        }
        changed {
            echo 'JENKINS PIPELINE STATUS HAS CHANGED SINCE LAST EXECUTION'
        }
    }
}

