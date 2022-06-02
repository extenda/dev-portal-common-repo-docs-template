# Docs template repository

## Writing guide

### Audience
The audience is developers and non-engineers, both internal and external.


## Entity hierarchy

The diagram below represents entity hierarchy in the developer portal:

```
Developer Portal
│
└───Product
│   │
│   │   docs   
│   │  
│   └───Services
│       │  
│       │ docs
│       │ 
│       └───APIs 
│           │
│           │ references
│           │ 
```

### Product

The **Product** is the top-level entity of the Developer Portal

* A **Product** is a collection of **Services**.
* Each Product has its documentation written in markdown (.md), explaining how the underlying Services work.
* A Product may contain some marketing materials.
* The Product may contain multiple documents (.md files).
* The Product may contain both **public** and **private (internal)** docs.

### Service

* A **Service** is a collection of **APIs**.
* Each Service has its documentation written in markdown (.md), explaining how the underlying APIs work.
* The Service may contain multiple documents (.md files).
* The Service may contain both **public** and **private (internal)** docs.

### API

The **API** is the bottom level entity and an atomic building block of the Developer Portal. 

* APIs are documented using reference docs. API reference docs are automatically generated from the openapi spec file.
It means that you *do not need* to create separate docs (.md) for your APIs.
* **The documentation of the API is the openapi spec file, that follows Development Chapter guidelines**
* API documentation (reference docs) is always **public**




## File structure

To reflect [entity hierarchy](#entity-hierarchy) in your `<CLAN_NAME>-common` repository,
please follow these guidelines:

Note: In diagrams below "📂" marks folders


### Root folder
Your `<CLAN_NAME>-common` repository should contain a root `developer-portal` folder.
Files in the `developer-portal`
folder will be synced to the developer portal.


### Product folder
In your `developer-portal` folder create folders for each product, developed by your team.

```
developer-portal 📂
│
└───Product 1 📂
│   │  
│   │  ... 
│     
└───Product 2 📂
│   │  
│   │  ... 
```

For each product folder you have, do the following:

* Create `docs` folder to store `*.md` documentation
* Create `images` folder to store product-level images
* Create [`services`](templates/service.md) folder to store Service docs
* Create [`concepts.md`](templates/concepts.md) file, describing key concepts
* Create [`index.md`](templates/product.md) file. This file is the root doc of your product, containing a high-level overview.
* Create [`terminology.md`](templates/terminology.md) file to describe the terminology used by the product.
* Create `product.yaml` file. This file will be used to declare everything related to your product: its underlying services and their APIs.


Now, the file structure should look like this:

```
developer-portal 📂
│
└───Product 1 📂 
│   │
│   │   concepts.md
│   │   terminology.md
│   │   index.md
│   │   product.yaml
│   │
│   └───docs 📂
│   │   ...
│   │ 
│   └───images 📂
│   │   ...
│   │ 
│   └───services 📂
│   │   ...
│   
└───Product 2 📂  
│   ...
```



### Service folder
In your `services` folder create folders for each service, related to the product your team develops

```
developer-portal 📂
│
└───Product 1 📂 
│   │
│   │   concepts.md
│   │   terminology.md
│   │   index.md
│   │   product.yaml
│   │
│   └───docs 📂
│   │   ...
│   │ 
│   └───images 📂
│   │   ...
│   │ 
│   └───services 📂
│   │     │
│   │     └───Service 1
│   │     │
│   │     └───Service 2
│   │     │
```

For each service folder you have, do the following:

* Create `docs` folder to store `*.md` documentation
* Create `images` folder to store product-level images
* Create [`index.md`](templates/service.md) file. This file is the root doc of your service, containing a high-level overview.

Your final folder structure should look like this:


```
developer-portal 📂
│
└───Product 1 📂
│   │   concepts.md
│   │   terminology.md
│   │   index.md
│   │   product.yaml
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
│   └───services 📂
│       │  
│       └───Service 1 📂
│       │    │  
│       │    │   index.md
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
│   
│   
│   
└───Product 2 📂
    │  
    │  ...
```


### Private docs

Developer portal allows you creating private docs. These docs will only be visible to authenticated Extenda users.

Use private docs to explain architecture, 

#### Private pages

If a page doesn't have any specific permission, that means the page will use the default permission from the rbac.yaml file. By default, all pages are publicly accessible.

When a permission is specified for a page, it is treated as a "required permission". That means all visitors must have the specified permission mapped to their role in order to access the page.
To set permissions for Markdown and MDX pages, add permission: `authenticated-user`to their front matter, for example:
```markdown
---
title: My Private page
permission: authenticated-user
---

...
Content of the page
```


#### Private folders

You can set permissions for all pages (and other files like static assets) in any directory at once. To configure permissions for all files in a directory and all its child directories, create a `permissions.rbac.yaml` file in that directory.

The file must contain the permission: `authenticated-user` entry like in the example:

```yaml
permission: authenticated-user
```

## product.yaml file


`product.yaml` is the main file needed to add your docs to the Developer Portal. It defines products, services and APIs as well as how they should be displayed, which of them are private.

The structure of the product.yaml aimed at representing [entity hierarchy](#entity-hierarchy) of the Developer Portal:


```yaml
- name: PRODUCT_NAME                        # Name of the product
  shortName: pr1                            # Short name of the product
  description: string                       # Description of the product. Short and accurate
  coverImage: ./images/PRODUCT_1.png             # Image link to be used as cover image of the product card in the Developer Portal
  rootDoc: index.md                         # Root doc (high-level overview) of the product. This document is the entry point documentation of the product. Must not contain internal information
  tags:                                     # Optional, allows finding the product faster on the "Products" page
    - tag1                                   
    - tag2
  services:                                 # List of the product's services
    - name: Service 1                       # Name of the service 
      shortName: svc1                       # Short name of the service
      description: SERVICE_1 description    # Description of the service. Short and accurate
      coverImage: ./services/SERVICE_1/images/SERVICE_1.png    # Image link to be used as cover image of the service card in the Developer Portal
      rootDoc: ./services/SERVICE_1/index.md                   # Root doc (high-level overview) of the service. This document is the entry point documentation of the product. Must not contain internal information
      tags:                                                    # Optional, allows finding the service faster
        - SERVICE_1_tag1
        - SERVICE_1_tag2
      apis:                                               # Define a list of service's APIs
        - name: API_1                                     # Name of the API
          shortName: api1                                 # Short name of the API
          description: API_1 Description                  # Description of the product. Short and accurate
          tags:                                           # Optional, allows finding the API faster on the "APIs" page
            - tag1
            - tag2
          generateCodeSamples:                            # Control generated code samples for your API
            languages:                                    # Choose which programming languages to generate examples for
              - lang: curl
              - lang: JavaScript
              - lang: Node.js
              - lang: C#
          specUrl: https://test.com/openapi.json          # Link to the openapi spec file

```
