# Linked Data Platform Query Language


- Queries can filter by triples in LDP resources
- Queries respond with the bodies of LDP resources, paginated
- Mutations can alter triples in LDP resources
- Mutations respond with success/error message and/or new resources?


An endpoint where you can query by one or more triple patterns (like SPARQL) and receive LDP Resources back.

Should you be able to specify only certain fields you want back?

TPF:
```
?subject ?predicate ?object.
```

SPARQL:
```
SELECT ?s WHERE {
  ?s ?p ?o.
  ?s ?p2 'blah'.
}
```

Should we use a subset of sparql or of graphql? or a superset of TPF?

`https://skl.standard.storage/ldpql/query={query}&variables={vars}&context={context}`

Really we want SPARQL without SELECT or CONSTRUCT

What about a list of TPFs:
```
[
  ?s ?p ?o,
  ?s2 ?p2 ?o2,
]
```

with the ability to include variables?

```
{
  selection: "subject"
  patterns: [
    ["?subject", "foaf:name", "$name"],
    ["skldata:project1", "projectns:hasPerson", "?subject"]
  ],
  variables: {
    name: "Adler"
  },
  context: {
    foaf: ""
  }
}
```

probably need operations like gt, gte, lt, lte, startsWith, endsWith, includes. Could do the string ones with regexps. Also need AND, OR, NOT

This leads me back to my work to do something like prisma where prisma client queries and mutations map to sql and other query languages.

Still need to expand on ldp to be able to do more complex queries on solid pods - but sparql?

If we choose sparql, we need to be restrictive according to access controls, so ideally the queries are not super complex. Or anything used as a subject has to be run through an access control sub query... Or this is handled at the translation layer to other query languages like DQL.


---

Get the store name, product uri, and price of any product that is less than $30.5
```
PREFIX  dc:  <http://purl.org/dc/elements/1.1/>
PREFIX  ns:  <http://example.org/ns#>
SELECT  ?storeName ?product ?price
WHERE   { ?product ns:price ?price .
            FILTER (?price < 30.5)
          ?store rdf:type "store" .
          ?store hasProduct ?product .
          ?store dc:name ?storeName .
        }
```

Returns
```
walmart https://example.com/product1 30.0
REI https://example.com/product2 10.7
```


LDP example:

`GET ?product` where `?product` is a uri

Returns
```
{
  id: https://example.com/product1,
  type: Product,
  price: 30.0
}
```

LDPQL example:
```
PREFIX  dc:  <http://purl.org/dc/elements/1.1/>
PREFIX  ns:  <http://example.org/ns#>
SELECT  ?product
WHERE   { ?product ns:price ?price .
            FILTER (?price < 30.5)
          ?store rdf:type "store" .
          ?store hasProduct ?product .
          ?store dc:name ?storeName .
        }
```
Returns:
```
[
  {
    id: https://example.com/product1,
    type: Product,
    price: 30.0
    aefionb: soisnbd,
    sdfioasbdf: sdfoisnd,
  },
  {
    id: https://example.com/product2,
    type: Product,
    price: 10.7
  }
]
```

```
Product
Store
ProductStoreAssociation
  product <https://example.com/product1>
  store: <https://example.com/store1>
  price: 10.4
  shelf: 3

Movie
Store
StoreListing
  store
  movie
  some other metadata about the listing
```

TPF example:

1. SKQL client library/API translates SKQL function call into one or more TPF queries.
2. TPF queries sent to solid server
3. TPF queries translated into and executed on persistence using persistence QL
4. TPF query results sent back to SKQL client
5. SKQL client uses LDP to send request for each TPF result (or merge of many TPF results)

SPARQL example 1 (auth added by solid server into sparql query):

1. SKQL client library/API translates SKQL function call into a SPARQL query
2. SPARQL query sent to Solid server or equivalent
3. Solid server rewrites the SPARQL query to add authorization/access control logic to SPARQL query
4. Translation later translates SPARQL query to persistence QL query

SPARQL example 2 (auth added by persistence translation):

1. SKQL client library/API translates SKQL function call into a SPARQL query
2. SPARQL query sent to Solid server or equivalent
3. Translation layer:
    - translates SPARQL query to persistence QL query
    - add authorization/access control to persistence QL query as the SPARQL query is translated


---

Why?


1. Library of schemas, mappings, integrations, interfaces - people can upload and contribute to.
2. Can get a copy of core schemas (or a subset) which you can add to/customize
3.
