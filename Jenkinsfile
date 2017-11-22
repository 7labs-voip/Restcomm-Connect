node("cxs-ups-testsuites_large") {
   // Mark the code checkout 'stage'....
   stage 'Checkout'

   // Checkout code from repository
   checkout scm

   // Mark the code build 'stage'....
   stage 'Build'
   // Run the maven build
   sh "mvn -f restcomm/pom.xml  clean install -pl '!restcomm.testsuite' -Dmaven.test.failure.ignore=true -Dmaven.test.redirectTestOutputToFile=true"
   junit '**/target/surefire-reports/*.xml'
   sh "mvn -f restcomm/pom.xml  clean"

   stage 'CITestsuite'
   sh "mvn -f restcomm/restcomm.testsuite/pom.xml  clean install -Pparallel-testing -DforkCount=16 -DskipUTs=false  -Dmaven.test.failure.ignore=true -Dmaven.test.redirectTestOutputToFile=true -Dfailsafe.rerunFailingTestsCount=1 -DexcludedGroups='org.restcomm.connect.commons.annotations.UnstableTests or org.restcomm.connect.commons.annotations.BrokenTests'"
   junit testResults: '**/target/surefire-reports/*.xml', testDataPublishers: [[$class: 'StabilityTestDataPublisher']]
   junit testResults: '**/target/failsafe-reports/*.xml', testDataPublishers: [[$class: 'StabilityTestDataPublisher']]
}
