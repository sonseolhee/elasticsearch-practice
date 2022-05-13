# Using the Analyze API

## Analyzing a string with the `standard` analyzer

```
POST /_analyze
{
  "text": "2 guys walk into   a bar, but the third... DUCKS! :-)",
  "analyzer": "standard"
}
```

## Building the equivalent of the `standard` analyzer

```
POST /_analyze
{
  "text": "2 guys walk into   a bar, but the third... DUCKS! :-)",
  "char_filter": [],
  "tokenizer": "standard",
  "filter": ["lowercase"]
}
```

# Updating analyzers

## Add `description` mapping using `my_custom_analyzer`

```
PUT /analyzer_test/_mapping
{
  "properties": {
    "description": {
      "type": "text",
      "analyzer": "my_custom_analyzer"
    }
  }
}
```

## Index a test document

```
POST /analyzer_test/_doc
{
  "description": "Is that Peter's cute-looking dog?"
}
```

## Search query using `keyword` analyzer

```
GET /analyzer_test/_search
{
  "query": {
    "match": {
      "description": {
        "query": "that",
        "analyzer": "keyword"
      }
    }
  }
}
```

## Close `analyzer_test` index

```
POST /analyzer_test/_close
```

## Update `my_custom_analyzer` (remove `stop` token filter)

```
PUT /analyzer_test/_settings
{
  "analysis": {
    "analyzer": {
      "my_custom_analyzer": {
        "type": "custom",
        "tokenizer": "standard",
        "char_filter": ["html_strip"],
        "filter": [
          "lowercase",
          "asciifolding"
        ]
      }
    }
  }
}
```

## Open `analyzer_test` index

```
POST /analyzer_test/_open
```

## Retrieve index settings

```
GET /analyzer_test/_settings
```

## Reindex documents

```
POST /analyzer_test/_update_by_query?conflicts=proceed
```

# How the `keyword` data type works

## Testing the `keyword` analyzer

```
POST /_analyze
{
  "text": "2 guys walk into   a bar, but the third... DUCKS! :-)",
  "analyzer": "keyword"
}
```

# Creating custom analyzers

## Remove HTML tags and convert HTML entities

```
POST /_analyze
{
  "char_filter": ["html_strip"],
  "text": "I&apos;m in a <em>good</em> mood&nbsp;-&nbsp;and I <strong>love</strong> açaí!"
}
```

## Add the `standard` tokenizer

```
POST /_analyze
{
  "char_filter": ["html_strip"],
  "tokenizer": "standard",
  "text": "I&apos;m in a <em>good</em> mood&nbsp;-&nbsp;and I <strong>love</strong> açaí!"
}
```

## Add the `lowercase` token filter

```
POST /_analyze
{
  "char_filter": ["html_strip"],
  "tokenizer": "standard",
  "filter": [
    "lowercase"
  ],
  "text": "I&apos;m in a <em>good</em> mood&nbsp;-&nbsp;and I <strong>love</strong> açaí!"
}
```

## Add the `stop` token filter

This removes English stop words by default.

```
POST /_analyze
{
  "char_filter": ["html_strip"],
  "tokenizer": "standard",
  "filter": [
    "lowercase",
    "stop"
  ],
  "text": "I&apos;m in a <em>good</em> mood&nbsp;-&nbsp;and I <strong>love</strong> açaí!"
}
```

## Add the `asciifolding` token filter

Convert characters to their ASCII equivalent.

```
POST /_analyze
{
  "char_filter": ["html_strip"],
  "tokenizer": "standard",
  "filter": [
    "lowercase",
    "stop",
    "asciifolding"
  ],
  "text": "I&apos;m in a <em>good</em> mood&nbsp;-&nbsp;and I <strong>love</strong> açaí!"
}
```

## Create a custom analyzer named `my_custom_analyzer`

```
PUT /analyzer_test
{
  "settings": {
    "analysis": {
      "analyzer": {
        "my_custom_analyzer": {
          "type": "custom",
          "char_filter": ["html_strip"],
          "tokenizer": "standard",
          "filter": [
            "lowercase",
            "stop",
            "asciifolding"
          ]
        }
      }
    }
  }
}
```

## Configure the analyzer to remove Danish stop words

To run this query, change the index name to avoid a conflict. Remember to remove the comments. :wink:

```
PUT /analyzer_test
{
  "settings": {
    "analysis": {
      "filter": {
        "danish_stop": {
          "type": "stop",
          "stopwords": "_danish_"
        }
      },
      "char_filter": {
        # Add character filters here
      },
      "tokenizer": {
        # Add tokenizers here
      },
      "analyzer": {
        "my_custom_analyzer": {
          "type": "custom",
          "char_filter": ["html_strip"],
          "tokenizer": "standard",
          "filter": [
            "lowercase",
            "danish_stop",
            "asciifolding"
          ]
        }
      }
    }
  }
}
```

## Test the custom analyzer

```
POST /analyzer_test/_analyze
{
  "analyzer": "my_custom_analyzer",
  "text": "I&apos;m in a <em>good</em> mood&nbsp;-&nbsp;and I <strong>love</strong> açaí!"
}
```

# Adding analyzers to existing indices

## Close `analyzer_test` index

```
POST /analyzer_test/_close
```

## Add new analyzer

```
PUT /analyzer_test/_settings
{
  "analysis": {
    "analyzer": {
      "my_second_analyzer": {
        "type": "custom",
        "tokenizer": "standard",
        "char_filter": ["html_strip"],
        "filter": [
          "lowercase",
          "stop",
          "asciifolding"
        ]
      }
    }
  }
}
```

## Open `analyzer_test` index

```
POST /analyzer_test/_open
```

## Retrieve index settings

```
GET /analyzer_test/_settings
```
