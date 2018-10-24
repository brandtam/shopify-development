# Example: Re-usable Modules

___
## The Explanation
- Create a snippet that will display content passed to it to apply a more DRY approach to theme development.


___
## Example Files

###### path: src/snippets/module-text-block.liquid
```liquid
{% comment %} Namespace: mod_text_block_ {% endcomment %}

{% comment %} Check if enabled {% endcomment %}
{% if text_block_enable %}

  {% comment %} Initialize variables {% endcomment %}
  {% assign text_block_enable = text_block_enable %}
  {% assign text_block_image = text_block_image %}
  {% assign text_block_title = text_block_title %}
  {% assign text_block_textarea = text_block_textarea %}

  {% if text_block_enable %}
  <div class="grid--full">
    <div class="grid__item one-whole article sell-text">
      <div class="grid__item large--one-half sell-text-image lazyload"
        data-bgset="{% include 'bgset', image: text_block_image %}"
        data-sizes="auto"
        data-parent-fit="cover"
        style="background-image: url({{ text_block_image | img_url: '1200x' }});">
      </div>
      <div class="grid__item large--one-half sell-textarea">
        <h1>{{ text_block_title }}</h1>
        <div class="rte">
          <p>{{ text_block_textarea | strip_html | truncatewords: 100 }}</p>
        </div>
      </div>
    </div>
  </div>
  {% endif %}

  {% comment %} Clear variables to prevent polution. {% endcomment %}
  {% assign text_block_enable = blank %}
  {% assign text_block_image = blank %}
  {% assign text_block_title = blank %}
  {% assign text_block_textarea = blank %}

{% endif %}

```

###### path: src/template/collection.liquid
```liquid
{% comment %} Include the text with image block from the collection template using collection data. {% endcomment %}

{% include "module-text-block",
  text_block_enable: true,
  text_block_image: collection.image,
  text_block_title: collection.title,
  text_block_textarea: collection.description
%}
```

###### path: src/sections/landing-page.liquid
```liquid
{% comment %} Include the text with image block from a section using section data. {% endcomment %}

{% comment %} Include the module if the section is enabled. {% endcomment %}
{% if section.settings.enable %}
  {% include "module-text-block",
    text_block_enable: section.settings.enable,
    text_block_image: section.settings.image,
    text_block_title: section.settings.title,
    text_block_textarea: section.settings.description
  %}
{% endif %}

{% comment %} Section Settings to allow content manager to control content. {% endcomment %}
{% schema %}
  {
    "name": "Text Block",
    "settings": [
      {
        "label": "Enable",
        "id": "enable",
        "type": "checkbox",
        "default": true
      },
      {
        "label": "Image",
        "id": "image",
        "type": "image_picker",
        "info": "800 x 600px .png recommended"
      },
      {
        "label": "Title",
        "id": "title",
        "type": "text"
      },
      {
        "label": "Description",
        "id": "description",
        "type": "textarea"
      }
    ]
  }
{% endschema %}
```

###### path: src/sections/landing-page-multi-modules.liquid
```liquid
{% comment %} Include the text with image block from a section using block data.
This allows a section to have multiple blocks of different module typtes. {% endcomment %}

{% comment %} Iterate through blocks and include modules when required. {% endcomment %}
{% for block in section.blocks %}
  {% case block.type %}
    {% when "text-block" %}
      {% include "module-text-block",
        text_block_enable: block.settings.text_block_enable,
        text_block_image: block.settings.text_block_image,
        text_block_title: block.settings.text_block_title,
        text_block_textarea: block.settings.text_block_textarea
      %}
    {% when "collection-slider" %}
      {% include "module-collection-slider",
        collection_slider_enable: block.settings.collection_slider_enable,
        collection_slider_title: block.settings.collection_slider_title,
        collection_slider_collection: block.settings.collection_slider_collection
      %}
  {% endcase %}
{% endfor %}

{% comment %} Section Settings to allow content manager to control content. {% endcomment %}
{% schema %}
  {
    "name": "Customize Page",
    "settings": [],
    "blocks": [
      {
        "type": "text-block",
        "name": "Text Block",
        "settings": [
          {
            "label": "Enable",
            "id": "text_block_enable",
            "type": "checkbox",
            "default": false
          },
          {
            "label": "Image",
            "id": "text_block_image",
            "type": "image_picker"
          },
          {
            "label": "Title",
            "id": "text_block_title",
            "type": "text"
          },
          {
            "label": "Description",
            "id": "text_block_textarea",
            "type": "textarea"
          }
        ]
      },
      {
        "type": "collection-slider",
        "name": "Collection Slider",
        "settings": [
          {
            "label": "Enable",
            "id": "collection_slider_enable",
            "type": "checkbox",
            "default": false
          },
          {
            "label": "Title",
            "id": "collection_slider_title",
            "type": "text"
          },
          {
            "label": "Collection",
            "id": "collection_slider_collection",
            "type": "collection"
          }
        ]
      }
    ]
  }
{% endschema %}
```
