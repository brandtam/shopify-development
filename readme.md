# Shopify Theme Development Best Practices
___
## Theme Architecture Basics
- Create reusable components as snippets, then use Sections to pass data to snippet, e.g. {% include 'hero', hero_title: section.settings.title %}
- Create module and/or helper snippets to create consistency and speed up development, e.g. /src/snippets/helper-image.liquid or /src/snippets/module-image-with-text.liquid

## Sections & Blocks
___
#### Sections
- Use Sections to custom settings for specific templates and areas whenever possible for maximum flexibility and theme management consistency.
- Avoid full page sections, prefer multiple sections to allow for multiple block instances.
- When creating Sections use the module snippets for the presentation and pass the section and/or block variables to the included module.
#### Blocks
- Use Blocks to add multiple copies of a single item such as images and image settings for an image slider.
- Alternatively use Blocks to add multiple types of content to a template.
- You can control the approach and end result by performing different actions on the section.blocks array.
- Include an enable checkbox and parameter to every block for flexibility. Often the need to disable a bit of content comes up and it is much more user friendly to be able to disable it and re-enable it in the future.
- Pass the block.id when passing variables to a module in case the module can use it or would like to in the future.

###### Example of a section using different block types and passing block values to module snippets.
```ruby
{% for block in section.blocks %}
  {% case block.type %}
    {% when "hero-image" %}
      {% include "module-hero-image",
        hero_image_block_id: block.id,
        hero_image_enable: block.settings.hero_image_enable,
        hero_image_title: block.settings.hero_image_title,
        hero_image_cta_text: block.settings.hero_image_cta_text
      %}
    {% when "image-with-text" %}
      {% include "module-image-with-text",
        image_with_text_id: block.id,
        image_with_text_enable: block.settings.image_with_text_enable,
        image_with_text_image: block.settings.image_with_text_image,
        image_with_text_text: block.settings.image_with_text_text
      %}
  {% endcase %}
{% endfor %}
```

## Modules & Helpers
___
#### Modules
- Modules are snippets that consume variables and output a complete section of content to the view layer.
- preface module files with `module-` to make them uniform and easy to find.
#### Helpers
- Helpers are snippets that consume variables and output a portion of a single bit of content, e.g. an image, or video that can be included as part of a larger section.
- preface helper files with `helper-` to make them uniform and easy to find.

## Theme Settings
___
##### Incomplete (Theme settings best practices)
- Template level data. Data is shared among all resources that use the same template.
-
## Metafields
___
##### Incomplete (Metafields best practices)
- Resource level Data (product, variant, customer, page, etc)
- You need an app that acts on the API in order to manage metafields.
- Metafields consist of three parts
    - Namespace - A container to isolate groups of metafields together.
    - Key - The name of the metafield.
    - Value - The value of the metafield that will be accessable via the API or theme objects.

## Images
___
##### Incomplete (Theme images best practices)
- Shopify has a maximum of 4472 x 4472px resolution.
- Encourage users to resize images to appropriate sizes prior to uploading.
- Encourage users to optimize images prior to uploading.
- Include Alt Tag on images.
- Use srcset when possible.
- https://www.shopify.com/partners/blog/img-url-filter
-

## Liquid Syntax
___
- Use soft-tabs with two spaces. Spaces are the only way to ensure code renders the same in any environment.
- Put one space after the opening Liquid markup `{{ ` and `{% `, as well as one space before the closing markup ` }}` and ` %}}`.
- Put one space before and after each filter statement, e.g. `{{ 5 | plus: 1 | divided_by: 2 }}`.
- Include one space after the colon `:` of filter parameters.
- Comma-separated parameter values should include a space after each comma, e.g. `{{ "foobar" | replace_first: "foo", "bar" }}`.
- Use double-quoted strings. Escaped characters will always work without a delimiter change, and ' is a lot more common than " in string literals. Only use single-quotes when necessary.
- Don’t quote integers, booleans or nil.

Example:
###### Bad
```
{% comment %}Bad{% endcomment %}
{{'foobar'|replace_first:'foo','bar'}}
```
###### Good
```ruby
{% comment %} Good {% endcomment %}
{{ "foobar" | replace_first: "foo", "bar" }}
```

## Variables

- Define all local variables at the top of the file.
- Use snake case (lowercase alphanumeric characters and underscores as spaces).
- Always start with a letter.

Example:
###### Bad
```
{% assign FooBar = "foo bar" %}
```
###### Good
```ruby
{% assign foo_bar = "foo bar" %}
```

## Comments

Code is written and maintained by people. Ensure your code is descriptive, well commented, and approachable by others. Great code comments convey context and/or purpose so that a stranger can understand.

Be sure to write in complete sentences for larger comments and succinct phrases for general notes.

Always use the Liquid comment tag for Liquid code, rather than HTML comments.

###### Bad
```
<!-- Replaces the first occurrence of "foo" with "bar" -->
{{'foobar'|replace_first:'foo','bar'}}
```
###### Good
```ruby
{% comment %} Replaces the first occurrence of "foo" with "bar" {% endcomment %}
{{ "foobar" | replace_first: "foo", "bar" }}
```

---

#### Additional resources

- [Liquid](https://shopify.github.io/liquid/)
- [Shopify Liquid Reference](https://help.shopify.com/en/themes/liquid)
- [Liquid template engine](https://github.com/Shopify/liquid)
