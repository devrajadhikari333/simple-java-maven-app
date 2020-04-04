node('Maven'){
    stage('checkout_scm'){
        try {
            checkout scm
        } catch(err) {
            sh "echo error in checkout"
        }
    }
    stage('maven test'){
        try {
            mvnHome=tool 'maven-3.6.3'
            sh "$mvnHome/bin/mvn --version"
            sh "$mvnHome/bin/mvn clean test surefire-report:report"
        } catch(err) {
            sh "echo error in defining maven"
        }
    }
    stage('test case and report'){
        try {
            echo "executing test cases"
            junit allowEmptyResults: true, testResults: 'target/surefire-reports'
            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'target/site', reportFiles: 'surefire-report.html', reportName: 'SureFireReportHTML', reportTitles: ''])
        } catch(err){
            throw err
        }
    }
}