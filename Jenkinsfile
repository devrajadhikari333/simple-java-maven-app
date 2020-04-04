node('Maven'){
    stage('checkout_scm'){
        try {
            checkout scm
        } catch(err) {
            sh "echo error in checkout"
        }
    }
    stage('maven test'){
        try{
            mvnHome=tool 'maven-3.6.6'
            sh "$mvnHome/bin/mvn --version"
            sh "$mvnHome/bin/mvn clean test surefire-report:report"
        }
    }
}