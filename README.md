# ObjectPath
ObjectPath is a utility class which allows you to retreive/set object path properties on dynamic and typed objects.

## Paths
Let's say you have an object, it could be a typed object, or dynamic object or even a dictionary. 

ObjectPath allows you to dynamically navigate the object hierachy using a string path expression.

For example:
```csharp
if (ObjectPath.TryGetValue<int>("x.y.z", out var val))
{
   // use val is a int
}
```

## Methods

| Methods| Description |
|------|------|
| HasValue(obj, path) | returns true if the path can be evaluated against obj |
| GetPathValue&lt;T&gt;(obj, path) | returns the result of evaluating path as T if it exists, or throws if the path doesn't |
| TryGetValueValue&lt;T&gt;(obj, path, out T) | retruns the result of valuating the path if it exists |
| SetPathValue(obj, path, val) | sets the path to a given value |
| RemovePathValue(obj, path) | removes the leaf of path (for dynamic objects) or resets it to default for typed objects |
| GetProperties(obj) | enumerates public properties for objects/dictionaries/dynamic objects|
| ForEachProperty(obj, action) | calls action with each public properties for objects/dictionaries/dynamic objects|
| ContainsProperty(obj, prop) | shallow inspection to see if current object has a property/value named prop |
| Clone&lt;T&gt;(obj) | creates a deep clone of public properties and all of it's children| 
| Merge&lt;T&gt;(startObj, overlayObj) | creates a new object which is clone of startObj with any properties defined on overlayObj merged over it |
| Assign&lt;T&gt;(startObj, overlayObj) | creates a new object which is clone of startObj with any properties defined on overlayObj merged over it |
| MapValueTo&lt;T&gt;(obj) | coercices an object to a given type. |

## Examples 

```csharp
  var dyn = new { X = 13, Y = new {Z = "15" } };

  ObjectPath.GetPathValue(dyn, "X"); ==> 13
  ObjectPath.GetPathValue(dyn, "X.Y.Z"); ==> "15"
  ObjectPath.GetPathValue<int>(dyn, "X.Y.Z"); ==> 15
  ObjectPath.HasPathValue(dyn, "X.Y") ==> true
  ObjectPath.HasPathValue(dyn, "X.FOO") ==> false
  
  // you can set path values without having to check sub parts, it will use JTokens to build the path.
  ObjectPath.SetPathValue(dyn, "X.FOO.Zoo", "hello");
  
  ObjectPath.HasPathValue(dyn, "X.FOO") ==> true
  ObjectPath.GetPathValue(dyn, "X.FOO.Zoo") ==> "hello"
  etc
```

