# Out-of-process ATL COM server (ATLExeCOMServer)
## Requires
- Visual Studio 2008
## License
- Apache License, Version 2.0
## Technologies
- COM
- ATL
## Topics
- out-of-process COM Server
## Updated
- 05/05/2011
## Description

<p style="font-family:Courier New"></p>
<h2>ACTIVE TEMPLATE LIBRARY : ATLExeCOMServer Project Overview</h2>
<p style="font-family:Courier New"></p>
<h3>Summary:</h3>
<p style="font-family:Courier New"><br>
The ATLDllCOMServer sample demonstrates how to use Acitve Template Library <br>
(ATL) wizards in Visual Studio 2008 to generate an out-of-process COM server. <br>
ATL is designed to simplify the process of creating efficient, flexible, <br>
lightweight COM components. ATLExeCOMServer exposes an ATL STA simple object <br>
with properties, methods, and events:<br>
<br>
&nbsp;(Please generate new GUIDs when you are writing your own COM server)<br>
&nbsp;Program ID: ATLExeCOMServer.SimpleObject<br>
&nbsp;CLSID_SimpleObject: 9465BE08-C9FA-4DDF-A56E-57B92BCDEEB0<br>
&nbsp;IID_ISimpleObject: 47E764FE-D065-4BDE-8943-30F46664B02C<br>
&nbsp;DIID__ISimpleObjectEvents: 6EE998B7-61C8-4D54-B27A-F623E8C1EA64<br>
&nbsp;LIBID_ATLExeCOMServerLib: C7902493-E23D-4487-824F-4AB97BD8B86D<br>
&nbsp;AppID: B711EE75-FDA3-4B0E-BFAA-67CB305D62AE<br>
<br>
&nbsp;Properties: <br>
&nbsp; &nbsp;// With both get and put accessor methods<br>
&nbsp; &nbsp;FLOAT FloatProperty<br>
<br>
&nbsp;Methods: <br>
&nbsp; &nbsp;// HelloWorld returns a BSTR &quot;HelloWorld&quot;<br>
&nbsp; &nbsp;HRESULT HelloWorld([out,retval] BSTR* pRet);<br>
&nbsp; &nbsp;// GetProcessThreadID outputs the running process ID and thread ID<br>
&nbsp; &nbsp;HRESULT GetProcessThreadID([out] LONG* pdwProcessId, [out] LONG* pdwThreadId);<br>
<br>
&nbsp;Events:<br>
&nbsp; &nbsp;// FloatPropertyChanging is fired before new value is set to the <br>
&nbsp; &nbsp;// FloatProperty property. The [in, out] parameter Cancel allows the
<br>
&nbsp; &nbsp;// client to cancel the change of FloatProperty.<br>
&nbsp; &nbsp;void FloatPropertyChanging(<br>
&nbsp; &nbsp; &nbsp;[in] FLOAT NewValue, [in, out] VARIANT_BOOL* Cancel);<br>
<br>
The majority of codes in the sample are generated by Visual Studio. You can <br>
find the detailed steps of creating such a COM server in the Creation section <br>
of this document. <br>
<br>
</p>
<h3>Project Relation:</h3>
<p style="font-family:Courier New"><br>
MFCCOMClient -&gt; ATLExeCOMServer<br>
MFCCOMClient demonstrates the use of the out-of-process component.<br>
<br>
ATLExeCOMServer - ATLDllCOMServer - ATLCOMService<br>
All are COM components written in ATL. ATLDllCOMServer is an in-process<br>
component in the form of DLL, ATLExeCOMServer is an out-of-process component<br>
in the form of EXE (Local Server), and ATLCOMService is an out-of-process <br>
component in the form of EXE (Local Service).<br>
<br>
</p>
<h3>Build:</h3>
<p style="font-family:Courier New"><br>
To build ATLExeCOMServer, please run Visual Studio as administrator because the <br>
component needs to be registered into HKCR.<br>
<br>
</p>
<h3>Deployment:</h3>
<p style="font-family:Courier New"><br>
A. Setup<br>
<br>
ATLExeCOMServer.exe /Regserver<br>
It registers the server as just a plain EXE. <br>
<br>
B. Cleanup<br>
<br>
ATLExeCOMServer.exe /Unregserver<br>
It removes the server from the system registry.<br>
<br>
</p>
<h3>Creation:</h3>
<p style="font-family:Courier New"><br>
A. Creating the project<br>
<br>
Step1. Create a Visual C&#43;&#43; / ATL / ATL Project named ATLExeCOMServer in Visual<br>
Studio 2008.<br>
<br>
Step2. In the page &quot;Application Settings&quot; of ATL Project Wizard, select the
<br>
server type as Executable (EXE), and allow merging of proxy/stub code.<br>
<br>
B. Adding an ATL Simple Object<br>
<br>
Step1. In Solution Explorer, add a new ATL / ATL Simple Object class to the <br>
project.<br>
<br>
Step2. In ATL Simple Object Wizard, specify the short name as <br>
SimpleObject, and select the threading model as Apartment (corresponding<br>
to STA), select Interface as Dual that supports both IDispatch (late binding)<br>
and vtable binding (early binding). Last, select the Connection points check <br>
box. This creates the _ISimpleObjectEvents interface in the file <br>
ATLExeCOMServer.idl. The Connection points option is the prerequisite for the <br>
component to supporting events.<br>
<br>
C. Adding Properties to the ATL Simple Object<br>
<br>
Step1. In Class View, find the interface ISimpleObject. Right click it <br>
and select Add / Add Property in the menu. <br>
<br>
Step2. In Add Property Wizard, specify the property type as FLOAT, property <br>
name as FloatProperty. Select both Get function and Put function. <br>
SimpleObject therefore exposes FloatProperty with the get&put accessor <br>
methods: get_FloatProperty, put_FloatProperty.<br>
<br>
Step3. Add a float field, m_fField, to the class CSimpleObject:<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp;protected:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;// Used by FloatProperty<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;float m_fField;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br>
Implement the get&put accessor methods of FloatProperty to access m_fField.<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp;STDMETHODIMP CSimpleObject::get_FloatProperty(FLOAT* pVal)<br>
&nbsp;&nbsp;&nbsp;&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*pVal = m_fField;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return S_OK;<br>
&nbsp;&nbsp;&nbsp;&nbsp;}<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp;STDMETHODIMP CSimpleObject::put_FloatProperty(FLOAT newVal)<br>
&nbsp;&nbsp;&nbsp;&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;m_fField = newVal;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return S_OK;<br>
&nbsp;&nbsp;&nbsp;&nbsp;}<br>
<br>
D. Adding Methods to the ATL Simple Object<br>
<br>
Step1. In Class View, find the interface ISimpleObject. Right-click it<br>
and select Add / Add Method in the menu.<br>
<br>
Step2. In Add Method Wizard, specify the method name as HelloWorld. Add a <br>
parameter whose parameter attributes = retval, parameter type = BSTR*, <br>
and parameter name = pRet.<br>
<br>
Step3. Write the body of HelloWorld as this:<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp;STDMETHODIMP CSimpleObject::HelloWorld(BSTR* pRet)<br>
&nbsp;&nbsp;&nbsp;&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;// Allocate memory for the string:
<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*pRet = ::SysAllocString(L&quot;HelloWorld&quot;);<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if (pRet == NULL)<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return E_OUTOFMEMORY;<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;// The client is now responsible for freeing pbstr<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return S_OK;<br>
&nbsp;&nbsp;&nbsp;&nbsp;}<br>
<br>
With the almost same steps, the method GetProcessThreadID is added to get the<br>
executing process ID and thread ID.<br>
<br>
HRESULT GetProcessThreadID([out] LONG* pdwProcessId, [out] LONG* pdwThreadId);<br>
<br>
E. Adding Events to the ATL Simple Object<br>
<br>
The Connection points option in B/Step2 is the prerequisite for the component<br>
to supporting events.<br>
<br>
Step1. In Class View, expand ATLExeCOMServer and ATLExeCOMServerLib to display<br>
_ISimpleObjectEvents.<br>
<br>
Step2. Right-click _ISimpleObjectEvents. In the menu, click Add, and <br>
then click Add Method.<br>
<br>
Step3. Select a Return Type of void, enter FloatPropertyChanging in the<br>
Method name box, and add an [in] parameter FLOAT NewValue, and an [in, out] <br>
parameter VARIANT_BOOL* Cancel. After clicking Finish, <br>
_ISimpleObjectEvents dispinterface in the ATLExeCOMServer.idl file <br>
should look like this: <br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp;dispinterface _ISimpleObjectEvents<br>
&nbsp;&nbsp;&nbsp;&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;properties:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;methods:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[id(1), helpstring(&quot;method FloatPropertyChanging&quot;)] void
<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;FloatPropertyChanging(<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[in] FLOAT NewValue, [in,out] VARIANT_BOOL* Cancel);<br>
&nbsp;&nbsp;&nbsp;&nbsp;};<br>
<br>
Step4. Generate the type library by rebuilding the project or by <br>
right-clicking the ATLExeCOMServer.idl file in Solution Explorer and clicking<br>
Compile on the shortcut menu. Please note: We must compile the IDL file <br>
BEFORE setting up a connection point.<br>
<br>
Step5. Use the Implement Connection Point Wizard to implement the Connection<br>
Point interface: In Class View, right-click the component's implementation <br>
class CSimpleObject. On the shortcut menu, click Add, and then click <br>
Add Connection Point. Select _ISimpleObjectEvents from the Source <br>
Interfaces list and double-click it to add it to the Implement connection <br>
points column. Click Finish. A proxy class for the connection point will be <br>
generated (ie. CProxy_ISimpleObjectEvents in this sample) in the file <br>
_ISimpleObjectEvents_CP.h. This also creates a function with the name<br>
Fire_[eventname] which can be called to raise events in the client. <br>
<br>
Step6. Fire the event in put_FloatProperty:<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp;STDMETHODIMP CSimpleObject::put_FloatProperty(FLOAT newVal)<br>
&nbsp;&nbsp;&nbsp;&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;// Fire the event, FloatPropertyChanging<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;VARIANT_BOOL cancel = VARIANT_FALSE;
<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fire_FloatPropertyChanging(newVal, &cancel);<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if (cancel == VARIANT_FALSE)<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;m_fField = newVal;&nbsp;&nbsp;&nbsp;&nbsp;// Save the new value<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;} // else, do nothing<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return S_OK;<br>
&nbsp;&nbsp;&nbsp;&nbsp;}<br>
<br>
F. Configuring and building the project as an ATL COM server<br>
<br>
Step1. Right-click the ATLExeCOMServer project and select Properties to open <br>
its Property Pages. Turn to Build Events / Post Build Event, and make sure <br>
that &quot;$(TargetPath)&quot; /RegServer is in the Command Line.<br>
<br>
</p>
<h3>References:</h3>
<p style="font-family:Courier New"><br>
ATL Tutorial<br>
<a target="_blank" href="http://msdn.microsoft.com/en-us/library/599w5e7x.aspx">http://msdn.microsoft.com/en-us/library/599w5e7x.aspx</a><br>
<br>
<br>
</p>
<hr>
<div><a href="http://go.microsoft.com/?linkid=9759640" style="margin-top:3px"><img src="http://bit.ly/onecodelogo">
</a></div>
