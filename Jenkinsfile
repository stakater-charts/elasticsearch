#!/usr/bin/groovy
@Library('github.com/stakater/fabric8-pipeline-library@master')

String elasticSearchPackageName = ""
String elasticSearchStoragePackageName = ""
String elasticSearchChartName = "elasticsearch"
String elasticSearchStorageChartName = "elasticsearch-storage"

toolsNode(toolsImage: 'stakater/pipeline-tools:1.2.0') {
    container(name: 'tools') {
        def helm = new io.stakater.charts.Helm()
        def common = new io.stakater.Common()
        def chartManager = new io.stakater.charts.ChartManager()
        stage('Checkout') {
            checkout scm
        }
        
        stage('Init Helm') {
            helm.init(true)
        }

        stage('Prepare Chart') {
            helm.lint(WORKSPACE, elasticSearchChartName)
            elasticSearchPackageName = helm.package(WORKSPACE, elasticSearchChartName)

            helm.lint(WORKSPACE, elasticSearchStorageChartName)
            elasticSearchStoragePackageName = helm.package(WORKSPACE, elasticSearchStorageChartName)
        }

        stage('Upload Chart') {
            String cmUsername = common.getEnvValue('CHARTMUSEUM_USERNAME')
            String cmPassword = common.getEnvValue('CHARTMUSEUM_PASSWORD')
            chartManager.uploadToChartMuseum(WORKSPACE, elasticSearchChartName, elasticSearchPackageName, cmUsername, cmPassword)
            chartManager.uploadToChartMuseum(WORKSPACE, elasticSearchStorageChartName, elasticSearchStoragePackageName, cmUsername, cmPassword)
        }
    }
}
