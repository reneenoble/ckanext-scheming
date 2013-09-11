ckanext-customschema
====================

This extension provides a way to configure and share
CKAN schemas using a JSON schema description. Custom
validators and template snippets for editing are also
supported.

The schemas used are configured with a configuration option
similar to `licenses_group_url`, e.g.:

```ini
customschema.schema_urls = http://example.com/spatialx_schema.json
```

Paths relative to the configuration file may also be used:

```ini
customschema.schema_urls = ../ckanext/spatialx/customschema.json
```

Example JSON schema description
-------------------------------

```json
{
  "dataset_type": "com.example.spatialx",
  "about_url": "http://example.com/the-spatialx-schema",
  "dataset_fields": [
    {
      "field_name": "title",
      "label": (language-text),
      "form_snippet": "large_text.html",
      "validators": "if_empty_same_as(name)"
    },
    {
      "field_name": "name",
      "label": (language-text),
      "form_snippet": "autofill_from_title.html",
      "validators": "not_empty name_validator package_name_validator"
    },
    {
      "field_name": "internal_flag",
      "label": (language-text),
      "form_snippet": "choice_selectbox.html",
      "validators": "not_empty",
      "choices": [
        {
          "label": (language-text),
          "value": "X",
        },
        {
          "label": (language-text),
          "value": "Y",
        }
      ]
    },
    {
      "field_name": "internal_categories",
      "label": (language-text),
      "form_snippet": "multiple_choice_checkboxes.html",
      "validators": "ignore_empty",
      "tag_vocabulary": "spatialx_categories",
      "choices": [
        {
          "label": (language-text),
          "value": "P",
        },
        {
          "label": (language-text),
          "value": "Q",
        },
        {
          "label": (language-text),
          "value": "R",
        }
      ]
    },
    {
      "field_name": "spatial",
      "label": (language-text),
      "form_snippet": "map_bbox_selection.html",
      "validators": "geojson_validator"
    }
  ]
}
```

In the example above `(language-text)` may be a plain string or an
object containing different language versions:

```json
{
  "eng": "Title",
  "fra": "Titre"
}
```

When using a plain string translations will be looked up with gettext.


dataset_type
------------

`dataset_type` is the "type" field stored in the dataset, which
determines the schema used.
This string should be unique to make sharing your schema easier,
use of a domain name in reverse order at the beginning of this
string is encouraged.


about_url
---------

A Link to human-readable information about this schema may be
provided in this field.


dataset_fields
--------------

Fields are specified in this list in the order you would like them
to appear in the dataset editing form.


### field_name

The `field_name` value is the name of an existing CKAN dataset field
or a new new extra or keyword vocabulary field. Existing field names
include:

* `name` - the URI for the dataset
* `title`
* `notes` - the dataset description
* `author`
* `author_email`
* `maintainer`
* `maintainer_email`

New field names should follow the current lowercase_with_underscores
 naming convention. Don't name your field `mySpecialField`, use
 `my_special_field` instead.

This value is available to the form snippet as `field.field_name`.


### label

The `label` value is a human-readable label for this field as
it will appear in the dataset editing form.
This label may be provided in multiple
languages, but only the correct version for the user's language
will be passed to the form snippet as `field.label`.


## form_snippet

The name of the snippet template to use in the dataset editing
form for this field. A number of snippets are provided with this
extension, but you may also provide your own by creating templates
under `customschema/snippets/` in a template directory in your
own extension.

This snippet is passed the `field` dict containing all the keys and
values in this `dataset_field` record, including any additional ones
you added to your that aren't handled by this extension.


This extension includes the following snippets:

* text.html - a simple text field for free-form text or numbers
* large_text.html - a larger text field, typically used for the title
* choice_selectbox.html - a drop-down list for single choice fields
* ... FIXME: complete this



