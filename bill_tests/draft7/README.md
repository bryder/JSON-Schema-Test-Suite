# Validators
* https://jsonschemalint.com/#!/version/draft-07/markup/json

# intellij bug report

https://youtrack.jetbrains.com/issue/IDEA-256729


# What steps will reproduce the issue?

## create a jsonschema and put into a file.

Use this schema - from [the json-schema-test-suite project](https://github.com/json-schema-org/JSON-Schema-Test-Suite/blob/f47003f11dbefd440810d76d4ea7cae5ce336a98/tests/draft7/properties.json#L46)
```json
{
  "description": "properties, patternProperties, additionalProperties interaction",
  "title": "patternProperty invalidates property",
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "properties": {
    "foo": {"type": "array", "maxItems": 3},
    "bar": {"type": "array"}
  },
  "patternProperties": {"f.o": {"type": "array","minItems": 2}},
  "additionalProperties": {"type": "integer"}

}

```
## create the test data and put into a file

Use this test data - from [the test suite](https://github.com/json-schema-org/JSON-Schema-Test-Suite/blob/f47003f11dbefd440810d76d4ea7cae5ce336a98/tests/draft7/properties.json#L67)

```json
{"foo": []}
```

## Apply the schema to the file.

You do this in intelliJ.

# What is the expected result?

The validation should fail.

# What happens instead?

The validation succeeds.

# Notes:

I spent a lot of time trying to figure out why schema I had built in intelliJ which was
validating my data was not validating using any of the schema validators I tried.

I found conflicting explanations of the behaviour when properties and patternProperties match 
on various forums etc on the internet.

Some people think that if a property matches in the "properties" clause  it does not have
to be checked against "patternProperties".

This is incorrect. If the property name matches in "patternProperties" it must ALSO match
that schema as well.


## References

* [draft-07 statement on keyword independence](https://tools.ietf.org/html/draft-handrews-json-schema-validation-01#section-3.1.1)
  The standard states that validation keywords typically operate independently.
  For properties - only  "additionalProperties" is defined in terms of "patternProperties" and "properties".  
* [draft-07 patternProperties](https://tools.ietf.org/html/draft-handrews-json-schema-validation-01#section-6.5.5)
  Nothing in this about interaction with "properties"
* [an issue where this misunderstanding is discussed](https://github.com/ajv-validator/ajv/issues/286)



# blah



What steps will reproduce the issue?

1. Use this schema - from [the json-schema-test-suite project](https://github.com/json-schema-org/JSON-Schema-Test-Suite/blob/f47003f11dbefd440810d76d4ea7cae5ce336a98/tests/draft7/properties.json#L66)

```json
{
  "description": "properties, patternProperties, additionalProperties interaction",
  "title": "patternProperty invalidates property",
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "properties": {
    "foo": {"type": "array", "maxItems": 3},
    "bar": {"type": "array"}
  },
  "patternProperties": {"f.o": {"minItems": 2}},
  "additionalProperties": {"type": "integer"}

}

```
2.

Use this test data - from [the test suite](https://github.com/json-schema-org/JSON-Schema-Test-Suite/blob/f47003f11dbefd440810d76d4ea7cae5ce336a98/tests/draft7/properties.json#L67)
```json
{"foo": []}
```

3.

Apply the schema to the test data

What is the expected result?

The validation should fail.

What happens instead?

The validation succeeds.

Notes:

I spent a lot of time trying to figure a schema I had built in intelliJ was not validating using any of the schema validators I tried.

I found conflicting explanations of the behaviour when properties and patternProperties match.

Some people think that if a property matches in the properties clause  it does not have to be checked against  patternProperties.

This is incorrect. If the property name matches in patternProperties it must ALSO match that test as well.

