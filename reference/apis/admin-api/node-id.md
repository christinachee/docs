# Using global node IDs

The node `id` is a globally unique identifier of an object. It is needed when you call the GraphQL query and mutation. In this section, we will go through how to generate the node ID for calling the Admin GraphQL API.

The node id is a [base64url](https://datatracker.ietf.org/doc/html/rfc4648#section-5) encoded string with the format of `<NODE_TYPE>:<ID>`. For example, in `User:97b1c929-842c-415c-a7df-6967efdda160` , the node is "User" while "97b1c929-842c-415c-a7df-6967efdda160" is the ID for a specific user.

## 1. Generate id for User node type

You can use your preferred programming language or tool to encode node IDs to base64url as shown below:

{% tabs %}
{% tab title="Go" %}
```go
package main

import (
	"encoding/base64"
	"fmt"
)

func main() {
	nodeID := base64.RawURLEncoding.EncodeToString([]byte("User:97b1c929-842c-415c-a7df-6967efdda160"))
	fmt.Println(nodeID)
}
```
{% endtab %}

{% tab title="Python" %}
```python
import base64

base64.urlsafe_b64encode(b'User:97b1c929-842c-415c-a7df-6967efdda160').replace(b'=', b'')
```
{% endtab %}
{% endtabs %}

### Fetch the User object example

The query:

```graphql
query {
  node(id: "<BASE64URL_ENCODED_USER_NODE_ID>") {
    ... on User {
      id
      standardAttributes
    }
  }
}
```



## Generate id for AuditLog node type&#x20;

{% tabs %}
{% tab title="Go" %}
```go
package main

import (
	"encoding/base64"
	"fmt"
)

func main() {
	nodeID := base64.RawURLEncoding.EncodeToString([]byte("AuditLog:000000000004d2e7"))
	fmt.Println(nodeID)
}
```
{% endtab %}

{% tab title="Python" %}
```python
import base64

base64.urlsafe_b64encode(b'AuditLog:000000000004d2e7').replace(b'=', b'')
```
{% endtab %}
{% endtabs %}

### Fetch AuditLog object example

Query:

```graphql
{
  node(id: "<BASE64URL_ENCODED_AUDIT_LOG_NODE_ID>") {
    ... on AuditLog {
      id
      data
    }
  }
}
```

sdsd
