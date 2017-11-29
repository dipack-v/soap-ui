# Run a Test Step & use results as input to anther step

## Problem Statement: ##
Run a test step and use response to anthoer step

## Code Snippet ##
```
  def groovyUtils = new  com.eviware.soapui.support.GroovyUtils(context);
  testRunner.runTestStepByName("GetAllCountries");
  def holder = groovyUtils.getXmlHolder( testRunner.testCase.testSteps["GetAllCountries"].testRequest.response.responseContent );
  holder.namespaces["v3"] = "http://services.apmoller.net/AMM/VesselScheduleRetrieval/v3"
  for( countryCode in holder.getNodeValues( "//v3:AlternativeCodeVal" )){
	  sleep(1000);
	  prop = testRunner.testCase.testSteps['Properties'];
	  prop.setPropertyValue('prop_CountryCode', countryCode );
	
	  testRunner.runTestStepByName("GetPortsByCountryCode");
	  def holder1 = groovyUtils.getXmlHolder( testRunner.testCase.testSteps["GetPortsByCountryCode"].testRequest.response.responseContent );
	  for( port in holder1.getNodeValues( "//v3:AlternativeCodeVal" )){
		  log.info 'port ------->' + port;
	  }
  }
```
