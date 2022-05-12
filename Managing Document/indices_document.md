# Creating & Deleting Indices

## Creating an index (with settings)

```
PUT /products
{
  "settings": {
    "number_of_shards": 2,
    "number_of_replicas": 2
  }
}
```

## Deleting an index

```
DELETE /pages
```

# Indexing documents

## Indexing document with auto generated ID:

```
POST /products/_doc
{
  "name": "Coffee Maker",
  "price": 64,
  "in_stock": 10
}
```

## Indexing document with custom ID:

```
PUT /products/_doc/100
{
  "name": "Toaster",
  "price": 49,
  "in_stock": 4
}
```

# Deleting documents

```
DELETE /products/_doc/100
```

# Replacing documents

```
PUT /products/_doc/100
{
  "name": "Toaster",
  "price": 79,
  "in_stock": 4
}
```

# Retrieving documents by ID

```
GET /products/_doc/100
```

# Updating documents

## Updating an existing field

```
POST /products/_update/100
{
  "doc": {
    "in_stock": 3
  }
}
```

## Adding a new field

```
POST /products/_update/100
{
  "doc": {
    "tags": ["electronics"]
  }
}
```
