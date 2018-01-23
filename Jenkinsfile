pipeline {
    agent any
    tools {
        maven 'localMaven'
        jdk 'localJava'
    }
    stages {
        stage ('Build') {
            steps {
            bat 'mvn install'
            }
            post {
                success {
                    junit 'target/surefire-reports/**/*.xml' 
                }
            }
     }
        stage ('Test unitaires'){
		steps{
			bat 'mvn test'
		}
		}
        stage ('Couverture du code'){
		steps{
			bat 'mvn clean cobertura:cobertura'
                           step([$class: 'CoberturaPublisher', autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: '**/target/site/cobertura/coverage.xml', failUnhealthy: false, failUnstable: false, maxNumberOfBuilds: 0, onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false])
		}
		}
         stage('Generation de la Documentation') {
                 steps{
                        bat 'mvn clean javadoc:javadoc site'
                 }
}
            
            
             stage('Analyse statique') {
                 steps{
                        bat 'mvn checkstyle:checkstyle findbugs:findbugs sonar:sonar -Dsonar.host.url=http://sic.emi.ac.ma:9000/'
                        checkstyle canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '**/target/checkstyle-result.xml', unHealthy: ''
                         findbugs canComputeNew: false, defaultEncoding: '', excludePattern: '', healthy: '', includePattern: '', pattern: ' **/target/findbugsXml.xml', unHealthy: ''

                 }
}
        stage('Generation du Jar') {
                 steps{
                        bat 'mvn package'
                 }
}
            
           
    

            
	 stage('Nexus') {
			 steps{

	nexusPublisher nexusInstanceId: 'nexus', nexusRepositoryId: 'releases', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'target/spring-petclinic-1.5.1.jar']], mavenCoordinate: [artifactId: 'spring-petclinic', groupId: 'org.springframework.samples', packaging: 'jar', version: '1.5.10']]]
			   }
		  }
           
    }

}