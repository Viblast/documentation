#Viblast Wowza Module
Quick Start Guide



##Installation
To install the Viblast Wowza module, simply place the viblast-wm-X.X.X.jar file into the Wowza lib folder.
##Configuration
###Configure the tracker
Once the jar file is copied in the appropriate folder, the components of the module should be registered with Wowza. 

The Viblast Tracker is started by the ViblastStarter Wowza server listener. The BaseClass of ViblastStarter is com.viblast.wowza.ViblastStarter. When the listener receives the signal that Wowza is inited, it reads the configuration properties and tries to start the Viblast Tracker. If there are errors, an appropriate error message will be logged in the default Wowza log. 

The configuration parameters that are passed to the Viblast Tracker are:  
**viblastTrackerNumberOfThreads** (Type:Integer) – how many threads should be used to handle peer connections. The default and recommended value is 2.  
* **viblastTrackerExternalIP** (Type:String) – specify the external IP of the tracker in case host machine of the tracker has multiple IP addresses. 
**viblastTrackerPort** (Type:Integer) – which port should be used by the Tracker to accept peer connections. The default value is 2222.  **viblastLicenseKey** (Type:String)– the license key string of the Viblast module. If the value is empty or the license is not valid, the number of peers connected to the Tracker will be limited to 100.  **viblastAutoStart** (Type:Boolean)– determines whether the Tracker should run when the Wowza server is started or not. The default value is “true,” but it can be changed to “false.” Manually the Tracker can be started from the configuration web page.

All these properties should be placed into the conf/Server.xml file of the Wowza server.

A simple configuration should look like this:  
Server.xml:

```xml
<Root><Server>...
	<ServerListeners>...
		<ServerListener>			<BaseClass>com.viblast.wowza.ViblastStarter</BaseClass>		</ServerListener>...
	</ServerListeners>...	<Properties>...
		<Property>			<Name>viblastNumberOfThreads</Name>			<Value>4</Value>			<Type>Integer</Type>		</Property>
       <Property>
       	<Name>viblastTrackerExternalIP</Name>
          <Value>127.0.0.1</Value>
          <Type>String</Type>
       </Property>
		<Property>			<Name>viblastTrackerPort</Name>			<Value>2222</Value>			<Type>Integer</Type>		</Property>		<Property>			<Name>viblastLicenseKey</Name>			<Value>test-license</Value>			<Type>String</Type>		</Property>		<Property>			<Name>viblastAutoStart</Name>			<Value>true</Value>			<Type>Boolean</Type>		</Property>...
	</Properties></Server></Root>```
###Configure Viblast HTTProvider
The Viblast Tracker is responsible for orchestrating the peers and forming the swarm.  The resources needed to implement the PDN functionality on the client side are served by the Viblast HTTProvider. It should be defined within one of the VHost.xml files in order to be accessible from the web pages containing the player.  The BaseClass of the provider is com.viblast.wowza.ViblastHTTPAdmin. A simple configuration should look like this:  VHost.xml

```xml
<Root>	<VHost>...
	<HostPortList>...
	<HostPort>
		<Name>Default Streaming</Name>
       <Type>Streaming</Type>
...
	</HostPort>
   <HostPort>
		<Name>Default Admin</Name>
       <Type>Admin</Type>
...
	<HTTProviders>...
		<HTTPProvider>			<BaseClass>com.viblast.wowza.ViblastHTTPAdmin</BaseClass>			<RequestFilters>viblast/static*|viblast/simple.html*</RequestFilters>			<AuthenticationMethod>none</AuthenticationMethod>
		</HTTPProvider>		
		<HTTPProvider>			<BaseClass>com.viblast.wowza.ViblastHTTPAdmin</BaseClass>			<RequestFilters>viblast*</RequestFilters>			<AuthenticationMethod>admin-digest</AuthenticationMethod>		</HTTPProvider>...	</HTTProviders>	</HostPort>	</HostPortList>...	</VHost>
</Root>
```
*Note1: The definitions of the ViblastHTTPAdmin should be placed before the definition of HTTPServerVersion.*  

*Note2: The definitions of the ViblastHTTPAdmin should*  **be placed** *within the admin section.*  

After applying the configuration changes, the server should be restarted in order for them to take effect.###Check the configuration
The Viblast HTTProvider serves an example test HTML page <http://[wowza-ip]:port/viblast/test.html>, which demonstrates the Viblast/Wowza integration.   
If everything is configured properly, the page should be accessible. 