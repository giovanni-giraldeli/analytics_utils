- ### Basic syntax
  ```sql
  WITH cte1 AS (
    ... --logic for cte1
  )
  , cte2 AS (
    ... --logic for cte2
  )
  SELECT
    ... --results from the query
  FROM
    [object_name_1] --table or view
  JOIN
    [object_name_2] --table or view
      ON ... --join conditions (keys)
  WHERE
    ... --conditions to filter columns
  GROUP BY
    ... --columns to be grouped
  HAVING
    ... --conditions to filter aggregations
  ORDER BY
    ... --columns from SELECT statement
  LIMIT
    ... --limiting the results to only 100 rows
  ```

- ### Execution order
  `FROM/JOIN >> WHERE >> GROUP BY >> HAVING >> SELECT >> ORDER BY >> LIMIT`
  
  This is a topic that can lead to some confusion, specially when trying to optimize your query time.<br>
  Always have in mind that the least data that you need to read and retrieve, the faster _- and cheaper -_ your query will run.
  
  SQL query execution order evaluates your data sources first (`FROM/JOIN`) and the next step already filters the data to a narrow scope (`WHERE`).
  That said, it's important to note that only this smaller chunk will be considered for the `GROUP BY` operations, possibly reducing compute for this operation.

  Note that `HAVING` comes right after the GROUP BY, and this is the reason why you can't filter aggregations on the WHERE clause.

  We are done filtering and aggregating, so it's time to get some results with the `SELECT` coming in to stage, while `ORDER BY` and `LIMIT` help you to organize how you'll display the data by _- well -_
  ordering and limiting the result set to be displayed.

  Try using some tips here before blaming and swearing **[add your data viz tool here]** that your dashboards are taking ages to load.

  I hope that this map helps you to better optimize your queries, narrowing the scope and the data size to be analyzed.<br>
  Some other techniques can help better optimizing the performance, but they come previous to querying the data, such as proper data modeling, indexing columns, and partitioning tables.
