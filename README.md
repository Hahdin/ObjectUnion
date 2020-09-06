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

# Object Union with grouped fields

```js
const ObjectUnionOnMultiKeysGroup = (keys, groups, [...arrays]) => {
  return arrays.reduce((acc, array) => 
    acc.concat(array.filter(item => 
      !acc.filter(ac => {
        const found = eval(keys.map((key, i) =>
            i > 0 ? `&& ac['${key}'] === item['${key}']` : `ac['${key}'] === item['${key}']`
          ).toString().replace(',', ' ')
        );
        groups.forEach(group => ac[group] += found ? `, ${item[group]}` : '');
        return found;
      }).length
    ))
  )};

  const ObjectKeyConcat = (array, keys) => array.map(item => {
    item.group = '';
    keys.forEach((key, i) => {
      item.group += i > 0 ? `, ${key.toUpperCase()}: ${item[key]}` : `${key.toUpperCase()}: ${item[key]}`;
    })
    return item;
  });
  
  
  const ar = ObjectKeyConcat(
    ObjectUnionOnMultiKeysGroup(
      ['name', 'age'],
      ['day', 'hour'],
      [[{name: 'Jill', age: 10, day: 'Mon', hour: '1'}, {name: 'Bob', age: 11, day: 'Mon', hour: '1'}],
      [{name: 'Jill', age: 10, day: 'Tue', hour: '2'}, {name: 'Tony', age: 12, day: 'Tue', hour: '2'}],
      [{name: 'Jill', age: 10, day: 'Wed', hour: '3'}, {name: 'Fred', age: 12, day: 'Wed', hour: '3'}],
      [{name: 'Jane', age: 10, day: 'Thur', hour: '1'}, {name: 'Tony', age: 12, day: 'Thur', hour: '1'}],
      [{name: 'Jill', age: 10, day: 'Fri', hour: '2'}, {name: 'Tony', age: 12, day: 'Fri', hour: '2'}],
      [{name: 'Bob', age: 12, day: 'Sat', hour: '3'}, {name: 'George', age: 12, day: 'Sat', hour: '3'}]]
    ),
    ['name','age', 'day']
  );


console.table(ar);

/*
─────────┬──────────┬─────┬──────────────────────┬──────────────┬────────────────────────────────────────────────┐
│ (index) │   name   │ age │         day          │     hour     │                     group                      │
├─────────┼──────────┼─────┼──────────────────────┼──────────────┼────────────────────────────────────────────────┤
│    0    │  'Jill'  │ 10  │ 'Mon, Tue, Wed, Fri' │ '1, 2, 3, 2' │ 'NAME: Jill, AGE: 10, DAY: Mon, Tue, Wed, Fri' │
│    1    │  'Bob'   │ 11  │        'Mon'         │     '1'      │         'NAME: Bob, AGE: 11, DAY: Mon'         │
│    2    │  'Tony'  │ 12  │   'Tue, Thur, Fri'   │  '2, 1, 2'   │   'NAME: Tony, AGE: 12, DAY: Tue, Thur, Fri'   │
│    3    │  'Fred'  │ 12  │        'Wed'         │     '3'      │        'NAME: Fred, AGE: 12, DAY: Wed'         │
│    4    │  'Jane'  │ 10  │        'Thur'        │     '1'      │        'NAME: Jane, AGE: 10, DAY: Thur'        │
│    5    │  'Bob'   │ 12  │        'Sat'         │     '3'      │         'NAME: Bob, AGE: 12, DAY: Sat'         │
│    6    │ 'George' │ 12  │        'Sat'         │     '3'      │       'NAME: George, AGE: 12, DAY: Sat'        │
└─────────┴──────────┴─────┴──────────────────────┴──────────────┴────────────────────────────────────────────────┘
*/

```
