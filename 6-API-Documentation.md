
RAPID DEVELOPMENT SYSTEM (RDS)

Description: Fundamental overview of RDS's web server API

Author: Topher

# RDS (FightView) API Documentation

## Overview
The **RDS API** provides powerful, simple and convenient methods for accessing services to support client applications. It
is built in conjuncture with an Apache2 Web Server and is coded in php with a common Model, View, Controller approach. It's aim is to provide a fundamental data driven modular development path.

All API requests are routed through the following endpoint:

127.0.0.1/api.php?

Two system parameters are required to engage with the system:

t=person
or 'table'

&s=read
or 'service'

The spirit of RDS is an unambigious mapping of a relational database table in this example 'person' to a dedicated model and controller. The combination of the table and service is here by defined as a route.

Routes are characterized by either being 'public' or 'private'. public routes can be accessed by anyone on the internet,  private require authentication.

If a client successfully authenticates with our RDS service they will recieve JSON Web Token. They can use this to accompany subsuqent requests to access private routes.

j=abcdefg...
or 'jwtoken'

The jwt is a clients access pass, it contains unencrypted basic informations about the clients profile. 
Like a physical key or access pass it should be safe guarded in thecorrect hands.

## REQUEST

eg. I want to retrieve one record that models a specifc person.

https://www.rds.app/api.php?
    t=person&                   table
    s=read&                     service
    j=abcdefg&                  json web token
    unique_ident=1234567890            unique table row identifier

# Row Parameters

When utilizing basic crud methods, RDS expectes a unique access key - unique_{column_name}. This will be validated by the table model.

unique_ident=124124124124124142&
unique_id=1&
unique_email=c.mansbridge@live.ca&

# List Parameters

To curate lists for CRUD operations, we must declare this intent with list={truthy value}

list=true&                                 
## Sorting

sort=first_name|asc,last_name|asc
## Pagination
page_number=2&
page_size=20&
return_count=true&
## Filtering
range_created_at=2024-01-01,2024-01-31&
filter_status=active&
filter_price_lt=100&
search_text=hello&
search_columns=name,description&

# Magic Parameters

include is a very powerful method for enhancing records with its relations. It leverages the extensive details in our table models for
building complex queries.

include=author,author_details,category&

Our default view = json. Here the response will be returned in json format with all table fields described in the model.
custom views out of the box handle the following: 
    -change the return format to other common forms such as html, csv, xml etc
    -declare relations to include, aka preset values for the include magic param
    -filter table or relation columns to specified values.
if a view is specified the values declared in the view will override any values passed with the magic param include *

view=json&

view=person_tree
    
    EG.

    $format = "csv"

    $include = [
        "company" => "*",
        "departments" => "id,name"
    ]

## RESPONSE

RDS is a JSON first API and all responses default to this markup style, this synergizes with our ability to deliver deeply nested relational data to a
base record in a readable easily parsed format. However the view element of the MVC paradigm is intentional left opened ended and flexible within RDS.

RDS mirrors the established system convention of providing an exit status of 0 for successful requests. This provides a simple flag for developers to handle the
binary outcome of a request. In the spirit of 'room to grow' we opt for initializing errors with a status value of 1 for unsuccessful requests, the message field is used as a tool for providing a unique string describing the error.

#Good request

{
    "status": 0,                            exit status: 1 or 0                
    "message": "Read successful",           debug tool
    "data": {                               request payload
        "first_name": "topher"
        ...
    }
}

Verbose error details are maintained on the server for developers, RDS out of the box aims to provide concise client friendly error messages.

#Bad request

{
    "status": 1,                    
    "message": "Client has insufficent privledges to access this resource"
    "data": null
}

## MODEL
The generation of models are heavily automated in RDS. They are built using a dedicated model_fragment.json file and extensive queried meta data. Once established
these are cached in memory utilized for every request.

A developer can expect a workflow similar to the following 

#SQL - add a new table
1) vim db/upgrades/{n}.sql

#BASH - upgrade the database
2) sbin/version.sh upgrade

#BASH - initialize the model by creating a fragment json file and a decicated controller file
2) sbin/model create table_name

JSON - utilize the fragment file for role based access, required params, data validation e 
declaring additional services and relational access extensions (see model docs for details)
*Specifically designed for developer tuning.
3) vim {table_name}_fragment.json

#BASH - prints out the complete model, contains extensive details for the table and relations 
4) sbin/model.sh inspect table_name

PHP - Utilize the dedicated controller file for custom services.
4) vim {table_name}.php

BASH - Test out of box services.
5) wget 127.0.0.1?t={table_name}&s=create&first_name=chris


## CONTROLLER
The base contoller file provides services common to all tables. 

examples = create,read,update,delete,form_create,form_update

less web/server/controller.php

Individual table controller files are provided for building custom services. Each service
requires certain details to be defined in the model such as needed params and  privledges

ls -l web/server/controllers/*


## VIEW
Out of box all matters concerned to views are housed in the view.php file. Here we have the default json view and is a location
for developers to add custom views directly with PHP. 





