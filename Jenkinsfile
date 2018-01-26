pipeline {
	agent none

	stages {
	   stage('build') {
	   agent {
        label 'Master'
      }
	   steps {
	   sh 'ant -f build.xml -v'
	   }
	   post {
        success {
          archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
        }
      }
	   }

	   stage('deploy') {
	   steps {
	   sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/"
	   }
	   }
	   stage("Test on Debian") {
      agent {
        docker 'openjdk:8u121-jre'
      }
      steps {
        sh "wget http://10.0.30.60/rectangles/all/${env.BUILD_NUMBER}.jar"
        sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4"
      }
    }

	}

}