pipeline {
    agent any
    stages {
        stage('compile') {
	         steps {
                // step1 
                echo 'compiling..'
		            git url: 'https://github.com/lerndevops/PetClinic'
		            sh script: '/opt/maven/bin/mvn compile'
           }
        }
        stage('codereview-pmd') {
	         steps {
                // step2
                echo 'codereview..'
		            sh script: '/opt/maven/bin/mvn -P metrics pmd:pmd'
           }
	         post {
               success {
		             recordIssues enabledForFailure: true, tool: pmdParser(pattern: '**/target/pmd.xml')
               }
           }		
        }
        stage('unit-test') {
	          steps {
                // step3
                echo 'unittest..'
	               sh script: '/opt/maven/bin/mvn test'
            }
	          post {
               success {
                   junit 'target/surefire-reports/*.xml'
               }
            }			
        }
        stage('codecoverage') {

	         steps {
                // step4
                echo 'codecoverage..'
		            sh script: '/opt/maven/bin/mvn cobertura:cobertura -Dcobertura.report.format=xml'
           }
	         post {
               success {
	               cobertura autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: 'target/site/cobertura/coverage.xml', conditionalCoverageTargets: '70, 0, 0', failUnhealthy: false, failUnstable: false, lineCoverageTargets: '80, 0, 0', maxNumberOfBuilds: 0, methodCoverageTargets: '80, 0, 0', onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false                  
               }
           }		
        }
        stage('package/build-war') {
	         steps {
                // step5
                echo 'package......'
		            sh script: '/opt/maven/bin/mvn package'	
           }		
        }
    }
}
