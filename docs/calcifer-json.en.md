# About the proprietary JSON implementation.

To update data in Firestore, we use json like representation which is implemented in Calcifer.

It supports not only general json values, but also Firestore Document specific types such as `timestamp` `geopoint` `reference`.   
You can write the following to save it

```js
[
  "foo",
  "bar",
  {
    "place": geo"-21,-65",
    "created_at": ts"2020-08-26T00:00:00.000+09:00",
    "user_ref": ref"user/1",
    "author": null,
  },
]
```

Or you can use it by itself.

```js
"foo"
```

and

```js
ts"2020-08-26T00:00:00.000+09:00"
```

etc.

----------

The notation for all types is listed below, and instructions on how to delete a Field are given at the end.

## string ("foo")

String, can be used as value in array or map.

### Notation.

Enclose it in `""`.   
Enclosing it in `'` (single quotes) will result in an error.

### Example.

To set as a single value

```js
"foo"
```

When set as an array or map value

```js
{ a: "aaa" }
[ "aaa", "bbb"]
```

## number (123)

A number, which can also be used as an array or map value.

### Notation.

Write the number as is.   
You don't need to enclose it in `` or `"`.

### Example.

To set a single value

```js
123
```

To set it as an array or map value

```js
{ a: 111 }
[ 111, 222 ]
```

## boolean (true/false)

The value of true or false, can be used as value in array or map.

### Notation.

Write `true` / `false`.   
You don't need to enclose it in `` or `"`.   
It is case sensitive, so `True`, `TRUE`, etc. will result in an error.

### Example.

To set as a single value

```js
true
false
```

When set as an array or map value

```js
{ a: true }
[ true, false ]
```

## null (null)

A null value, which can also be used as an array or map value.

### Notation.

Write `null`.  
You don't need to enclose it in `` or `"`.   
It is case sensitive, so `Null`, `NULL`, etc. will result in an error.

### Example.

To set as a single value

```js
null
```

When set as an array or map value

```js
{ a: null }
[ null, null ]
```

## timestamp (ts"2020-05-22T22:01:51.322+09:00")

A timestamp, which can be used as a value in an array or map.

### Notation.

Enclose in `ts""`.   
It is case sensitive, so `Ts""`, `TS""`, etc. will result in an error.   
If you put a space between `ts` and `"`, it will be an error.  
The input value can be an `int` or a `string` as long as it can be passed as an argument to javascript's `new Date()`.

### Example.

To set it as a single value

```js
ts"2020-05-22T22:01:51.322+09:00"
ts"2020-05-22T22:01:51.322Z"
ts"1590184911322"
```

When set as an array or map value

```js
{ a: ts"2020-05-22T22:01:51.322+09:00" }
[ ts"2020-05-22T22:01:51.322+09:00", ts"1590184911322" ]
```

## geopoint (geo"35.6580939,139.7413553")

Geolocation information, can be used as value of array or map.

### Notation.

Enclose the value in `geo""` and separate it with `,`. It is written as `geo"latitude,longitude"`.  
Because it is case sensitive, `Geo""`, `GEO""`, etc. will result in an error.   
If you put a space between `geo` and `"`, an error will occur.  
There is a range restriction on the values you can enter, `-90 <= latitude <= 90` and `-180 <= longitude <= 180`.

### Example.

To set as a single value

```js
geo"35.6580939,139.7413553"
geo"-48.834,-102.3475"
```

When set as an array or map value

```js
{ a: geo"35.6580939,139.7413553" }
[ geo"35.6580939,139.7413553", geo"35.6580939,139.7413553" ]
```

## reference (ref"users/2HfFxzsMFieaf835d")

A reference to another document, which can be used as a value in an array or map.

### Notation

Enclose the path to the target document in `ref""`.  
It is case sensitive, so `Ref""`, `REF""`, etc. will result in an error.   
If you put a space between `ref` and `"`, an error will occur.   

Because of the restrictions on document IDs which according to the [official Firebase documentation](https://firebase.google.com/docs/firestore/quotas), You can not use `/` for the document ID (actually you can use them, but it will be recognized as a separate subcollection), `.`, `..` and `__. *__`.

### Example.

To set as a single value

```js
ref"users/38aflkj3hb34fa3fc3"
ref"posts/8fq3fktaf8fh38vb36a11/comments/b4yjja91ddat36a"
```

When set as an array or map value

```js
{ a: ref"users/111" }
[ ref"users/111", ref"users/222" ]
```


## array ([1,2,3])

Array, can be used as value for array or map.

### Notation.

Enclose in `[]`.   
Up to one trailing `,` is ignored, but more than one will result in an error (`[1,2,3,]` => OK, `[1,2,3,,]` => NG).   
You can mix and match any values.

### Example

```js
[ 1,2,3, "a", "b", "c",ref"users/111",ref"users/222" ]
```

## map ({a: "aaa", b: "bbb"})

Map, can be used as value for array or map.

### Notation.

Enclose in `{}`.   
The `,` at the end will be ignored if there is more than one (`{a: "a",}` => OK, `{a: "a",,}` => NG).   
You can mix and match any values.   
Enclosing the key name with `__` (two underscores) will result in an error.   
If a key name contains some symbols or numbers, it must be enclosed in `""` or an error will occur.

### Example

```js
{ a: "abc", "!b": "def" }
```

## About deleting a Field

If you save a field with a blank value, the entire field will be deleted. After saving, there will be no key or value. If you want to use empty or null characters for the value, use `""` (empty) or `null` respectively.

Translated with www.DeepL.com/Translator (free version)
