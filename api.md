# API

_This reference guide lists all methods exposed by MST. Contributions like linguistic improvements, adding more details to the descriptions or additional examples are highly appreciated! Please note that the docs are generated from source. Most methods are declared in the_ [_mst-operations.ts_](https://github.com/mobxjs/mobx-state-tree/blob/master/src/core/mst-operations.ts) _file._

## addDisposer

Use this utility to register a function that should be called whenever the targeted state tree node is destroyed. This is a useful alternative to managing cleanup methods yourself using the `beforeDestroy` hook.

**Parameters**

* `target` **IStateTreeNode**
* `disposer`

**Examples**

```javascript
const Todo = types.model({
  title: types.string
}).actions(self => ({
  afterCreate() {
    const autoSaveDisposer = reaction(
      () => getSnapshot(self),
      snapshot => sendSnapshotToServerSomehow(snapshot)
    )
    // stop sending updates to server if this
    // instance is destroyed
    addDisposer(self, autoSaveDisposer)
  }
}))
```

## addMiddleware

Middleware can be used to intercept any action is invoked on the subtree where it is attached. If a tree is protected \(by default\), this means that any mutation of the tree will pass through your middleware.

For more details, see the [middleware docs](https://mobx-state-tree.gitbook.io/docs/middleware)

**Parameters**

* `target` **IStateTreeNode**
* `handler`
* `includeHooks`

Returns **IDisposer**

## applyAction

Applies an action or a series of actions in a single MobX transaction. Does not return any value Takes an action description as produced by the `onAction` middleware.

**Parameters**

* `target` [**Object**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)
* `actions` [**Array**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)**&lt;IActionCall&gt;**
* `options` **IActionCallOptions?**

## applyPatch

Applies a JSON-patch to the given model instance or bails out if the patch couldn't be applied See [patches](https://mobx-state-tree.gitbook.io/docs/concepts/patches) for more details.

Can apply a single past, or an array of patches.

**Parameters**

* `target` [**Object**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)
* `patch` **IJsonPatch**

## applySnapshot

Applies a snapshot to a given model instances. Patch and snapshot listeners will be invoked as usual.

**Parameters**

* `target` [**Object**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)
* `snapshot` [**Object**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)

## clone

Returns a deep copy of the given state tree node as new tree. Short hand for `snapshot(x) = getType(x).create(getSnapshot(x))`

_Tip: clone will create a literal copy, including the same identifiers. To modify identifiers etc during cloning, don't use clone but take a snapshot of the tree, modify it, and create new instance_

**Parameters**

* `source` **T**
* `keepEnvironment` **\(**[**boolean**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean) **\| any\)** indicates whether the clone should inherit the same environment \(`true`, the default\), or not have an environment \(`false`\). If an object is passed in as second argument, that will act as the environment for the cloned tree.

Returns **T**

## createActionTrackingMiddleware

Convenience utility to create action based middleware that supports async processes more easily. All hooks are called for both synchronous and asynchronous actions. Except that either `onSuccess` or `onFail` is called

The create middleware tracks the process of an action \(assuming it passes the `filter`\). `onResume` can return any value, which will be passed as second argument to any other hook. This makes it possible to keep state during a process.

See the `atomic` middleware for an example

**Parameters**

* `hooks`

Returns **IMiddlewareHandler**

## decorate

Binds middleware to a specific action

**Parameters**

* `handler` **IMiddlewareHandler**
* `fn`
* `Function` } fn

**Examples**

```javascript
type.actions(self => {
  function takeA____() {
      self.toilet.donate()
      self.wipe()
      self.wipe()
      self.toilet.flush()
  }
  return {
    takeA____: decorate(atomic, takeA____)
  }
})
```

Returns **any** the original function

## destroy

Removes a model element from the state tree, and mark it as end-of-life; the element should not be used anymore

**Parameters**

* `target`

## detach

Removes a model element from the state tree, and let it live on as a new state tree

**Parameters**

* `target`

## escapeJsonPath

escape slashes and backslashes [http://tools.ietf.org/html/rfc6901](http://tools.ietf.org/html/rfc6901)

**Parameters**

* `str`

## flow

See [asynchronous actions](https://mobx-state-tree.gitbook.io/docs/creating-asynchronous-actions).

**Parameters**

* `asyncAction`

Returns [**Promise**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)

## getChildType

Returns the _declared_ type of the given sub property of an object, array or map.

**Parameters**

* `object` **IStateTreeNode**
* `child` [**string**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)

**Examples**

```javascript
const Box = types.model({ x: 0, y: 0 })
const box = Box.create()

console.log(getChildType(box, "x").name) // 'number'
```

Returns **IType&lt;any, any&gt;**

## getEnv

Returns the environment of the current state tree. For more info on environments, see [Dependency injection](https://mobx-state-tree.gitbook.io/docs/concepts/dependency-injection)

Returns an empty environment if the tree wasn't initialized with an environment

**Parameters**

* `target` **IStateTreeNode**

Returns **any**

## getIdentifier

Returns the identifier of the target node.

**Parameters**

* `target` **IStateTreeNode**

Returns **\(**[**string**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) **\| null\)**

## getMembers

Returns a reflection of the node

**Parameters**

* `target` **IStateTreeNode**

Returns **IModelReflectionData**

## getParent

Returns the immediate parent of this object, or throws.

Note that the immediate parent can be either an object, map or array, and doesn't necessarily refer to the parent model

**Parameters**

* `target` [**Object**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)
* `depth` [**number**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number) = 1, how far should we look upward?

Returns **any**

## getParentOfType

Returns the target's parent of a given type, or throws.

**Parameters**

* `target` [**Object**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)
* `type` **IType&lt;any, any&gt;**

Returns **any**

## getPath

Returns the path of the given object in the model tree

**Parameters**

* `target` [**Object**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)

Returns [**string**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)

## getPathParts

Returns the path of the given object as unescaped string array

**Parameters**

* `target` [**Object**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)

Returns [**Array**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)**&lt;**[**string**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)**&gt;**

## getRelativePath

Given two state tree nodes that are part of the same tree, returns the shortest jsonpath needed to navigate from the one to the other

**Parameters**

* `base` **IStateTreeNode**
* `target` **IStateTreeNode**

Returns [**string**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)

## getRoot

Given an object in a model tree, returns the root object of that tree

**Parameters**

* `target` [**Object**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)

Returns **any**

## getSnapshot

Calculates a snapshot from the given model instance. The snapshot will always reflect the latest state but use structural sharing where possible. Doesn't require MobX transactions to be completed.

**Parameters**

* `target` [**Object**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)
* `applyPostProcess` [**boolean**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean) = true, by default the postProcessSnapshot gets applied

Returns **any**

## getType

Returns the _actual_ type of the given tree node. \(Or throws\)

**Parameters**

* `object` **IStateTreeNode**

Returns **IType&lt;S, T&gt;**

## hasParent

Given a model instance, returns `true` if the object has a parent, that is, is part of another object, map or array

**Parameters**

* `target` [**Object**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)
* `depth` [**number**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number) = 1, how far should we look upward?

Returns [**boolean**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)

## hasParentOfType

Given a model instance, returns `true` if the object has a parent of given type, that is, is part of another object, map or array

**Parameters**

* `target` [**Object**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)
* `type` **IType&lt;any, any&gt;**

Returns [**boolean**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)

## Identifier

## IdentifierCache

## isAlive

Returns true if the given state tree node is not killed yet. This means that the node is still a part of a tree, and that `destroy`has not been called. If a node is not alive anymore, the only thing one can do with it is requesting it's last path and snapshot

**Parameters**

* `target` **IStateTreeNode**

Returns [**boolean**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)

## isProtected

Returns true if the object is in protected mode, @see protect

**Parameters**

* `target`

## isRoot

Returns true if the given object is the root of a model tree

**Parameters**

* `target` [**Object**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)

Returns [**boolean**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)

## isStateTreeNode

Returns true if the given value is a node in a state tree. More precisely, that is, if the value is an instance of a `types.model`, `types.array` or `types.map`.

**Parameters**

* `value` **any**

## joinJsonPath

Generates a json-path compliant json path from path parts

**Parameters**

* `path` [**Array**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)**&lt;**[**string**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)**&gt;**

Returns [**string**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)

## ObjectNode

## onAction

Registers a function that will be invoked for each action that is called on the provided model instance, or to any of its children. See [actions](https://mobx-state-tree.gitbook.io/docs/concepts/actions) for more details. onAction events are emitted only for the outermost called action in the stack. Action can also be intercepted by middleware using addMiddleware to change the function call before it will be run.

Not all action arguments might be serializable. For unserializable arguments, a struct like `{ $MST_UNSERIALIZABLE: true, type: "someType" }` will be generated. MST Nodes are considered non-serializable as well \(they could be serialized as there snapshot, but it is uncertain whether an replaying party will be able to handle such a non-instantiated snapshot\). Rather, when using `onAction` middleware, one should consider in passing arguments which are 1: an id, 2: a \(relative\) path, or 3: a snapshot. Instead of a real MST node.

**Parameters**

* `target` **IStateTreeNode**
* `listener`
* `attachAfter` {boolean} \(default false\) fires the listener _after_ the action has executed instead of before.

**Examples**

```text
const Todo = types.model({
  task: types.string
})

const TodoStore = types.model({
  todos: types.array(Todo)
}).actions(self => ({
  add(todo) {
    self.todos.push(todo);
  }
}))

const s = TodoStore.create({ todos: [] })

let disposer = onAction(s, (call) => {
  console.log(call);
})

s.add({ task: "Grab a coffee" })
// Logs: { name: "add", path: "", args: [{ task: "Grab a coffee" }] }
```

Returns **IDisposer**

## onPatch

Registers a function that will be invoked for each mutation that is applied to the provided model instance, or to any of its children. See [patches](https://mobx-state-tree.gitbook.io/docs/concepts/patches) for more details. onPatch events are emitted immediately and will not await the end of a transaction. Patches can be used to deep observe a model tree.

**Parameters**

* `target` [**Object**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object) the model instance from which to receive patches
* `callback`
* `boolean` **includeOldValue** if oldValue is included in the patches, they can be inverted. However patches will become much bigger and might not be suitable for efficient transport

Returns **IDisposer** function to remove the listener

## onSnapshot

Registers a function that is invoked whenever a new snapshot for the given model instance is available. The listener will only be fire at the and of the current MobX \(trans\)action. See [snapshots](https://mobx-state-tree.gitbook.io/docs/concepts/snapshots) for more details.

**Parameters**

* `target` [**Object**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)
* `callback`

Returns **IDisposer**

## process

**Parameters**

* `asyncAction`

Returns [**Promise**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)

**Meta**

* **deprecated**: has been renamed to `flow()`. See [https://github.com/mobxjs/mobx-state-tree/issues/399](https://github.com/mobxjs/mobx-state-tree/issues/399) for more information. Note that the middleware event types starting with `process` now start with `flow`.

## protect

The inverse of `unprotect`

**Parameters**

* `target` **IStateTreeNode**

## recordActions

Small abstraction around `onAction` and `applyAction`, attaches an action listener to a tree and records all the actions emitted. Returns an recorder object with the following signature:

**Parameters**

* `subject` **IStateTreeNode**

**Examples**

```javascript
export interface IActionRecorder {
     // the recorded actions
     actions: ISerializedActionCall[]
     // stop recording actions
     stop(): any
     // apply all the recorded actions on the given object
     replay(target: IStateTreeNode): any
}
```

Returns **IPatchRecorder**

## recordPatches

Small abstraction around `onPatch` and `applyPatch`, attaches a patch listener to a tree and records all the patches. Returns an recorder object with the following signature:

**Parameters**

* `subject` **IStateTreeNode**

**Examples**

```javascript
export interface IPatchRecorder {
     // the recorded patches
     patches: IJsonPatch[]
     // the inverse of the recorded patches
     inversePatches: IJsonPatch[]
     // stop recording patches
     stop(target?: IStateTreeNode): any
     // resume recording patches
     resume()
     // apply all the recorded patches on the given target (the original subject if omitted)
     replay(target?: IStateTreeNode): any
     // reverse apply the recorded patches on the given target  (the original subject if omitted)
     // stops the recorder if not already stopped
     undo(): void
}
```

Returns **IPatchRecorder**

## resolveIdentifier

Resolves a model instance given a root target, the type and the identifier you are searching for. Returns undefined if no value can be found.

**Parameters**

* `type` **IType&lt;any, any&gt;**
* `target` **IStateTreeNode**
* `identifier` **\(**[**string**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) **\|** [**number**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)**\)**

Returns **any**

## resolvePath

Resolves a path relatively to a given object. Returns undefined if no value can be found.

**Parameters**

* `target` [**Object**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)
* `path` [**string**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) escaped json path

Returns **any**

## ScalarNode

## splitJsonPath

Splits and decodes a json path into several parts

**Parameters**

* `path` [**string**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)

Returns [**Array**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)**&lt;**[**string**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)**&gt;**

## StoredReference

## tryResolve

**Parameters**

* `target` [**Object**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)
* `path` [**string**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)

Returns **any**

## typecheck

Run's the typechecker on the given type. Throws if the given value is not according the provided type specification. Use this if you need typechecks even in a production build \(by default all automatic runtime type checks will be skipped in production builds\)

**Parameters**

* `type` **IType&lt;any, any&gt;**
* `value` **any**

## types.array

Creates an index based collection type who's children are all of a uniform declared type.

This type will always produce [observable arrays](https://mobx.js.org/refguide/array.html)

**Parameters**

* `subtype` **IType&lt;S, T&gt;**

**Examples**

```javascript
const Todo = types.model({
  task: types.string
})

const TodoStore = types.model({
  todos: types.array(Todo)
})

const s = TodoStore.create({ todos: [] })
unprotect(s) // needed to allow modifying outside of an action
s.todos.push({ task: "Grab coffee" })
console.log(s.todos[0]) // prints: "Grab coffee"
```

Returns **IComplexType&lt;**[**Array**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)**&lt;S&gt;, IObservableArray&lt;T&gt;&gt;**

## types.boolean

Creates a type that can only contain a boolean value. This type is used for boolean values by default

**Examples**

```javascript
const Thing = types.model({
  isCool: types.boolean,
  isAwesome: false
})
```

## types.compose

Composes a new model from one or more existing model types. This method can be invoked in two forms: Given 2 or more model types, the types are composed into a new Type.

## types.custom

Creates a custom type. Custom types can be used for arbitrary immutable values, that have a serializable representation. For example, to create your own Date representation, Decimal type etc.

The signature of the options is:

```javascript
export type CustomTypeOptions<S, T> = {
    // Friendly name
    name: string
    // given a serialized value, how to turn it into the target type
    fromSnapshot(snapshot: S): T
    // return the serialization of the current value
    toSnapshot(value: T): S
    // if true, this is a converted value, if false, it's a snapshot
    isTargetType(value: T | S): boolean
    // a non empty string is assumed to be a validation error
    getValidationMessage?(snapshot: S): string
}
```

**Parameters**

* `options`

**Examples**

```javascript
const DecimalPrimitive = types.custom<string, Decimal>({
    name: "Decimal",
    fromSnapshot(value: string) {
        return new Decimal(value)
    },
    toSnapshot(value: Decimal) {
        return value.toString()
    },
    isTargetType(value: string | Decimal): boolean {
        return value instanceof Decimal
    },
    getValidationMessage(value: string): string {
        if (/^-?\d+\.\d+$/.test(value)) return "" // OK
        return `'${value}' doesn't look like a valid decimal number`
    }
})

const Wallet = types.model({
    balance: DecimalPrimitive
})
```

## types.Date

Creates a type that can only contain a javascript Date value.

**Examples**

```javascript
const LogLine = types.model({
  timestamp: types.Date,
})

LogLine.create({ timestamp: new Date() })
```

## types.enumeration

Can be used to create an string based enumeration. \(note: this methods is just sugar for a union of string literals\)

**Parameters**

* `name` [**string**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) descriptive name of the enumeration \(optional\)
* `options` [**Array**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)**&lt;**[**string**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)**&gt;** possible values this enumeration can have

**Examples**

```javascript
const TrafficLight = types.model({
  color: types.enumeration("Color", ["Red", "Orange", "Green"])
})
```

Returns **ISimpleType&lt;**[**string**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)**&gt;**

## types.frozen

Frozen can be used to story any value that is serializable in itself \(that is valid JSON\). Frozen values need to be immutable or treated as if immutable. They need be serializable as well. Values stored in frozen will snapshotted as-is by MST, and internal changes will not be tracked.

This is useful to store complex, but immutable values like vectors etc. It can form a powerful bridge to parts of your application that should be immutable, or that assume data to be immutable.

Note: if you want to store free-form state that is mutable, or not serializeable, consider using volatile state instead.

**Examples**

```javascript
const GameCharacter = types.model({
  name: string,
  location: types.frozen
})

const hero = GameCharacter.create({
  name: "Mario",
  location: { x: 7, y: 4 }
})

hero.location = { x: 10, y: 2 } // OK
hero.location.x = 7 // Not ok!
```

## types.identifier

Identifiers are used to make references, lifecycle events and reconciling works. Inside a state tree, for each type can exist only one instance for each given identifier. For example there couldn't be 2 instances of user with id 1. If you need more, consider using references. Identifier can be used only as type property of a model. This type accepts as parameter the value type of the identifier field that can be either string or number.

**Parameters**

* `baseType` **IType&lt;T, T&gt;**

**Examples**

```javascript
const Todo = types.model("Todo", {
     id: types.identifier(types.string),
     title: types.string
 })
```

Returns **IType&lt;T, T&gt;**

## types.late

Defines a type that gets implemented later. This is useful when you have to deal with circular dependencies. Please notice that when defining circular dependencies TypeScript isn't smart enough to inference them. You need to declare an interface to explicit the return type of the late parameter function.

**Parameters**

* `nameOrType`
* `maybeType`
* `name` [**string**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)**?** The name to use for the type that will be returned.
* `type` **ILateType&lt;S, T&gt;** A function that returns the type that will be defined.

**Examples**

```javascript
interface INode {
      childs: INode[]
 }

  // TypeScript is'nt smart enough to infer self referencing types.
 const Node = types.model({
      childs: types.optional(types.array(types.late<any, INode>(() => Node)), [])
 })
```

Returns **IType&lt;S, T&gt;**

## types.literal

The literal type will return a type that will match only the exact given type. The given value must be a primitive, in order to be serialized to a snapshot correctly. You can use literal to match exact strings for example the exact male or female string.

**Parameters**

* `value` **S** The value to use in the strict equal check

**Examples**

```javascript
const Person = types.model({
    name: types.string,
    gender: types.union(types.literal('male'), types.literal('female'))
})
```

Returns **ISimpleType&lt;S&gt;**

## types.map

Creates a key based collection type who's children are all of a uniform declared type. If the type stored in a map has an identifier, it is mandatory to store the child under that identifier in the map.

This type will always produce [observable maps](https://mobx.js.org/refguide/map.html)

**Parameters**

* `subtype` **IType&lt;S, T&gt;**

**Examples**

```javascript
const Todo = types.model({
  id: types.identifier(types.number),
  task: types.string
})

const TodoStore = types.model({
  todos: types.map(Todo)
})

const s = TodoStore.create({ todos: {} })
unprotect(s)
s.todos.set(17, { task: "Grab coffee", id: 17 })
s.todos.put({ task: "Grab cookie", id: 18 }) // put will infer key from the identifier
console.log(s.todos.get(17).task) // prints: "Grab coffee"
```

Returns **IComplexType&lt;**[**Array**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)**&lt;S&gt;, IObservableArray&lt;T&gt;&gt;**

## types.maybe

Maybe will make a type nullable, and also null by default.

**Parameters**

* `type` **IType&lt;S, T&gt;** The type to make nullable

Returns **\(IType&lt;\(S \| null \|** [**undefined**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined)**\), \(T \| null\)&gt;\)**

## types.model

Creates a new model type by providing a name, properties, volatile state and actions.

See the [model type](https://mobx-state-tree.gitbook.io/docs/concepts/creating-models) description or the [getting started](https://mobx-state-tree.gitbook.io/docs/getting-started) tutorial.

## types.null

The type of the value `null`

## types.number

Creates a type that can only contain a numeric value. This type is used for numeric values by default

**Examples**

```javascript
const Vector = types.model({
  x: types.number,
  y: 0
})
```

## types.optional

`types.optional` can be used to create a property with a default value. If the given value is not provided in the snapshot, it will default to the provided `defaultValue`. If `defaultValue` is a function, the function will be invoked for every new instance. Applying a snapshot in which the optional value is _not_ present, causes the value to be reset

**Parameters**

* `type`
* `defaultValueOrFunction`

**Examples**

```javascript
const Todo = types.model({
  title: types.optional(types.string, "Test"),
  done: types.optional(types.boolean, false),
  created: types.optional(types.Date, () => new Date())
})

// it is now okay to omit 'created' and 'done'. created will get a freshly generated timestamp
const todo = Todo.create({ title: "Get coffee "})
```

## types.reference

Creates a reference to another type, which should have defined an identifier. See also the [reference and identifiers](https://mobx-state-tree.gitbook.io/docs/concepts/references-and-identifiers) section.

**Parameters**

* `subType`
* `options`

## types.refinement

`types.refinement(baseType, (snapshot) => boolean)` creates a type that is more specific than the base type, e.g. `types.refinement(types.string, value => value.length > 5)` to create a type of strings that can only be longer then 5.

**Parameters**

* `name` [**string**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)
* `type` **IType&lt;T, T&gt;**

Returns **IType&lt;T, T&gt;**

## types.string

Creates a type that can only contain a string value. This type is used for string values by default

**Examples**

```javascript
const Person = types.model({
  firstName: types.string,
  lastName: "Doe"
})
```

## types.undefined

The type of the value `undefined`

## types.union

types.union\(dispatcher?, types...\) create a union of multiple types. If the correct type cannot be inferred unambiguously from a snapshot, provide a dispatcher function of the form \(snapshot\) =&gt; Type.

**Parameters**

* `dispatchOrType` **\(ITypeDispatcher \| IType&lt;any, any&gt;\)**
* `otherTypes` **...**[**Array**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)**&lt;IType&lt;any, any&gt;&gt;**

Returns **IType&lt;any, any&gt;**

## unescapeJsonPath

unescape slashes and backslashes

**Parameters**

* `str`

## unprotect

By default it is not allowed to directly modify a model. Models can only be modified through actions. However, in some cases you don't care about the advantages \(like replayability, traceability, etc\) this yields. For example because you are building a PoC or don't have any middleware attached to your tree.

In that case you can disable this protection by calling `unprotect` on the root of your tree.

**Parameters**

* `target`

**Examples**

```javascript
const Todo = types.model({
    done: false
}).actions(self => ({
    toggle() {
        self.done = !self.done
    }
}))

const todo = Todo.create()
todo.done = true // throws!
todo.toggle() // OK
unprotect(todo)
todo.done = false // OK
```

## walk

Performs a depth first walk through a tree

**Parameters**

* `target`
* `processor`

