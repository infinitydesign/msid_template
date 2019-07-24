#!groovy​

node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */
        checkout scm
    }

    stage('Update Project Libraries') {
        sh 'composer install'
    }

    stage('Run Test Steps') {
        sh 'ant full-build'
    }

    stage('Publish Reports') {
        publishHTML(target: [
            allowMissing: false,
            alwaysLinkToLastBuild: false,
            keepAll: false,
            reportDir: 'tests/_output/',
            reportFiles: 'report.html',
            reportName: 'Codeception Report',
            reportTitles: 'Codeception Report'
        ])

        publishHTML(target: [
            allowMissing: false,
            alwaysLinkToLastBuild: false,
            keepAll: false,
            reportDir: 'tests/_output/coverage/',
            reportFiles: 'dashboard.html',
            reportName: 'Codeception Coverage',
            reportTitles: 'Codeception Coverage'
        ])

        plot([
            csvFileName: 'plot-64172a89-b292-479a-aee3-f3506437f0fc.csv',
            csvSeries: [[
                displayTableFlag: false,
                exclusionValues: 'Lines of Code (LOC), Comment Lines of Code (CLOC), Non-Comment Lines of Code (NCLOC), Logical Lines of Code (LLOC)',
                file: 'build/logs/phploc.csv',
                inclusionFlag: 'INCLUDE_BY_STRING',
                url: ''
            ]],
            group: 'phploc',
            numBuilds: '100',
            style: 'line',
            title: 'A - Lines of code',
            yaxis: 'Lines of Code'
        ])

        plot([
            csvFileName: 'plot-64172a89-b292-479a-aee3-f3506437f0fc.csv',
            csvSeries: [[
                displayTableFlag: false,
                exclusionValues: 'Directories, Files, Namespaces',
                file: 'build/logs/phploc.csv',
                inclusionFlag: 'INCLUDE_BY_STRING',
                url: ''
            ]],
            group: 'phploc',
            numBuilds: '100',
            style: 'line',
            title: 'B - Structures Containers',
            yaxis: 'Count'
        ])

        plot([
            csvFileName: 'plot-64172a89-b292-479a-aee3-f3506437f0fc.csv',
            csvSeries: [[
                displayTableFlag: false,
                exclusionValues: 'Average Class Length (LLOC), Average Method Length (LLOC), Average Function Length (LLOC)',
                file: 'build/logs/phploc.csv',
                inclusionFlag: 'INCLUDE_BY_STRING',
                url: ''
            ]],
            group: 'phploc',
            numBuilds: '100',
            style: 'line',
            title: 'C - Average Length',
            yaxis: 'Average Lines of Code'
        ])

        step([
            $class: 'hudson.plugins.checkstyle.CheckStylePublisher',
            checkstyle: 'build/logs/checkstyle.xml'
        ])
    }

    stage('Generate Documentation') {
        sh 'raml2html docs/api.raml > docs/_output/index.html'

        publishHTML(target: [
            allowMissing: false,
            alwaysLinkToLastBuild: false,
            keepAll: false,
            reportDir: 'docs/_output/',
            reportFiles: 'index.html',
            reportName: 'RAML Documentation',
            reportTitles: 'RAML Documentation'
        ])
    }
}
