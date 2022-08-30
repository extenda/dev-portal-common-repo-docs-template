# Developer portal docs repository template

This repo contains a writing guide and a template for the new Hii Retail Developer Portal docs repository.

## Writing guide

### Audience
The audience is developers and non-engineers, both internal and external.

## Entity hierarchy

To solve problems of the previous iteration of the Developer Portal a new docs hierarchy proposed.
New hierarchy is aimed at better connecting different pieces of Hii Retail software, presenting a complete and unified view of our solutions.

The diagram below represents entity hierarchy in the developer portal:

```
Developer Portal
│
└───Product Collection
│   │
│   │   Docs   
│   │   Changelogs
│   │  
│   └───Product
│       │  
│       │ Docs (private + public)
│       │ Changelogs
│       │
│       └───Service 
│           │
│           │ DOCS (private + public)
│           │ API references
│           │ Changelogs
```

### Service
The **Service** is the bottom level entity in the hierarchy and an atomic building block.

Example: IAM UI, IAM API, IAM Worker, OCMS API, OCMS UI, etc.

The **Audience** is internal and external developers, mostly interested in technical details and internal workings of the service.

Notes:
* Docs for the service may contain:
  * Private (internal) docs: 
    * architectural diagrams
    * usage guides
    * links to the Slack channels of maintainers
    * implementation details
    * ADRs
    * SRE information
  * Public (external) docs: 
    * user guides
    * API references
* APIs are documented using reference docs. 
API reference docs are **automatically generated** from the openapi spec file. It means that you *do not need* to create separate docs (.md) for your APIs. 
**The documentation of the API is the openapi spec file, that follows Development Chapter guidelines**.

### Product
The **Product** is a group of services, which together deliver a complete set of functionality.

Example: IAM, OCMS, External Events.

The **Audience** is internal and external developers. Unlike **Service**, the documentation for the Product should be less technical and more generalized.
It should explain what is the purpose of the product, give a more general overview of the functionality and refer to each service's docs for more technical details.

Notes:
* Docs for the product may contain:
  * Private (internal) docs: 
    * architectural diagrams
    * ADRs
  * Public (external) docs: 
    * purpose (which business problem it solves)
    * functionality overview
    * use cases
    * concepts and terminology
    * links to technical details (services docs)



### Product Collection
The **Product Collection** is the top-level entity of the Developer Portal, which represents a "sellable" entity

Example: Hii Retail Core is a product collection, consisting of several products: IAM, OCMS, External Events

The **Audience** is _managers and non-technical personal_

**Docs for the product collection will be delivered by the marketing department**


## File structure

To reflect [entity hierarchy](#entity-hierarchy) in the docs repository, please follow these guidelines:

Note: In diagrams below "📂" marks folders

### Product collection folder
Create folders for each product collection, developed by your tribe.

```
developer-portal 📂
│
└───Product Collection 1 📂
│   │  
│   │  ... 
│     
└───Product Collection 2 📂
│   │  
│   │  ... 
```

For each product collection folder you have, do the following:

* Create `docs` folder to store `*.md` marketing materials and other high-level docs of the product collection
* Create `images` folder to store images 
* Create `products` folder. You will define products that make up your product collection
* Create [`concepts.md`](templates/concepts.md) file, describing key concepts
* Create [`index.md`](templates/product.md) file. This file is the root doc of your product collection, containing a high-level overview and marketing materials. `index.md` **is mandatory.**
* Create [`terminology.md`](templates/terminology.md) file to describe the terminology used by the product.
* Create [`product-collection.yaml`](#productyaml-file) file. This file will be used to declare everything related to your product collection: its underlying products and services.

Now, the file structure should look like this:

```
developer-portal 📂
│
└───Product Collection 1 📂 
│   │
│   │   concepts.md
│   │   terminology.md
│   │   index.md
│   │   product-collection.yaml
│   │
│   └───docs 📂
│   │   ...
│   │ 
│   └───images 📂
│   │   ...
│   │ 
│   └───products 📂
│   │   ...
│   
└───Product Collection 2 📂  
│   ...
```

### Product folder
In your `products` folder create folders for each product, related to the product collection your team develops

```
developer-portal 📂
│
└───Product Collection 1 📂 
│   │
│   │   concepts.md
│   │   terminology.md
│   │   index.md
│   │   product-collection.yaml
│   │
│   └───docs 📂
│   │   ...
│   │ 
│   └───images 📂
│   │   ...
│   │ 
│   └───products 📂
│   │     │
│   │     └───Product 1
│   │     │
│   │     └───Product 2
│   │     │
```

For each product folder you have, do the following:

* Create `docs` folder to store `*.md` documentation of each product.
* Create `services` folder to store documentation of the services that are a part of your product.
* Create `images` folder to store product-level images.
* Create [`index.md`](templates/service.md) file. This file is the root doc of your product, containing a high-level overview. `index.md` **is mandatory.**.
* Create [`product.yaml`](#serviceyaml-file) file. This file will be used to declare all sub parts of the product.

The folder structure should look like this:

```
developer-portal 📂
│
└───Product Collection 1 📂
│   │   concepts.md
│   │   terminology.md
│   │   index.md
│   │   product-collection.yaml
│   │  
│   └───docs 📂
│   │   │   doc1.md
│   │   │   doc2.md
│   │   │   ... 
│   │   
│   └───images 📂
│   │   │   img1.png
│   │   │   img2.png
│   │   │   ... 
│   │  
│   └───products 📂
│       │  
│       └───Product 1 📂
│       │    │  
│       │    │   index.md
│       │    │   product.yaml
│       │    │   
│       │    └───docs 📂
│       │    │    │    doc1.md
│       │    │    │    doc2.md
│       │    │    │    ...     
│       │    │    
│       │    └───images 📂
│       │    │    │    img1.png
│       │    │    │    img2.png
│       │    │    │    ...   
│       │    │  
│       │    └───services 📂
│       │    │    │    
│       │    │    │    ...  
│   
│   
│   
└───Product 2 📂
    │  
    │  ...
```

### Service folder

Finally, for each service folder do the following:

* Create `docs` folder to store `*.md` documentation of your service
* Create `images` folder to store service-level images
* In case your service is an API, create [`api.yaml`](#apiyaml-file) file.

**Final folder structure** should look like this:

```
developer-portal 📂
│
└───Product Collection 1 📂             //Product folder
│   │   concepts.md                          //  Concepts
│   │   terminology.md                       //  Terminology    
│   │   index.md                             //  Root documentation of the Product
│   │   product-collection.yaml              //  Product collection definition file
│   │  
│   └───docs 📂                              //  Product-level docs folder
│   │   │   doc1.md
│   │   │   doc2.md
│   │   │   ... 
│   │   
│   └───images 📂                            //  Product-related images
│   │   │   img1.png
│   │   │   img2.png
│   │   │   ... 
│   │  
│   └───products 📂      //  Contains docs for all products in Product Collection 1
│       │  
│       └───Product 1 📂
│       │    │  
│       │    │   index.md
│       │    │   
│       │    └───docs 📂             // Product-level docs folder
│       │    │    │    doc1.md
│       │    │    │    doc2.md
│       │    │    │    ...     
│       │    │    
│       │    └───images 📂            //  Product-related images
│       │    │    │    img1.png
│       │    │    │    img2.png
│       │    │    │    ...   
│       │    │  
│       │    └───services 📂          //  Contains docs for all services in Product 1
│       │    │    │    
│       │    │    └───Service 1 📂        
│       │    │    │    │
│       │    │    │    │  api.yaml
│       │    │    │    │ 
│       │    │    │    └───docs 📂            //  Service-level docs
│       │    │    │    │    │   doc1.md
│       │    │    │    │    │   doc2.md
│       │    │    │    │    |
│       │    │    │    │    └───Doc Group 📂         //  Docs group
│       │    │    │    │           │   doc1.md
│       │    │    │    │           │   doc2.md
│       │    │    │    │           │   permissions.rbac.yaml            //  Marks Doc Group (and its content) as private (Extenda only) group
│       │    │    │    │           ...     
│       │    │    │    │
│       │    │    │    └───images 📂          //  Service-related images
│       │    │    │    │     │  img1.png
│       │    │    │    │     │  img2.png
│       │    │    │ 
│       │    │    │ 
│       │    │    └───Service 2 📂
│       │    │    │    │
│       │    │    │    │  ...               
│   
│   
└───Product 2 📂
    │  
    │  ...
```

### Private docs

Developer portal allows you creating private docs. These docs will only be visible to authenticated Extenda users.

Use private docs to explain architecture, internals of the service and other information that shouldn't be exposed to external developers.

#### Private pages

If a page doesn't have any specific permission, that means the page will use the default permission from the rbac.yaml file. By default, all pages are publicly accessible.

When a permission is specified for a page, it is treated as a "required permission". That means all visitors must have the specified permission mapped to their role in order to access the page.
To set permissions for Markdown and MDX pages, add permission: `authenticated-user`to their front matter, for example:
```markdown
---
permission: authenticated-user
---

...
Content of the page
```

#### Private folders

You can set permissions for all pages (and other files like static assets) in any directory at once. To configure permissions for all files in a directory (group) and all its child directories (nested groups), create a `permissions.rbac.yaml` file in that directory.

The `permissions.rbac.yaml` must contain the permission: `authenticated-user` entry like in the example:

```yaml
permission: authenticated-user
```

### Groups

To group your Markdown documents on the UI, so that they appear in an expandable list, put them into a separate folder inside your `docs` folder.

The **name of the folder** will be used as a title for the dropdown on the UI.

You can create groups for **Products**, **Services** and **APIs**. Developer portal supports **nested groups**.

```
developer-portal 📂
│
└───Product Collection 1 📂
│   │   concepts.md
│   │   terminology.md
│   │   index.md
│   │   product-collection.yaml
│   │  
│   └───docs 📂
│   │   │   doc1.md
│   │   │   doc2.md
│   │   │    
│   │   └───Doc Group 📂
│   │   │      │  
│   │   │      │  group-doc1.md   
│   │   │      │  group-doc2.md
│   │   │   
│   ...  
│   
└───Product Collection 2 📂
    │  
    │  ...
```


## product-collection.yaml file


`product-collection.yaml` is the main file needed to add your docs to the Developer Portal. Use it to define product collection information

The structure of the `product-collection.yaml`:


```yaml
- name: PRODUCT_COLLECTION_NAME             # Name of the product collection (example: Scan & Go, Hii Retail Core)
  shortName: prc1                            # Short name of the product collection
  description: string                               # Description of the product collection. Short and accurate
  coverImage: ./images/PRODUCT_COLLECTION_1.png        # Image link to be used as cover image of the product card in the Developer Portal
  tags:                                           # Optional, helps to find the product faster on the "Products" page
    - tag1                                   
    - tag2
```


## product.yaml file

`product.yaml` is used to define a Product. 

The structure of the `product.yaml` is the same as for the `product-collection.yaml` :

```yaml
- name: PRODUCT_NAME                        # Name of the product
  shortName: pr1                           # Short name of the product
  description: string                       # Description of the product. Short and accurate
  coverImage: ./images/PRODUCT_1.png        # Image link to be used as cover image of the product card in the Developer Portal
  tags:                                     # Optional, helps to find the product faster on the "Products" page
    - tag1                                   
    - tag2
```


## service.yaml file

`api.yaml` is the file used to define an API of your service.

The structure of the `api.yaml` file:

```yaml
name: API_1                                     # Name of the API
shortName: api1                                 # Short name of the API
description: API_1 Description                  # Description of the product. Short and accurate
specUrl: https://test.com/openapi.json          # Link to the openapi spec file
tags:                                           # Optional, helps to find the API faster on the "APIs" page
  - tag1
  - tag2
```

# Sync docs action spec

Specs for the sync docs action can be found [here](SYNC-DOCS.md)

