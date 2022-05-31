# Concept overview

A concept is an entity that is actively in use in the product or service.

With concepts, it's expected to explain the domain, rather than implementation details.
It should provide engineers and non-engineers with a good conceptual understanding of the product, service, or API.

The concept document should include the following headers:

# Concept `<Title>`

## Overview

Explain the entity and what it is from the real world perspective.
How it relates to the other entities, from the real world or HiiRetail services perspective.
How it should be used to cover real-world scenarios.

## Examples

Examples are there to make it easier for the reader to understand the concept.

An example can be:
* Text
* Diagram or drawing
* JSON representation of the entity.

## Related services (optional)

List of the services handling the entity.

## Types (optional)

Usually, an entity can take different forms with somewhat different meaning.
All those forms should be listed and explained here.
For example,
if the context is `IAM` and the concept is `tokens` it would be natural to see both `IAM User token` and `OCMS token`.

## States (optional)

In many cases, an entity may have different states, which affect how it's handled.
Those states along with explanations should be stated here.
For example,
if the context is `Master data` and the concept is `Item` the following states should be described.

* Active – the item is available for normal operation
* Discontinued - the item is deprecated, it could still be sold to clear stock but should not be available for ordering
* Expired – the item is no longer in service and should be ignored
