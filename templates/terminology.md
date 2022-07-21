# Terminology Overview
Terminology is a list of descriptions of entities and concepts that concerns the service.
Terminology can include entities that are important to understand the service even if it’s not directly used by the service. This is different from concepts where concepts are always actively used by the service.
 
Example: The POS is not something that the Price Service is actively interacting with. It’s simply expected that the POS will receive the prices produced by the Price Service. In this case, the POS is a term and not a concept from the Price Service perspective.
 
``` 
### POS
Final destination of price in the store where it will be used when making a purchase.
The above is the Price Service’s view of a POS. It can also be the link to the POS concept (where it is a concept).
``` 
 
# Requirements
* Every entry must be an anchor to link it from other documents.
* Terms must be sorted alphanumerically.
* Every document on the same level or below MUST use the term as a link at least on first usage.
* Existing concepts from other products/services MAY be linked to the terminology list.
* Existing concepts from other products/services MAY be linked into the terminology list when you need to provide different meanings or additional your-service-details.
* The terminology list MUST be consistent and MUST NOT use synonymous terms for the same entities. It MUST use the same term name on a product level as on a service level.
 
  
# Terminology Template
_It's expected that the `terminology.md` includes the following headers:_
### `<Name of term 1>`
Description of `Term 1` not in active use by service
 
### `<Name of term 2>`
Description of `Term 2` not in active use by service
 
### `<Name of concept 1>`
Add the description for your product/service context here (OPTIONAL - when you need to redefine the description). Read here about the original concept - Link to concept/concept1.md
 
 
 
