---
description: Information for advanced users of the API.
---

# Advanced Usage

In addition the `search`, an alternative approach to querying data is with the underlying `_search` function. Instead of using input parameters defined in the client, direct API query parameters are used instead. Documentation on which of these are available for each endpoint can be found [here](https://api.materialsproject.org/docs) or in the [API endpoint documentation sections](../api-endpoint-documentation/). While this information is provided, **most people can and should stick to using the standard `search` function.**

The code below is an example illustrating the difference in querying for data from the summary endpoint with volume criteria using `search` and `_search`:

```python
from mp_api import MPRester

with MPRester("your_api_key_here") as mpr:
    # -- Using 'search'
    docs = mpr.summary.search(volume=(3,10))
    
    # -- Using '_search'
    docs = mpr.summary._search(volume_min=3, volume_max=10)
```

{% tabs %}
{% tab title="Relevant Code Links" %}
{% embed url="https://github.com/materialsproject/api/blob/main/src/mp_api/core/client.py#L760-L796" %}
`_search` method.
{% endembed %}
{% endtab %}
{% endtabs %}
