---
title: BigQuery Levenshtien Distance 
description: A UDF to find edit distance for in a BigQuery table
slug: edit-distance-bigquery
date: 20-07-2022 00:00:00+0000
image: cover.webp
categories:
    - SQL
tags:
    - js
    - big-query
---

Edit distance is essentially a metric which is about comparing the number of changes required to turn one string into another. 

"Whisky" — "Whiskey": Levenshtein Distance, 1. An ‘e’ is added.

Using this function on a large text column often tens to be pretty slow. So we write a UDF(user-defined-function) in Javascript and use BigQuery to execute this Javascript function over the whole column. (The syntax highlighting breaks badly cause this is a weird combination of SQL and JS.)

```sql
CREATE OR REPLACE FUNCTION
  validation_data.LevenshteinDistance(in_a string,
    in_b string)
  RETURNS INT64
  LANGUAGE js AS 
"""
/*
 * Data Quality Function - Fuzzy Matching
 * dq_fm_LevenshteinDistance
 * Based off of https://gist.github.com/andrei-m/982927
 * input: Two strings to compare the edit distance of.
 * returns: Integer of the edit distance.
 */
var a = in_a.toLowerCase();
var b = in_b.toLowerCase();

if (a.length == 0) return b.length;
if (b.length == 0) return a.length;
var matrix = [];
// increment along the first column of each row
var i;
for (i = 0; i <= b.length; i++) {
    matrix[i] = [i];
}
// increment each column in the first row
var j;
for (j = 0; j <= a.length; j++) {
    matrix[0][j] = j;
}
// Fill in the rest of the matrix
for (i = 1; i <= b.length; i++) {
    for (j = 1; j <= a.length; j++) {
        if (b.charAt(i - 1) == a.charAt(j - 1)) {
            matrix[i][j] = matrix[i - 1][j - 1];
        } else {
            matrix[i][j] =
                Math.min(matrix[i - 1][j - 1] + 1, // substitution
                    Math.min(matrix[i][j - 1] + 1, // insertion
                        matrix[i - 1][j] + 1)); // deletion
        }
    }
}
return matrix[b.length][a.length];
"""
```

This function was creatively extracted from a blog [here](https://medium.com/google-cloud/a-journey-into-bigquery-fuzzy-matching-2-of-1-more-soundex-and-levenshtein-distance-e64b25ea4ec7). Please read it to get more info on how this exactly works