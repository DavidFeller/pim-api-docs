# Reconcile PIM data with print features capabilities

## Main goal

Congratulations! Now that you have a good overview of Akeneo PIM data, you need to reconcile it with your print solution capabilities.

Next step, for each piece of PIM data, you need to ask yourself the following questions:

1. What data does my print solution need from the PIM?
2. How will the configuration UI of my connector fulfill all of my end users' needs?  
3. How flexible does my connector need to be to adapt to any catalog structure created with the PIM?
4. Documentation: What do I need to document? (Data processing, feature limits, possible settings...)   

In order to help you develop your connector features, here are some useful features you can use. 

## API connection configuration

**What is it?**

In order for your connector to communicate with Akeneo PIM, you need to provide a setting to invite Julia to enter her PIM API credentials.

**What do you need to implement?**

* A UI or a “configuration file” to copy and paste Akeneo PIM API credentials (client id, secret, connection username and password).
* If you create a UI, you should add a “Test” button to test the connection (in order to check that everything is correctly configured on this side, and to start retrieving some configuration information from Akeneo PIM)


## Product management

### Manage products


**What is it?**

You need to import products from the PIM into your print solution.
But which products? All of them?

Products are enriched in Akeneo PIM but not all of them are meant to be exported to your print platform.

You should allow your end-users to define which PIM data they want to import via an advanced filter system. based on PIM structural elements such as channels, locales, attribute groups, you name it!

Here are some examples of filters you should implement:

* Channel: Only import product date related to the selected channel (most of the time a "print" channel).
* Locale: Only import product data related to the selected locale.
* Completeness: A PIM product can be exported if it is complete in terms of data contents, translation (localizable attribute) and information specific to the channels you are going to use (scopable attribute).
* Enabled/Disabled status: Each PIM product has an enable/disable status. A PIM product can be exported if it has an enabled status only.

And sometimes, your customers may also need to import a PIM product into the print solution based on the following parameters:

* Published product: some PIM customers can use this feature to work on different versions of the product. In the PIM, you can manage two different versions of a very same product: one “published” version that can be exported (using a published product export profile) and another version that is used to prepare the next product collection or season, for example. This is handled by the Akeneo PIM “Publication” feature.

* The value of a specific attribute

* A category tree or a subcategory tree…
...

**Conclusion:** each customer project has its own needs and it is important to implement enough flexibility in your connector settings to allow each customer to import only the product information he needs according to his criteria.

**What do you need to implement?**

In the features of your connector configuration, you can add a “simple” and an “advanced” filter system to toggle from one to the other according to your customers' needs.

### Product with variants

**What is it?**

Products with variants are products that have similarities, they are based on the same model but differ in some aspects from one another.

Please consult [our documentation](https://help.akeneo.com/pim/v3/articles/what-about-products-variants.html), to learn everything about PIM product with variants:

::: info
* PIM can manage up to 3 levels of enrichment (1 or 2 variant levels)
  * 1 or 2 levels of “product model”
  * 1 level of variant “product”
* PIM can have up to 5 attributes as variant axes for each level
* PIM variant axes can be:
  * Simple select
  * Reference entity single link (EE only)
  * Measurement
  * Boolean (Yes/No)
:::

**What do you need to implement?**

Compared to your print solution and your end-user needs, you could offer a UI that allows your users to define how these variants will be displayed on the paper catalog page.


### Enabled/disabled status

**What is it?**

A product is rarely removed. It is more often “disabled” to keep its history.

**What do you need to implement?**

Please consider relying only on the enabled/disabled status of the PIM product to know if a product can be used with your paper catalog or not.

## Media management

### Manage image

**What is it?**

A PIM can manage product images in many different ways:
* Images can be managed as binaries or external DAM/CDN URLs.
* Images can be stored in an “image” attribute (one image per attribute) or as assets linked to products that are  managed as an “asset collection” attribute (multiple images per attribute).
* Image attributes are localizable (different images for each locale) and/or scopable (different assets for each channel).
* Image can have metadata (assets with a corresponding set of text attributes)
These image metadata are scopable and/or localizable.
* Asset Images can be ordered at the product level

**What do you need to implement?**

Apart from taking into consideration all these PIM modeling capabilities, you also need to take into account some print solution specific features such as:

* How to define product attributes that represent the product images
* How to define the “main” image of the product (Should be bigger than the ones on your print solution)
* Define if you need to import metadata or not (could be the image caption)

::: warning
The number of images that a catalog can contain can be quite large and we know that images dedicated to a print catalog can be very heavy (high definition).
Make sure you have all the mechanisms in your connector to optimize the performance to manage image binaries.
:::


### Manage product associations

**What is it?**

In a PIM, you can manage different types of associations such as:
* Cross-sell
* Up-sell
* Substitutions
* Pack
* Custom association

Some of your customers may want to use these associations for their print catalog.

**What do you need to implement?**

Well, here, it depends on your customers' needs.

Also, don't forget our new feature that adds [quantities to PIM associations](https://help.akeneo.com/pim/serenity/updates/2020-07.html#new-association-type-with-quantities).

### What about PIM product groups feature?

**What is it?**

Groups are used to bring products together.
In Akeneo PIM, you can group a selection of products to create a theme, for example a collection of products for Christmas.
But most of the time, this feature is for internal purpose and product groups don’t have to be exported to your print solution.

**What do you need to implement?**

As product groups can be used for so many different use-cases and for internal purposes most of the time, we don't recommend that you manage this PIM feature (but, once again, do not hesitate to discuss this with your customers in order to evaluate their needs).

## Category management

![category mapping](../../img/guides/configuration-category.png)

**What is it?**

In a print solution, categories can be used to define how to organize your products in your print catalog.

At PIM level, the same catalog can have several navigation trees. Some category trees are only used for PIM purposes and are not intended to be exported. Other category trees can be directly used by your print solution.


**What do you need to implement?**

Remember that for each PIM channel, Julia can associate a category tree: your users only need to see the category tree associated to the "Print" channel.

Then with this category tree, your end-users :
* can define how the products are organized in the paper catalog
* can create navigation tabs for their print catalog
* ...


## What about Reference Entities?

**What is it?**

Please consult the introduction of [our documentation on Reference Entities](https://help.akeneo.com/pim/v3/articles/what-about-reference-entities.html) to understand how this can be used for print catalog purposes:

As you can see, Reference Entities can be used to enrich product information or to create dedicated pages with product relationships.

The main problem here is that Julia can use "Reference Entities" for a wide variety of needs.

**What do you need to implement?**

Well, two main use-cases here:

1. **Manage Reference Entities as a specific print page for a set of products**

As an entity reference can represent a product "universe" (ie: dedicated editorial content for a brand or a designer), you could enable your users:
* to organize their products according to these reference entities
* to create a specific header page to present this "universe" for a brand, a designer...


2. **Manage Reference Entities as rich information around a specific product attribute**

Because a reference entity can be related to a specific attribute (ie: details of a color, a material...), you could enable your users:
* to display this information at product level
* to feature this information on a dedicated page