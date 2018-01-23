pipeline {
    agent any
    tools {
        maven 'Maven 3.5.2'
        jdk 'jdk1.8.0_151'
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
        stage ('JUnit tests'){
		steps{
			bat 'mvn test'
		}
		}
        stage ('Code Coverage'){
		steps{
			bat 'mvn clean cobertura:cobertura'
                           step([$class: 'CoberturaPublisher', autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: '**/coverage.xml', failUnhealthy: false, failUnstable: false, maxNumberOfBuilds: 0, onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false])
		}
		}
         stage('Generate Documentation and site') {
                 steps{
                        bat 'mvn clean javadoc:javadoc site'
                 }
}
            
            
             stage('Analyse statique') {
                 steps{
                        bat 'mvn checkstyle:checkstyle findbugs:findbugs sonar:sonar -Dsonar.host.url=http://sic.emi.ac.ma:9000/'
                        checkstyle canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '**/checkstyle-result.xml', unHealthy: ''
                         findbugs canComputeNew: false, defaultEncoding: '', excludePattern: '', healthy: '', includePattern: '', pattern: '**/findbugsXml.xml', unHealthy: ''

                 }
}
        stage('Generate Jar') {
                 steps{
                        bat 'mvn package'
                 }
}
            
           
    

            
	 stage('Publish') {
			 steps{

	nexusPublisher nexusInstanceId: 'nexus', nexusRepositoryId: 'releases', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'target/spring-petclinic-1.5.1.jar']], mavenCoordinate: [artifactId: 'spring-petclinic', groupId: 'org.springframework.samples', packaging: 'jar', version: '1.5.10']]]
			   }
		  }
           
    }

}
