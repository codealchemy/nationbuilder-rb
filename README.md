# nationbuilder-rb

[![Build Status](https://travis-ci.org/3dna/nationbuilder-rb.svg?branch=master)](https://travis-ci.org/3dna/nationbuilder-rb)

A Ruby client for the NationBuilder API.

## Installing

To install this gem to your current gemset, run:

```shell
$ gem install nationbuilder-rb
```

Or, add this to your gemfile:

```ruby
gem 'nationbuilder-rb'
```

## Creating a client

Require the gem:

```ruby
require 'nationbuilder'
```

Then, create a client by specifying the name of your nation and
your API token:

```ruby
client = NationBuilder::Client.new('my_nation_name', 'my_api_token')
```

## Calling the API

The primary method for calling the NationBuilder API in
`nationbuilder-rb` is the `#call` method, used on a client. The
`#call` method takes three parameters: the name of the endpoint,
the name of the method, and an optional hash containing arguments
for the method.

## Examples

### Fetching a person

```ruby
client.call(:people, :show, id: 15)
```

### Creating a person

```ruby
params = {
  person: {
    email: "bob@example.com",
    last_name: "Smith",
    first_name: "Bob"
  }
}

client.call(:people, :create, params)
```

### Destroying a person

```ruby
client.call(:people, :destroy, id: 15)
```

## Pagination

A call can be paginated by creating a new instance of the `Paginator` class. This takes two parameters: the client and the response of the call to be paginated.

```ruby
response = client.call(:people, :index)

paginated = Paginator.new(client, response)
page1 = paginated
page2 = page1.next
page3 = page2.next
```
Methods `#next` and `#prev` return the results of the next or previous page of results, nil if none. `#next?` and `#prev?` return the path to the next or prev page, or nil if none. The results of a page can be accessed through `.body` - in the above example, `page1.body` returns the same hash as `response`. 

## Documentation

This gem includes a command line utility that prints out
all covered endpoints, methods, and parameters. To see
documentation for a single endpoint, run:

```shell
$ nbdoc ENDPOINT_NAME
```

To see all documentation, don't provide a endpoint name:

```shell
$ nbdoc
```

### Example

```
$ nbdoc basic_pages

=====================
Endpoint: basic_pages
=====================

  Method: index
  Description: Shows a list of the basic pages in the system
  Required parameters: site_slug

  Method: create
  Description: Creates a basic page for a site
  Required parameters: site_slug

...
```