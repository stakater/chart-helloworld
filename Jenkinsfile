#!/usr/bin/groovy
@Library('github.com/stakater/fabric8-pipeline-library@pod-annotation')
String chartPackageName = ""
String chartName = "helloworld"

toolsNode(toolsImage: 'stakater/pipeline-tools:1.4.0') {
    container(name: 'tools') {
        def helm = new io.stakater.charts.Helm()
        def common = new io.stakater.Common()
        def chartManager = new io.stakater.charts.ChartManager()
        stage('Checkout') {
            checkout scm
        }
        
        stage('Init Helm') {
            sh "sleep 1h"
            helm.init(true)
        }

        stage('Prepare Chart') {
            helm.lint(WORKSPACE, chartName)
            chartPackageName = helm.package(WORKSPACE, chartName)
        }

        stage('Upload Chart') {
            String cmUsername = common.getEnvValue('CHARTMUSEUM_USERNAME')
            String cmPassword = common.getEnvValue('CHARTMUSEUM_PASSWORD')
            chartManager.uploadToChartMuseum(WORKSPACE, chartName, chartPackageName, cmUsername, cmPassword)
        }
    }
}
