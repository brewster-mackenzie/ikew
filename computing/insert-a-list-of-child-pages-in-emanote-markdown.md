# Insert a list of child pages in Emanote markdown

#markdown #emanote #notes #selfhosting

-----

To insert a list of the child pages of the current page using
Emanote requires an Emanote query

````markdown
```query
path:./*
```
````

The value of `path` can be modified to filter specific pages and paths
