# Changelog
All notable changes to this project will be documented in this file.

## 1.0.5

* [Fixed] Deprecated filter name being used
* [Fixed] Falsey values being casted to `true`, thanks to @dafydd-orphans

## 1.0.4

* [Added] Possibility to load blocks from subfolders (block-name/template.php instead of the default block-name.php)
* [Fixed] Error with invalid array key if file didn't have a block header
* [Fixed] Hardcoded default blocks path
* [Fixed] Missing Filesystem dependency, thanks to @joshuafredrickson

## 1.0.3

* [Added] Filters for config
* [Added] Possibility to load block from multiple locations

## 1.0.2

* [Fixed] Invalid variable name

## 1.0.1

* [Fixed] Block slug is nw properly used as html id attribute if no ACF field specified.
* [Added] html_anchor field added to the top of the list of fields which can be used as html id attribute.

## 1.0.0

Initial release
