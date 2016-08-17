# RIHA iframe redirect manual

# Table of content

RIHA iframe redirect manual        1

Table of content        1

Preconditions        2

IFRAME element&#39;s src attribute        2

SRC attribute parameters        2

Examples of constructed IFRAME element        2

Querying the user session information from old RIHA system        4

# Preconditions

User clicks on one of the IFRAME backed menu options or uses kiirOtsing widget.

System loads an HTML view with Iframe element.

# IFRAME element&#39;s src attribute

SRC attribute is configurable for each of the menu options. Configuration is done through modifying  &quot;system&quot;.sys\_parameeter table. Relevant codes are:

IFRAME\_WIDGET\_ASUTUSED\_LINK

IFRAME\_WIDGET\_INFOSYSTEEMID\_LINK

IFRAME\_WIDGET\_TEENUSED\_LINK

IFRAME\_WIDGET\_KLASSIFIKAATORID\_LINK

IFRAME\_WIDGET\_ANDMEOBJEKTID\_LINK

IFRAME\_WIDGET\_VALDKONNAD\_LINK

IFRAME\_WIDGET\_XML\_VARAD\_LINK

IFRAME\_WIDGET\_YLDOTSING\_LINK

IFRAME\_WIDGET\_LIITUMINE\_LINK

**NB!** protocol, domain name and port must match RIHA instance protocol, name and port.

# SRC attribute parameters

Additional info passed as URL attributes to the iframe:

sessionId - user session id to the iframe.

otsingTekst - is set if user has used the kiirOtsing widget and contains the search term.

# Examples of constructed IFRAME element

**Example 1:**

User clicked &quot;Asutused&quot; menu option,

&quot;system&quot;.sys\_parameeter has the following row:
 

The following iframe element is generated:

&lt;iframe src=&quot; [http://riha.domain](http://riha.domain):riha\_port/iframe.html?sessionId=SESSION\_ID&quot;&gt;&lt;/iframe&gt;

**Example 2:**

User is searching for text RIHA using kiirOtsing widget,

&quot;system&quot;.sys\_parameeter has the following row:

The following iframe element is generated:

&lt;iframe src=&quot; [https://riha.domain](https://riha.domain):riha\_port/iframe.html?sessionId=SESSION\_ID&amp;otsingTekst=RIHA&quot;&gt;&lt;/iframe&gt;

# Querying the user session information from old RIHA system

User session information service is bound to:

RIHA\_URL:RIHA\_PORT/sessionManagementServlet?sessionId=SESSION\_ID

RIHA\_URL - RIHA domain name / ip address

RIHA\_PORT - tomcat port

SESSION\_ID - sessionId to check.

if sessionId is found, the following information is returned as response:

{&quot;isikuKood&quot;:&quot;PERSONAL\_CODE&quot;,

 &quot;roll&quot;:&quot;ROLE&quot;,

&quot;asutus&quot;: &quot;ORGANISATION&quot;}

PERSONAL\_CODE - user&#39;s personal code

ROLE - current user role

ORGANISATION - current user organisation

**NB!** Some or all of the JSON response fields can be null, if the information is not available to the sessionManagement servlet.
