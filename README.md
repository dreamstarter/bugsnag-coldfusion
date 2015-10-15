bugsnag-coldfusion
======

A Coldfusion notifier for [BugSnag](https://bugsnag.com)

This component will push instant error notifications in your ColdFusion application to the BugSnag platform.

Requirements
=========

+ ColdFusion 8+ / Railo 3+
+ BugSnag account
+ BugSnag API key

Installation / usage
=========

### Configuration

+ Place BugSnag.cfc somehwere into your project.
+ Call the init() function to initiate the BugSnag component:

```cfm
<cfset bugSnag = createObject("component","BugSnag").init(
	APIKey = "xxxxxxxxxxxxxxxx" <!--- Your API key here --->
)/> 
```

+ Configure the 'onError' method in your Application.cfc so that it passes the exception to BugSnag.cfc:

```cfm
<cffunction name="onError" returntype="void" output="false">

	<cfargument name="exception" type="any" required="true"/>
    
	<cfset var bugSnag = createObject("component","BugSnag").init(
		APIKey = "xxxxxxxxxxxxxxxx" <!--- Your API key here --->
	)/>

	<!--- Notify BugSnag --->
	<cfset this.bugSnag.notifyError(exception = arguments.exception)/>

</cffunction>
```

### Additional options

The init() function of BugSnag.cfc accepts several additional parameters: 
+ releaseStage: The current release stage for the application (development, test, production etc). Default: development
+ notifyReleaseStages: A list of release stages that the notifier will capture and send errors for. If the current release stage is not in this list the error will not be sent to BugSnag
+ autoNotify: If this is false, the plugin doesn't notify Bugsnag of any uncaught exceptions (if possible). Default: true
+ useSSL: If this is true, the plugin will notify Bugsnag using SSL. Default: true
```cfm
<cfset this.bugSnag = createObject("component","BugSnag").init(
	<cfset this.bugSnag = createObject("component","BugSnag").init(
		APIKey = "xxxxxxxxxxxxx", <!--- Your API key here --->
		releaseStage = "development", <!--- Optional --->
		notifyReleaseStages = "development", <!--- Optional --->
		autoNotify = true, <!--- Optional --->
		useSSL = true <!--- Optional --->
	)/> 
)/> 
```

The notifyError() function of BugSnag.cfc accepts several parameters:

+ exception: The exception object generated by ColdFusion (Required)
+ context: The context that is currently active in the application (Optional. Default: "#CGI.REQUEST_METHOD# #CGI.PATH_INFO#")
+ severity: The severity of the error (error, warning, info). (Optional. Default: error)
+ app: An object representing the current application (Optional. Default: structNew())
+ user: An object representing the current application's user (Optional. Default: structNew())
+ metaData: An object that contains other objects which will be displayed as tabs in Bugsnag (Optional. Default: structNew())

```cfm
<cffunction name="onError" returntype="void" output="false">

	<cfargument name="exception" type="any" required="true"/>
    
    <cfset var app = structNew()/>
	<cfset var user = structNew()/>
	<cfset var metadata = structNew()/>

	<!--- Create app data --->
	<cfset app["appData1"] = "One"/>
	<cfset app["appData2"] = "Two"/>

	<!--- Create user data --->
   	<cfset user["id"] = 1/>
	<cfset user["username"] = "John Doe"/>

	<!--- Create meta data 1 --->		
	<cfset metadata["something"] = structNew()/>
	<cfset metadata["something"]["test"] = 123/>
	<cfset metadata["something"]["hi"] = "Hi!"/>

	<!--- Create meta data 2 --->	
	<cfset metadata["else"] = structNew()/>
	<cfset metadata["else"]["test"] = 123/>

	<!--- Notify BugSnag --->
	<cfset this.bugSnag.notifyError(
		exception = arguments.exception, 
		context = "/some/other/context", <!--- Optional --->
		severity = "warning", <!--- Optional --->
		app = app,  <!--- Optional --->
		user = user,  <!--- Optional --->
		metaData = metaData <!--- Optional --->
	)/>

</cffunction>
```

Support
=======
If you have any questions, feel free to contact me.

Rick Groenewegen

rick@aanzee.nl








