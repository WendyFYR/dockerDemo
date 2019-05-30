node("master") {
    def server = Artifactory.newServer url: "http://9.98.12.105:8081/artifactory", credentialsId: 'jfrog artifactory'
    def rtMaven = Artifactory.newMavenBuild()
    def buildInfo

    stage ('Clone') {
        git url: 'https://github.com/WendyFYR/jenkinsdemo.git'
    }

    stage ('Artifactory configuration') {
        rtMaven.tool = '64maven' // Tool name from Jenkins configuration
        rtMaven.deployer releaseRepo: 'maven_local', snapshotRepo: 'maven_local', server: server
        buildInfo = Artifactory.newBuildInfo()
    }

    stage ('Exec Maven') {
        rtMaven.run pom: 'pom.xml', goals: 'clean install', buildInfo: buildInfo
    }

    stage ('Publish build info') {
        server.publishBuildInfo buildInfo
    }
}
