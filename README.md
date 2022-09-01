# Storyblok Field Type Swell Integration

> Field Type Integration for Swell Ecommerce. This Field Type plugin makes use of Swell SDK. 

## Project setup

```
yarn install
```

### Compiles and hot-reloads for development

Startups local development server running Vue. You will only be able to develop this custom field type by enabling the [local development option](https://www.storyblok.com/docs/Guides/Creating-a-field-type-plugin#how-to-develop-plugins-locally) with Storyblok.

```
yarn dev
```

### Compiles and minifies for production

Exports the final plugin that can be included in Storyblok under `export.js`. Upload its content to your plugin. Make sure that your plugin name configured in `/src/Plugin.vue` is the equal to the name of your actual Storyblok plugin. Each plugin name is globally unqiue, which means that across Storyblok there will only be one plugin with that exact name for all users, even tho not all users will have access to your plugin.

```
yarn build
```

## View: The Integration Selection Overview

The overview of your results will be shown as in the screen below. It will handle pagination generation.

![Example selection overview](https://a.storyblok.com/f/39898/3356x1750/d86bee6ad1/integration-field-selection.jpg)

## View: The Selected Value

The selected value will be shown as a box similar to the one you can see in the overview. It will also be marked in the overview itself so editors will see that they have already selected that value.

![One selected value](https://img2.storyblok.com/fit-in/1600x0/filters:fill(ffffff)/f/39898/702x324/e08b453c09/integration-field-selected.jpg)

## How to use this custom field type with your Swell Store?

When adding the field type, configure your `storeId` and a public `apiKey`. Everything else is handled by the plugin like pagination, search, product listing.

## The Integration Object 

Refer to Swell's Product API

```
{
  "id": "5e30cf72fc941e164ff1a6c4",
  "attributes": {
    "brand": {
      "visible": true,
      "type": "select",
      "name": "Brand",
      "id": "brand",
      "filterable": true,
      "value": "Porlex"
    }
  },
  "content": {
    "product_benefits": [
      {
        "icon": "uil:truck",
        "text": "Free US shipping on all orders over $100"
      },
      {
        "icon": "uil:clock-two",
        "text": "Free returns up to 30 days"
      },
      {
        "icon": "uil:award",
        "text": "98% customer satisfaction rating"
      }
    ],
    "enable_quantity": true,
    "max_quantity": 99,
    "up_sell_cols": 4
  },
  "currency": "USD",
  "description": "<p>Take freshly ground coffee with you anywhere with The Porlex Mini Coffee Grinder. Powerful, lightweight, small, and manually powered, it&rsquo;s an excellent travel companion that doesn&rsquo;t limit you to outlets or batteries.<br><br>The grinder&rsquo;s ceramic conical burrs are designed to remain sharp for many years and cannot rust. They produce consistently uniform grounds that are suitable for french press, espresso, and everything in between. Beneath the burrs is the grind adjustment knob, offering 12+ grind settings for easy adjustments.<br><br>Take it with you across the world or down the street. Either way, it&rsquo;ll grind your coffee with ease and consistency.</p>",
  "bundle": false,
  "cross_sells": [
    {
      "id": "5e30cfacc2e7c4a1ac91a573",
      "product_id": "5e30cdcccf1eb5144c14da50",
      "discount_type": "fixed",
      "discount_amount": null
    },
    {
      "id": "5e30cfbbc2e7c4a1ac91a574",
      "product_id": "5e308ef8fc9ee47b47ac0323",
      "discount_type": "fixed",
      "discount_amount": null
    }
  ],
  "images": [
    {
      "id": "5e31fbdfe4e37c39bc47feeb",
      "file": {
        "height": 1042,
        "md5": "291b4efb1aa395c0088adb8ac4e17938",
        "url": "https://cdn.schema.io/test-theme/5f6fd1908256191a6a1bc25a/291b4efb1aa395c0088adb8ac4e17938",
        "width": 1500
      }
    }
  ],
  "meta_description": "Take freshly ground coffee with you anywhere with The Porlex Mini Coffee Grinder.",
  "meta_title": null,
  "name": "Mini Hand Grinder",
  "options": [],
  "price": 48,
  "purchase_options": {
    "standard": {
      "price": 48,
      "sale": false,
      "sale_price": null
    }
  },
  "sale": false,
  "sku": "PRLX-MIN",
  "slug": "porlex-mini-hand-grinder",
  "stock_purchasable": true,
  "stock_status": null,
  "stock_tracking": true,
  "tags": [],
  "up_sells": [],
  "category_index": {
    "sort": {
      "5e31dbc0ae1309046a52f1a2": 0
    },
    "id": [
      "5e31dbc0ae1309046a52f1a2"
    ]
  },
  "cost": null
}
```
