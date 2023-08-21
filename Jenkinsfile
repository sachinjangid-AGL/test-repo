properties([
    parameters([
        string(name: 'SITE_URL', defaultValue: 'https://www.marutisuzuki.com/',
                description: 'URL to test the accesibility against')
    ])
])

SITE_URL = params.SITE_URL ?: 'https://www.marutisuzuki.com/'

stage('Test URL') {
    node {
        deleteDir()
        checkout scm

        def reportDir = "${env.WORKSPACE}/reports"
        
        
        docker.build('test').inside("--privileged -v ${reportDir}:/reports") {
            sh """
                pa11y --reporter csv "${SITE_URL}" > /reports/report.csv
            """
        }
        
        // Copy the generated report.csv from the container to the Jenkins workspace
        step([$class: 'CopyArtifact', filter: 'reports/report.csv', fingerprintArtifacts: true, projectName: env.JOB_NAME, selector: [$class: 'StatusBuildSelector', stable: false]])
    }
}
