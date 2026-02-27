pipeline {
    agent any
    tools{
        maven "Maven_3.9.12"
    }
    stages { 
        stage('Setup') {
            steps {
                script {
                    startZap(host: "localhost", port:9091, timeout:500, zapHome: "C:\\Program Files\\ZAP\\Zed Attack Proxy", allowedHosts:['github.com']) // Start ZAP at /opt/zaproxy/zap.sh, allowing scans on github.com (if allowedHosts is not provided, any local addresses will be used
                }
            }
        }
        stage('Build & Test') {
            steps {
                script {
                    bat '"C:\\Program Files\\Apachi\\Maven\\apache-maven-3.9.12-bin\\apache-maven\\src\\bin\\mvn.cmd" verify -Dhttp.proxyHost=127.0.0.1 -Dhttp.proxyPort=9091 -Dhttps.proxyHost=localhost -Dhttps.proxyPort=9091' // Proxy tests through ZAP
                }
            }
        }
    }
    post {
        always {
            script {
                archiveZap(failAllAlerts: 1, failHighAlerts: 0, failMediumAlerts: 0, failLowAlerts: 0, falsePositivesFilePath: "zapFalsePositives.json")
            }
        }
    }
}












