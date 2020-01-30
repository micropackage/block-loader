# Block Loader

[![BracketSpace Micropackage](https://img.shields.io/badge/BracketSpace-Micropackage-brightgreen)](https://bracketspace.com)
[![Latest Stable Version](https://poser.pugx.org/micropackage/block-loader/v/stable)](https://packagist.org/packages/micropackage/block-loader)
[![PHP from Packagist](https://img.shields.io/packagist/php-v/micropackage/block-loader.svg)](https://packagist.org/packages/micropackage/block-loader)
[![Total Downloads](https://poser.pugx.org/micropackage/block-loader/downloads)](https://packagist.org/packages/micropackage/block-loader)
[![License](https://poser.pugx.org/micropackage/block-loader/license)](https://packagist.org/packages/micropackage/block-loader)

## 🧬 About Block Loader

Block Loader loads the Gutenberg Block configuration directly out of the block template file. It parses the file header comment figuring out hwo to register the Block.

Basically instead doing this:

```php
acf_register_block_type( [
	'name'              => 'sample-block',
	'title'             => __('Sample Block'),
	'render_template'   => 'blocks/sample-block.php',
] );
```

You can do initialize the loader once:

```php
Micropackage\BlockLoader\BlockLoader::init( [
	'dir' => 'blocks',
] );
```

And define the block config directly in the template (`blocks/sample-block.php`):

```
<?php
/**
 * Block Name: Sample Block
 */

...
```

**Supported plugins:**
- Advanced Custom Fields
- Meta Box

---

This micropackage is compatible with [ACF Block Creator](https://github.com/micropackage/acf-block-creator/) which can create the block templates for you while defining the Field group for your new block.

## 💾 Installation

``` bash
composer require micropackage/block-loader
```

## 🕹 Usage

Before you can start creating blocks you need to initiate the block loader passing optional config array:

```php
Micropackage\BlockLoader\BlockLoader::init( [
	'dir'        => 'blocks',
	'categories' => [],
	'wrap'       => true, // ACF only
	'wrap_html'  => '<div id="%3$s" class="%2$s">%1$s</div>', // ACF only
] );
```

Blocks are based on the template files located by default in `blocks` folder in your theme (you can change that).

There are two steps to create a block:
1. Create block template file in the `blocks` folder
2. Define custom fields for your block using ACF or Meta Box.

Block template file needs to have a comment header containing block parameters.

```
<?php
/**
 * Block Name: (required)
 * Description:
 * Category:
 * Icon:
 * Keywords: (comma-separated)
 * Post Types: 	(comma-separated, ACF only)
 * Mode: (ACF only)
 * Align: (ACF only)
 * Enqueue Style:
 * Enqueue Script:
 * Enqueue Assets:
 * Supports Align: (comma-separated)
 * Supports Anchor: (true|false)
 * Supports Custom Class Name: (true|false)
 * Supports Mode: (true|false, ACF only)
 * Supports Multiple: (true|false)
 * Supports Reusable: (true|false)
 */
```

### ACF

Creating template files is enough for ACF to register blocks. After that you only need to create new fields group and set it's location to your custom block.

### Meta Box

In Meta Box you need "MB Blocks" extension to work with blocks.
With this plugin custom fields are defined in code. You need to use `rwmb_meta_boxes` filter to create metabox for your block.

Let's say you have a template called `blocks/some-block.php`. You need to add the fields definition like this:

```php
add_filter( 'rwmb_meta_boxes', function( $meta_boxes ) {
	$meta_boxes[] = [
		'id'     => 'some-block',
		'type'   => 'block',
		'fields' => [
			// ...fields configuration
		],
	];

	return $meta_boxes;
} );
```

All block parameters will be fetched from template header comment and merged with your fields configuration.

## ⚙️ Configuration
All parameters are optional.

| Parameter                  | Type              | Description                                                  |
| -------------------------- | ----------------- | ------------------------------------------------------------ |
| **dir**                    | (*string*)        | This is a directory within your theme where block templates are located.<br/>**Default:** `'blocks'` |
| **categories**             | (*array*)         | Array of custom block categories passed directly to [`block_categories`](https://developer.wordpress.org/reference/hooks/block_categories/) filter.<br />If only one category will be configured, it will be used as default category for all custom blocks.<br />**Default:** `[]` (empty array) |
| **wrap**                   | (*false\|string*) | Wrapper to each block. If set to false, the block content will be just the template file content.<br/>Works only for ACF due to the differences in block rendering mechanisms.<br/>**Default:** `'<div id="%3$s" class="%2$s">%1$s</div>'` |
| `...$block_creator_params` | -                 | Additional parameters passed to [ACF Block Creator](https://github.com/micropackage/acf-block-creator/) |

### Categories definition

This is how to define the categories array.

```php
	...
	'categories' => [
		[
			'slug'  => 'custom-cat',
			'title' => __( 'Custom Category', 'textdomain' ),
			'icon'  => 'book-alt',
		],
		...
	],
	...
```

### Wrap template

Parameters used in internal `sprintf`:

1. block content from template file, which should be wrapped
2. block classes string
3. unique block id

Example: `'<div id="%3$s" class="%2$s">%1$s</div>'`

## 📦 About the Micropackage project

Micropackages - as the name suggests - are micro packages with a tiny bit of reusable code, helpful particularly in WordPress development.

The aim is to have multiple packages which can be put together to create something bigger by defining only the structure.

Micropackages are maintained by [BracketSpace](https://bracketspace.com).

## 📖 Changelog

[See the changelog file](./CHANGELOG.md).

## 📃 License

GNU General Public License (GPL) v3.0. See the [LICENSE](./LICENSE) file for more information.

## © Credits

The Loader engine is based on the [palmiak](https://github.com/palmiak) [Timber ACF WP Blocks](https://github.com/palmiak/timber-acf-wp-blocks)
