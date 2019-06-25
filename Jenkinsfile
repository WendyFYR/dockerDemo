node("master") {
    def server = Artifactory.newServer url: "http://9.98.12.105:8081/artifactory", credentialsId: 'jfrog artifactory'
    def rtMaven = Artifactory.newMavenBuild()
    def buildInfo

    stage ('Clone') {
        git url: 'https://github.com/WendyFYR/jenkinsdemo.git'
    }

    stage ('Artifactory configuration') {
        rtMaven.tool = 'Maven' // Tool name from Jenkins configuration
        rtMaven.deployer releaseRepo: 'maven_local', snapshotRepo: 'maven_local', server: server
        buildInfo = Artifactory.newBuildInfo()
    }

    stage ('Exec Maven') {
        rtMaven.run pom: 'pom.xml', goals: 'clean install', buildInfo: buildInfo
    }

        stage ('Publish build info') {
        server.publishBuildInfo buildInfo
    }
    
        stage ('Exec Maven') {
        rtMaven.run pom: 'pom.xml', goals: 'clean install', buildInfo: buildInfo
    }
    
    
        stage ('Publish build info') {
        server.publishBuildInfo buildInfo
    }

    stage('build docker'){
			steps {
				echo "start build image"
				dir('sso-client1') {
 					bat 'docker build -t liangyun/dockerdemo:1.0 .'
				 
			}
		}
    
    
    stage ('Xray Scan') {
        def xrayconfig = [
        'buildName'   :  env.JOB_NAME,
        'buildNumber' :  env.BUILD_NUMBER
        ]
        
        def xrayResults = server.xrayScan xrayconfig
        echo xrayResults as String
        sleep 10
    }
    
    

}
