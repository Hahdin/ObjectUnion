# ObjectUnion
Create one array of unique objects from multiple arrays of objects, based on specific keys

```js
const ObjectUnionOnMultiKey = (keys, arrayGroup) => {
  const arrays = [...arrayGroup];
  return arrays.reduce((acc, array) => 
    acc.concat(array.filter(item => 
      !acc.filter(ac => 
        eval(keys.map((key, i) =>
            i > 0 ? `&& ac['${key}'] === item['${key}']` : `ac['${key}'] === item['${key}']`
          ).toString().replace(',', ' ')
        )
      ).length)
    )
  )};

console.table(ObjectUnionOnMultiKey(
    ['name', 'age'],
    [[{name: 'Jill', age: 10}, {name: 'Bob', age: 11}],
    [{name: 'Jill', age: 10}, {name: 'Tony', age: 12}],
    [{name: 'Jill', age: 10}, {name: 'Fred', age: 12}],
    [{name: 'Jane', age: 10}, {name: 'Tony', age: 12}],
    [{name: 'Jill', age: 10}, {name: 'Tony', age: 12}],
    [{name: 'Bob', age: 12}, {name: 'George', age: 12}]]
  )
);
/*
┌─────────┬──────────┬─────┐
│ (index) │   name   │ age │
├─────────┼──────────┼─────┤
│    0    │  'Jill'  │ 10  │
│    1    │  'Bob'   │ 11  │
│    2    │  'Tony'  │ 12  │
│    3    │  'Fred'  │ 12  │
│    4    │  'Jane'  │ 10  │
│    5    │  'Bob'   │ 12  │
│    6    │ 'George' │ 12  │
└─────────┴──────────┴─────┘
*/



```
