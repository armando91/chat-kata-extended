chat-kata
=========

A Code Kata for building a REST client-server chat app for Android.

Server-side Kata
-----------------

Follow the README in [server directory](/server)

Client-side Kata
-----------------

Follow the README in [client directory](/client/SimpleChat)

API Contract
------------

The following are the definition of the API resources.

### Users Resource ###

**Endpoint:** ```/api/users```

#### Get all online users ####

**Method:** GET

**Response:**

* Response when there are new messages:

    ```json
    [
        "juan",
        "jose",
        "pedro"
    ]
    ````

### User Resource ###

**Endpoint:** ```/api/users/{nick}```

#### Register as a online user ####

**Method:** PUT

**Errors:** 

<table class="confluenceTable"><tbody><tr><th class="confluenceTh">Status Code</th><th class="confluenceTh">Body</th><th class="confluenceTh">Description</th></tr><tr><td colspan="1" class="confluenceTd">403</td><td colspan="1" class="confluenceTd"><table class="wysiwyg-macro" data-macro-name="code" style="background-image: url(/confluence/plugins/servlet/confluence/placeholder/macro-heading?definition=e2NvZGV9&amp;locale=en_GB&amp;version=2); background-repeat: no-repeat;" data-macro-body-type="PLAIN_TEXT"><tr><td class="wysiwyg-macro-body"><pre>{"error":"public can not be used as user nick"}</pre></td></tr></table></td><td colspan="1" class="confluenceTd">Use public as user nick is forbidden</td></tr></tbody></table>


#### Unregister as a online user ####

**Method:** DELETE

**Errors:** 

<table class="confluenceTable"><tbody><tr><th class="confluenceTh">Status Code</th><th class="confluenceTh">Body</th><th class="confluenceTh">Description</th></tr><tr><td colspan="1" class="confluenceTd">404</td><td colspan="1" class="confluenceTd"><table class="wysiwyg-macro" data-macro-name="code" style="background-image: url(/confluence/plugins/servlet/confluence/placeholder/macro-heading?definition=e2NvZGV9&amp;locale=en_GB&amp;version=2); background-repeat: no-repeat;" data-macro-body-type="PLAIN_TEXT"><tr><td class="wysiwyg-macro-body"><pre>{"error":"user X is not found"}</pre></td></tr></table></td><td colspan="1" class="confluenceTd">When trying to delete a user which does not exists</td></tr></tbody></table>


### Public Chat Resource ###

This the resource of the public chat.

**Endpoint:** ```/api/chat/public```

#### Get public chat messages ####

**Method:** GET

**Headers**
<table class="confluenceTable"><tbody><tr><th class="confluenceTh">Name</th><th class="confluenceTh">Description</th><th colspan="1" class="confluenceTh">Cardinality</th><th class="confluenceTh">Example</th></tr><tr><td colspan="1" class="confluenceTd">X-User-Nick</td><td colspan="1" class="confluenceTd">The nick of the user making the request</td><td colspan="1" class="confluenceTd">1</td><td colspan="1" class="confluenceTd">X-User-Nick: juan</td></tr></tbody></table>


**Parameters:**

<table class="confluenceTable"><tbody><tr><th class="confluenceTh">Name</th><th colspan="1" class="confluenceTh">Type</th><th class="confluenceTh">Description</th><th colspan="1" class="confluenceTh">Mandatory</th><th colspan="1" class="confluenceTh">Cardinality</th><th class="confluenceTh">Example</th></tr><tr><td colspan="1" class="confluenceTd">seq</td><td colspan="1" class="confluenceTd">Integer</td><td colspan="1" class="confluenceTd">Sequence from next message. No present if this is the first call</td><td colspan="1" class="confluenceTd">No</td><td colspan="1" class="confluenceTd">1</td><td colspan="1" class="confluenceTd">/api/chat?seq=3</td></tr></tbody></table>

**Response:**

* Response when there are new messages:

	```json
	{
    	"messages": [ {"nick":"user1", "message":"hi there"},
                  {"nick":"user2", "message":"hola"} ],
    	"next_seq": 6
    }
    ```

* Response when there are no new messages:

	```json
	{
    	"messages": [],
    	"next_seq": 6
    }
    ```

     
**Errors:** 

<table class="confluenceTable"><tbody><tr><th class="confluenceTh">Status Code</th><th class="confluenceTh">Body</th><th class="confluenceTh">Description</th></tr><tr><td colspan="1" class="confluenceTd">400</td><td colspan="1" class="confluenceTd"><table class="wysiwyg-macro" data-macro-name="code" style="background-image: url(/confluence/plugins/servlet/confluence/placeholder/macro-heading?definition=e2NvZGV9&amp;locale=en_GB&amp;version=2); background-repeat: no-repeat;" data-macro-body-type="PLAIN_TEXT"><tr><td class="wysiwyg-macro-body"><pre>{"error":"invalid seq parameter"}</pre></td></tr></table></td><td colspan="1" class="confluenceTd">When the seq parameter is invalid (e.g. an string)</td></tr></tbody></table>


#### Send public chat messages ####

**Method:** POST

**Headers**
<table class="confluenceTable"><tbody><tr><th class="confluenceTh">Name</th><th class="confluenceTh">Description</th><th colspan="1" class="confluenceTh">Cardinality</th><th class="confluenceTh">Example</th></tr><tr><td colspan="1" class="confluenceTd">X-User-Nick</td><td colspan="1" class="confluenceTd">The nick of the user making the request</td><td colspan="1" class="confluenceTd">1</td><td colspan="1" class="confluenceTd">X-User-Nick: juan</td></tr></tbody></table>


**Body:**

```json
{
    "message": "Hola Mundo Reader"
}
```

**Errors:**

<table class="confluenceTable"><tbody><tr><th class="confluenceTh">Status Code</th><th class="confluenceTh">Body</th><th class="confluenceTh">Description</th></tr><tr><td colspan="1" class="confluenceTd">400</td><td colspan="1" class="confluenceTd"><table class="wysiwyg-macro" data-macro-name="code" style="background-image: url(/confluence/plugins/servlet/confluence/placeholder/macro-heading?definition=e2NvZGV9&amp;locale=en_GB&amp;version=2); background-repeat: no-repeat;" data-macro-body-type="PLAIN_TEXT"><tr><td class="wysiwyg-macro-body"><pre>{"error":"invalid body"}</pre></td></tr></table></td><td colspan="1" class="confluenceTd">When no body or invalid JSON is sent</td></tr><tr><td colspan="1" class="confluenceTd">400</td><td colspan="1" class="confluenceTd"><table class="wysiwyg-macro" data-macro-name="code" style="background-image: url(/confluence/plugins/servlet/confluence/placeholder/macro-heading?definition=e2NvZGV9&amp;locale=en_GB&amp;version=2); background-repeat: no-repeat;" data-macro-body-type="PLAIN_TEXT"><tr><td class="wysiwyg-macro-body"><pre>{"error":"missing nick parameter"}</pre></td></tr></table></td><td colspan="1" class="confluenceTd">When the user nick is missing in the body</td></tr><tr><td colspan="1" class="confluenceTd">400</td><td colspan="1" class="confluenceTd"><table class="wysiwyg-macro" data-macro-name="code" style="background-image: url(/confluence/plugins/servlet/confluence/placeholder/macro-heading?definition=e2NvZGV9&amp;locale=en_GB&amp;version=2); background-repeat: no-repeat;" data-macro-body-type="PLAIN_TEXT"><tr><td class="wysiwyg-macro-body"><pre>{"error":"missing message parameter"}</pre></td></tr></table></td><td colspan="1" class="confluenceTd">When the message is missing in the body</td></tr></tbody></table>


### Private Chat Resource ###

This the resource of the public chat.

**Endpoint:** ```/api/chat/{user_id}```

#### Get private chat messages ####

**Method:** GET

**Headers**
<table class="confluenceTable"><tbody><tr><th class="confluenceTh">Name</th><th class="confluenceTh">Description</th><th colspan="1" class="confluenceTh">Cardinality</th><th class="confluenceTh">Example</th></tr><tr><td colspan="1" class="confluenceTd">X-User-Nick</td><td colspan="1" class="confluenceTd">The nick of the user making the request</td><td colspan="1" class="confluenceTd">1</td><td colspan="1" class="confluenceTd">X-User-Nick: juan</td></tr></tbody></table>


**Parameters:**

<table class="confluenceTable"><tbody><tr><th class="confluenceTh">Name</th><th colspan="1" class="confluenceTh">Type</th><th class="confluenceTh">Description</th><th colspan="1" class="confluenceTh">Mandatory</th><th colspan="1" class="confluenceTh">Cardinality</th><th class="confluenceTh">Example</th></tr><tr><td colspan="1" class="confluenceTd">seq</td><td colspan="1" class="confluenceTd">Integer</td><td colspan="1" class="confluenceTd">Sequence from next message. No present if this is the first call</td><td colspan="1" class="confluenceTd">No</td><td colspan="1" class="confluenceTd">1</td><td colspan="1" class="confluenceTd">/api/chat?seq=3</td></tr></tbody></table>

**Response:**

* Response when there are new messages:

    ```json
    {
        "messages": [ {"nick":"user1", "message":"hi there"},
                  {"nick":"user2", "message":"hola"} ],
        "next_seq": 6
    }
    ````

* Response when there are no new messages:

    ```json
    {
        "messages": [],
        "next_seq": 6
    }
    ```

     
**Errors:** 

<table class="confluenceTable"><tbody><tr><th class="confluenceTh">Status Code</th><th class="confluenceTh">Body</th><th class="confluenceTh">Description</th></tr><tr><td colspan="1" class="confluenceTd">400</td><td colspan="1" class="confluenceTd"><table class="wysiwyg-macro" data-macro-name="code" style="background-image: url(/confluence/plugins/servlet/confluence/placeholder/macro-heading?definition=e2NvZGV9&amp;locale=en_GB&amp;version=2); background-repeat: no-repeat;" data-macro-body-type="PLAIN_TEXT"><tr><td class="wysiwyg-macro-body"><pre>{"error":"invalid seq parameter"}</pre></td></tr></table></td><td colspan="1" class="confluenceTd">When the seq parameter is invalid (e.g. an string)</td></tr></tbody></table>


#### Send private chat messages ####

**Method:** POST

**Headers**
<table class="confluenceTable"><tbody><tr><th class="confluenceTh">Name</th><th class="confluenceTh">Description</th><th colspan="1" class="confluenceTh">Cardinality</th><th class="confluenceTh">Example</th></tr><tr><td colspan="1" class="confluenceTd">X-User-Nick</td><td colspan="1" class="confluenceTd">The nick of the user making the request</td><td colspan="1" class="confluenceTd">1</td><td colspan="1" class="confluenceTd">X-User-Nick: juan</td></tr></tbody></table>


**Body:**

```json
{
    "message": "Hola Mundo Reader"
}
```

**Errors:**

<table class="confluenceTable"><tbody><tr><th class="confluenceTh">Status Code</th><th class="confluenceTh">Body</th><th class="confluenceTh">Description</th></tr><tr><td colspan="1" class="confluenceTd">400</td><td colspan="1" class="confluenceTd"><table class="wysiwyg-macro" data-macro-name="code" style="background-image: url(/confluence/plugins/servlet/confluence/placeholder/macro-heading?definition=e2NvZGV9&amp;locale=en_GB&amp;version=2); background-repeat: no-repeat;" data-macro-body-type="PLAIN_TEXT"><tr><td class="wysiwyg-macro-body"><pre>{"error":"invalid body"}</pre></td></tr></table></td><td colspan="1" class="confluenceTd">When no body or invalid JSON is sent</td></tr><tr><td colspan="1" class="confluenceTd">400</td><td colspan="1" class="confluenceTd"><table class="wysiwyg-macro" data-macro-name="code" style="background-image: url(/confluence/plugins/servlet/confluence/placeholder/macro-heading?definition=e2NvZGV9&amp;locale=en_GB&amp;version=2); background-repeat: no-repeat;" data-macro-body-type="PLAIN_TEXT"><tr><td class="wysiwyg-macro-body"><pre>{"error":"missing nick parameter"}</pre></td></tr></table></td><td colspan="1" class="confluenceTd">When the user nick is missing in the body</td></tr><tr><td colspan="1" class="confluenceTd">400</td><td colspan="1" class="confluenceTd"><table class="wysiwyg-macro" data-macro-name="code" style="background-image: url(/confluence/plugins/servlet/confluence/placeholder/macro-heading?definition=e2NvZGV9&amp;locale=en_GB&amp;version=2); background-repeat: no-repeat;" data-macro-body-type="PLAIN_TEXT"><tr><td class="wysiwyg-macro-body"><pre>{"error":"missing message parameter"}</pre></td></tr></table></td><td colspan="1" class="confluenceTd">When the message is missing in the body</td></tr></tbody></table>


<a rel="license" href="http://creativecommons.org/licenses/by/3.0/deed.en_US"><img alt="Creative Commons License" style="border-width:0" src="http://i.creativecommons.org/l/by/3.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/3.0/deed.en_US">Creative Commons Attribution 3.0 Unported License</a>.
