# *Flexirest:* JSON API

If you are working with a [JSON API](http://jsonapi.org), you need to activate JSON API by specifying the `json_api` proxy:

```ruby
class Article < Flexirest::Base
  proxy :json_api
end
```

This proxy translates requests according to the JSON API specifications, parses responses, and retrieves linked resources. It also adds the `Accept: application/vnd.api+json` header for all requests.

It supports lazy loading by default. Unless a compound document is returned from the connected JSON API service, it will make another request to the service for the specified linked resource.

To reduce the number of requests to the service, you can ask the service to include the linked resources in the response. Such responses are called "compound documents". To do this, use the `includes` method:

```ruby
# Makes a call to /articles with parameters: include=images
Article.includes(:images).all

# For nested resources, the include parameter becomes: include=images.tags,images.photographer
Article.includes(:images => [:tags, :photographer]).all
```

For POST and PATCH requests, the proxy formats a JSON API compliant request, and adds a `Content-Type: application/vnd.api+json` header. It guesses the `type` value in the resource object from the class name, but it can be set specifically with `alias_type`:

```ruby
class Photographer < Flexirest::Base
  proxy :json_api
  # Sets the type in the resource object to "people"
  alias_type :people

  patch :update, '/photographers/:id'
end
```

NB: Updating relationships is not yet supported.


-----

[< Plain requests](plain-requests.md) | [Proxying APIs >](proxying-apis.md)
