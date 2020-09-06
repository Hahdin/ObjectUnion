# ObjectUnion
Create one array of unique objects from multiple arrays of objects, based on specific keys

```js
const ObjectUnionMultiKey = (keys, ...arrays) => 
  arrays.reduce((acc, array) => 
    acc.concat(array.filter((item) => 
      !acc.filter(ac => 
        eval(keys.map((key, i) =>
            i > 0 ? `&& ac['${key}'] === item['${key}']` : `ac['${key}'] === item['${key}']`
          ).toString().replace(',', ' ')
        )
      ).length)
    )
  );

console.table(ObjectUnionMultiKey(
    ['name', 'age'],
    [{name: 'Jill', age: 10}, {name: 'Bob', age: 11}],
    [{name: 'Jill', age: 10}, {name: 'Tony', age: 12}],
    [{name: 'Bob', age: 12}, {name: 'Tony', age: 12}]
  )
);
/*
┌─────────┬────────┬─────┐
│ (index) │  name  │ age │
├─────────┼────────┼─────┤
│    0    │ 'Jill' │ 10  │
│    1    │ 'Bob'  │ 11  │
│    2    │ 'Tony' │ 12  │
│    3    │ 'Bob'  │ 12  │
└─────────┴────────┴─────┘
*/


```
