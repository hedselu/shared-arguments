# shared-arguments
Ruby gem for GraphQL arguments sharing within fields

**Sponsored by**
<br>
<br>
<a href="https:///www.indoorway.com/" target="_blank" rel="noopener noreferrer">
  <img src="images/indoorway_logo.png" height="58" width="170" alt="Sponsored by Indoorway" style="max-width:100%;">
</a>

## Contents
<!-- Table of contents generated generated by http://tableofcontent.eu -->
- [Description](#description)
- [Installation](#installation)
- [Setup](#setup)
- [Sample use case](#sample-use-case)

## Description

Sometimes you want to use the same arguments for multiple fields in custom object type.
To avoid repeating those declarations and DRY-up your schema, I introduce you shared_arguments field.

This gem is mostly usable when you have:
 - many fields with few repeating arguments in each field
 - few fields with many repeating arguments in each field
 - many fields with many repeating arguments in each field

## Installation

`gem install shared-arguments`

## Setup

Include SharedArguments in your schema

```ruby
#graphql/your_schema.rb

YourSchema = GraphQL::Schema.define do
  use GraphQL::SharedArguments.new
end
```

## Sample use case

```ruby
#graphql/types/analytics_type.rb

Types::AnalyticsType = GraphQL::ObjectType.define do
    name 'Analytics'
    
    field :invoices, types.[Types::Invoice] do
       argument :from, !Types::DateType
       argument :to, !Types::DateType
       argument :companyID, types.ID
    end
    
    field :registeredUsers, types.[Types::User] do
       argument :from, !Types::DateType
       argument :to, !Types::DateType
       argument :companyID, types.ID
    end
    
    field :downloadsAmount, types.Int do
       argument :from, !Types::DateType
       argument :to, !Types::DateType
       argument :userAmount, types.ID
    end
end
```

This can be shortened like in below example

```ruby
#graphql/types/analytics_type.rb

Types::AnalyticsType = GraphQL::ObjectType.define do
    name 'Analytics'
    
    field :invoices, types.[Types::Invoice]
    
    field :registeredUsers, types.[Types::User]
    
    field :downloadsAmount, types.Int do
       argument :userAmount, types.ID
    end
    
    shared_arguments do
       argument :from, !Types::DateType
       argument :to, !Types::DateType
    end
    
    shared_arguments except: %i(downloadsAmount) do
       argument :companyID, types.ID
    end
end
```

`shared_arguments` field takes both `except` and `only` keyword with array of symbols so you can omit or select specified fields.

Any contributions/suggestions/opened issues are welcomed.

License: MIT
