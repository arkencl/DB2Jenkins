@Library('github.com/cloudogu/ces-build-lib@888733b')
import com.cloudogu.ces.cesbuildlib.*

node('docker'){

    Maven mvn = new MavenInDocker(this, "3.6.2-jdk-8")
    Git git = new Git(this)

    stage('Checkout'){
        git 'https://github.com/arkencl/DB2Jenkins'
        git.clean('".*/"')
    }

    stage('Build') {
        mvn 'clean install'
    }

    stage('Integration Testing'){
        def dockerContainer = docker.build("ibmcom/db2:latest", "database").run("--privileged=true -p 50001:50000")
        mvn '-Dflyway.url=jdbc:db2://localhost:50001/Jenkins flyway:migrate'
    }

}