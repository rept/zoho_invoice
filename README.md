# Zoho Invoice

[![Build Status](https://travis-ci.org/rept/zoho_invoice.png?branch=master)](https://travis-ci.org/rept/zoho_invoice)
[![Coverage Status](https://coveralls.io/repos/rept/zoho_invoice/badge.png?branch=master)](https://coveralls.io/r/rept/zoho_invoice)

Need to interact with the Zoho Invoice API?  The zoho_invoice gem has your back.

##### Attention: this Gem is fork of the neovintage gem which didn't seem to get any updates anymore.
I have merged Alex Sherstinsky pull request and committed some fixes.  Units will be added when needed.  If you want to do commit I'll do my best to merge them!  I use the gem myself in a production environment.   

## Installation

Add this line to your application's Gemfile:

    gem 'zoho_invoice', :git => 'https://github.com/rept/zoho_invoice.git'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install zoho_invoice, :git => 'https://github.com/rept/zoho_invoice.git'

## Configuration

The Zoho Invoice API service requires that you generate an auth token ahead of time using the API, just once.  Fortunately, the gem can handle that in console for you, all you need is your username and password.
Then you can save the authtoken in your project based on however you do your configuration.

```
homebase $ irb
> require 'zoho_invoice'
 => true
> result = ZohoInvoice::AuthToken.generate_authtoken('dude@example.com', 'thisismysweetpassword')
 => #<struct ZohoInvoice::AuthToken::AuthTokenResult authtoken="blahblahblahnumbersnstuff", cause=nil>
> result.success?
 => true
> result.authtoken
 => "blahblahblahnumbersnstuff"
```

## Client Setup

One of the great things about zoho_invoice is the ability for clients to inherit from a global config and override it if need be.

```
homebase $ irb
> require 'zoho_invoice'
 => true
> ZohoInvoice.authtoken = 'first_authtoken'
 => "first_authtoken"
> ZohoInvoice.apikey = 'first_apikey'
 => "first_apikey"
> client = ZohoInvoice::Client.new
 => #<ZohoInvoice::Client:0x007f9e6cf274b8 @authtoken="first_authtoken", @scope="invoiceapi", @apikey="first_apikey", @client_options={:url=>"https://invoice.zoho.com"}>
> another_client = ZohoInvoice::Client.new(:authtoken => "override_authtoken")
 => #<ZohoInvoice::Client:0x007f9e6cf395a0 @authtoken="override_authtoken", @scope="invoiceapi", @apikey="first_apikey", @client_options={:url=>"https://invoice.zoho.com"}>
```

## Usage

```ruby
require 'zoho_invoice'

client = ZohoInvoice::Client.new(:authtoken => 'my authtoken', :apikey => 'my apikey')

invoice = ZohoInvoice::Invoice.new(client, :customer_id => 'asdf')
invoice.save
```

If you're so inclined, you can scope the resources through the client.

```ruby
require 'zoho_invoice'

client = ZohoInvoice::Client.new(:authtoken => 'my authtoken', :apikey => 'my apikey')
invoice = client.invoices.new(:customer_id => 'asdf')
invoice.save
```

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
