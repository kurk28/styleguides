# Basic rules

1. Basic principles and patterns that we specially follow to make the code maintainable and replaceable: [KISS](https://en.wikipedia.org/wiki/KISS_principle), [low coupling](https://en.wikipedia.org/wiki/Coupling_(computer_programming)), [low cohesion](https://en.wikipedia.org/wiki/Cohesion_(computer_science)), [LoD](https://en.wikipedia.org/wiki/Law_of_Demeter), [IoC](https://en.wikipedia.org/wiki/Inversion_of_control), [encapsulation](https://en.wikipedia.org/wiki/Encapsulation_(computer_programming)), [separation of concerns](https://en.wikipedia.org/wiki/Separation_of_concerns),[singleton](https://en.wikipedia.org/wiki/Singleton_pattern)
2. Code should avoid patterns that are known to cause problems, especially for users new to the language
3. Code across projects should be consistent across irrelevant variations
4. Code should be maintainable in the long term, try to avoid temporary solutions, because we know that here is not more permanent, that temporary
5. Code reviewers should be focused on improving the quality of the code, not enforcing arbitrary rules. Please, share your experience with colleagues, but remember that code should be readable for all teammates
6. New added file should be covered with unit tests

<br />

# TypeScript

1. Write variable names in camelcase and constant names in uppercase styles [camelcase](https://eslint.org/docs/latest/rules/camelcase)

2. Avoid magic `number`s [no-magic-numbers](https://typescript-eslint.io/rules/no-magic-numbers)

3. Variables contain `boolean` value should be named with `is`, `has` or `can` prefix

4. Avoid non-`null` assertion (`!`) [no-non-null-assertion](https://typescript-eslint.io/rules/no-non-null-assertion)

5. Don't use `any` type. Use `unknown` instead of it. `Unknown` can hold any value, including `null` or `undefined`, and is generally more appropriate [no-explicit-any](https://typescript-eslint.io/rules/no-explicit-any)

6. Always use the lowercase version of `string`, `number` and `boolean`
```typescript
// DON'T:
const age: Number = 10
const name: String = "Ilia"

// DO:
const age: number = 10
const name: string = "Ilia"
```

7. Do not mark `interface`s specially, for instance
```typescript
// DON'T
IComponentOwnProps {
  /* some code here */
 }

// DO:
ComponentOwnProps {
  /* some code here */
}
```

8. Prefer `interface`s over `type`s. `Interface`s create a single flat object type that [detects property conflicts](https://github.com/microsoft/TypeScript/wiki/Performance#preferring-interfaces-over-intersections), which are usually important to resolve. Also `type` could have a [performance issue](https://ncjamieson.com/prefer-interfaces/) [consistent-type-definitions](https://typescript-eslint.io/rules/consistent-type-definitions)
```typescript
// DON'T:
type Bar = { prop: string };

// DO:
interface Foo { prop: string };
```

```typescript
// DON'T:
type Foo = Bar & Baz & {
  someProp: string;
};

// DO:
interface Foo extends Bar, Baz {
  someProp: string;
};
```

9. Use `type` annotation. Adding `type` annotations, especially return `type`s, can [save the compiler a lot of work](https://github.com/microsoft/TypeScript/wiki/Performance#using-type-annotations). The compiler can infer what your code does, but it has no idea what you intended. It helps to read and make code maintainable in the future [consistent-generic-constructors](https://typescript-eslint.io/rules/consistent-generic-constructors) [explicit-function-return-type](https://typescript-eslint.io/rules/explicit-function-return-type)

```typescript
// DON'T:
export function func(data) {};

// DO:
interface Data { /* some code here */}
export function func(data: Data): OtherType {
  return otherFunc();
};
```

10. Try to avoid using `null` for fields, that you can set with default values. It makes it unnecessary to check for `null` and helps `V8` to optimize our code.
```typescript
  // DON'T:
  interface UserData {
    name: string | null;
    age: number | null;
    city: string | null;
  }

  const userData: UserData = {
    name: null,
    age: null,
    city: null,
  }

  // DO:
  interface UserData {
    name: string;
    age: number;
    city: string;
  }

  const userData: UserData = {
    name: '',
    age: 0,
    city: '',
  }
```

11. Try to fill array with objects that have the same type. Don't create sparse arrays. These block `V8` from [optimizing](https://mathiasbynens.be/notes/shapes-ics) the code [no-sparse-arrays](https://eslint.org/docs/latest/rules/no-sparse-arrays) [no-array-delete](https://typescript-eslint.io/rules/no-array-delete)
```typescript
// DON'T:
const arr: any[] = [1, Symbol(1), '1'];

// DO:
const arr: number[] = [1,2,3,4];
```

12. If `function` accepts more than three arguments, put them into object [max-params](https://typescript-eslint.io/rules/max-params)
```typescript
// DON'T:
  function calculatePosition(a, b, c, d) { /* some code here */ }

// DO:
  interface Coordinate {
    a: number;
    b: number:
    c: number:
    d: number;
  }

  function calculatePosition({a, b, c, d}: Coordinate) { /* some code here */ }
```

13. Don't overplay with `TypeScript` and data structures. For instance, you may use simple `Object` instead of `Map` if you want to [get value by key fast](https://v8.dev/blog/fast-properties). `Map` could bring an [issue](https://www.typescriptlang.org/play/?#code/PTAEHUFMBsGMHsC2lQBd5oBYoCoE8AHSAZVgCcBLA1UABWgEM8BzM+AVwDsATAGiwoBnUENANQAd0gAjQRVSQAUCEmYKsTKGYUAbpGF4OY0BoadYKdJMoL+gzAzIoz3UNEiPOofEVKVqAHSKymAAmkYI7NCuqGqcANag8ABmIjQUXrFOKBJMggBcISGgoAC0oACCbvCwDKgU8JkY7p7ehCTkVDQS2E6gnPCxGcwmZqDSTgzxxWWVoASMFmgYkAAeRJTInN3ymj4d-jSCeNsMq-wuoPaOltigAKoASgAywhK7SbGQZIIz5VWCFzSeCrZagNYbChbHaxUDcCjJZLfSDbExIAgUdxkUBIursJzCFJtXydajBDIKMjJBhLAAidXEAG8AL7BVDtB6Cb701DiAC8oAAsgwCAAeQSoSicZj8HkMAB8wQQnAloHYXLI+U53IZoAFnEgEiFIoAFABKYLq74BLmoE0AcgAktAKAArBj2-iMtFcSV4LUAAwA4t9EGY8AHQMzQBbFMrBPB3AFoPBmCarWQAsxIHanS73fazbH4zQEAQ8PcNXKtZWdby9aBvQFmxmo0rGgmkym02WK1WGVmcw7nW6PUXgiocM0PGQvIh4H0GMD2DQvm4zMx2Axs-xYC7YIkl-A9CIvAAiACiqwYiAW+jPSWxZ-ADlQ9uEADlDWegioAPJfGQ7xcvw2ZHLyZAKK40h4KATjzjowxYPoKAIIg0LCJca4SAu0QiAYHDYgsTCsBwPBBCUQA) that `TypeScript` doesn't handle, also `Object` brings you a obvious way to iterate over the keys and values easily by calling `Object.keys()`, `Object.values()` methods. You only need to use a `Map` if you want to store key not as a `string`, in opposite way, please, use `Object`

14. Hide implementation from consumer (**encapsulation**, **hight cohesion**)
```typescript
// DON'T:
interface AuthConfig {
  /* some code here */
}
let accessToken: string = '';
export function getAccessToken(): string {
  return accessToken;
}
export function setAccessToken(token: string) {
  accessToken = token;
}
export function getAuthConfig(): AuthConfig { /* some code here */ }

// DO:
/* instead of class you can use default Proxy pattern in JavaScript  */
class AuthConfig {
  #accessToken: string;;
  #authConfig: AuthConfig;

  constructor(defaultToken = '') {
    this.#accessToken = defaultToken
    /* some code here */
    Object.freeze(this.#authConfig);)
  }

  getAccessToken(): string { return this.#accessToken }
  setAccessToken(token: string) { this.#accessToken = token }
  getAuthConfig(): AuthConfig { return this.#authConfig }

}

export const authConfig = new AuthConfig();
```

<br />

# React

1. Folders structure

The main idea is to show visible hierarchy and get low coupling (how strongly one independent element is connected to another) and hight cohesion (the responsibilities of a given set of elements are strongly related and highly focused on a rather specific topic).
Component can import only components on the same level or direct descendant. Shared components may be imported from `common` folder.
For example, in the next folder structure we set `MyPage` component that import `Form` and `Modal` components, but can't import `Sidebar` component directly, the `Sidebar` can be imported only in `Form` component

    .
    ├── shared
    │   └── components
    │       ├── Button
    │       │   ├── Button.tsx
    │       │   └── Button.test.tsx 
    │       │                                 # shared Button component
    │       ├── Input                         # shared Input component
    │       └── ...
    ├── pages
    │   └── MyPage                            # Root folder for MyPage
    │       ├── MyPage.tsx                    # MyPage component
    │       ├── MyPage.test.tsx               # Tests for MyPage component
    │       ├── Form                          # Folder for Form
    │       │   ├── Form.tsx                  # Form component
    │       │   ├── Form.test.tsx             # Tests for Form component
    │       │   └── Sidebar 
    │       │       ├── Sidebar.tsx
    │       │       └── Sidebar.test.tsx
    │       └── Modal
    │           ├── Modal.tsx
    │           └── Modal.test.tsx
    ├── store                                 # Redux store
    ├── utils                                 # only one place where utils file are allowed
    │


2. Filename: use `CamelCase` for filenames
```typescript
// DON'T:
mySuperClass.tsx;

// DO:
MySuperClass.tsx;
```

3. Use the filename as the component name

4. Only include **one** React component per file (you could also have a stateless component if you use it in the same file) [react/no-multi-comp](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless)

```typescript
// DON'T
const HeadlessComponent = () => { /* some code here */ }
const FirstComponent = (props) => { 
  /* some code here */ 
  return <HeadlessComponent />
}
const SecondComponent = (props) => { /* some code here */ }

// DO:
const HeadlessComponent = () => { /* some code here */ }
const FirstComponent = (props) => { 
  /* some code here */ 
  return <HeadlessComponent />
}
```

5. Prefer Functional component over Class component in a new code

6. Move functions declaration that not influence on render directly outside of component and test them separately

```typescript
// DON'T:
interface Element {
  id: string;
};

const TheComponent = () => {
  const [elements, setElements] = useState<Element[]>([]);
  const createNewElement = (): Element => {
    /* some code here*/
      return newElement;
    };

  const addNewELement = (_event) => {
    const newElement = createNewElement();
    const updatedElements = [...elements, newElement];
    setElements(updatedElements);
  }

  return (
    <>
      <button onClick={addNewELement}>Add new element</button>
      {data.map((element) => element.id)}
    </>
  )
}

// DO:
interface Element {
  id: string;
};

const createNewElement = (): Element => {
  /* some code here*/
    return newElement;
  };

const TheComponent = () => {
  const [elements, setElements] = useState<Element[]>([]);
  const addNewELement = (_event) => {
    const newElement = createNewElement();
    const updatedElements = [...elements, newElement];
    setElements(updatedElements);
  }

  return (
    <>
      <button onClick={addNewELement}>Add new element</button>
      {data.map((element) => element.id)}
    </>
  )
}
```

7. Declare `interface`'s and `type`'s at the beginning of the file
```typescript
// DON'T:
const TheComponent = (props: TheComponentProps) => {};
interface TheComponentProps {};
interface NewElement {};
const createNewElement = (): NewElement => {}

// DO:
interface NewElement {};
interface TheComponentProps {};
const createNewElement = (): NewElement => {}
const TheComponent = (props: TheComponentProps) => {
  /* use createNewElement function here  */
};
```

8. If you pass a callback or object to another React component wrap it with `useCallback`, `useMemo` or `useState` hooks
```typescript
// DON'T:
const FormComponent = () => {
  return <Button onClick={(event) => { /* some code here */}}>Save form</Button>;
};

// DO:
const FormComponent = () => {
  const saveForm = useCallback((event) => {
    /* some code here */
  }. []);

  return <Button onClick={saveForm}>Save form</Button>;
};
```

9. Don't try to put everything into `useMemo` hook for optimizing calculations. Put a data inside it when you pass the data as a property to another React component

10. Prefer destructuring for property access  [prefer-destructuring](https://eslint.org/docs/latest/rules/prefer-destructuring)
```typescript
// DON'T:
const TheComponent = (props: TheProps) => (
  <>
    {`client has id: ${props.id}, name: ${props.name} and status ${props.status}`}
  </>
)

// DO:
const TheComponent = ({ id, name, status }: TheProps) => (
  <>
    {`client has id: ${id}, name: ${name} and status ${status}`}
  </>
)
```

11. Always use double quotes (") for JSX attributes, but single quotes (') for all other JavaScript [jsx-quotes](https://eslint.org/docs/latest/rules/jsx-quotes)

12. Avoid inline styles; use CSS classes instead. CSS classes help us to select element it also it could be overridden without important
```typescript
// DON'T:
const TheComponent = () => <button styles={{borderRadius: '20px'}}>Cancel</button>

// DO:
import * as styles from  TheComponent.css

const TheComponent = () => <button className={styles.button}>Cancel</button>
```

13. Combine calculation in one variable with calling name
```jsx
// DON'T:
isAvailable && isEditable && !isEmpty && <TheSearchInput />;

// DO:
isSearchInputShown = isAvailable && isEditable && !isEmpty;
isSearchInputShown && <TheSearchInput />;  
```

14. Use short-circuit evaluation or ternary operators for conditional rendering, avoid multiple return from component
```typescript
// DON'T:
const Router = ({ isLoggedIn }) => {
  if (isLoggedI) {
    return <MainPage />;
  } else {
    <LoginPage />;
  }
}

// DO:
const Router = ({ isLoggedIn }) => isLoggedIn ? <MainPage /> : <LoginPage />

// DO:
const Router = ({ isLoggedIn, isFooterShown, isHeaderShown }) => {
  return (
    <>
      {
        isHeaderShown && <isHeaderShown />
        isLoggedIn &&  <MainPage />
        !isLoggedIn && <LoginPage />
        isFooterShown && <Footer />
      }
    </>
  )

}
```

15. New created component should do only one thing and should have only one reason to be updated (**separation of concerns**)
```typescript
// DON'T:
interface ButtonProps {
  buttonName: string;
  labelName: string;
  onButtonClick: (event) => void;
}

const Button = ({ labelName, buttonName, onButtonClick }: ButtonProps) => (
  <>
    <label for="input" >{labelName}</label>
    <input type="checkbox" name>
    <button onCLick={onButtonClick}>{buttonName}</button>  
  </>
)

// DO:
interface ButtonProps {
  name: string;
  onClick: (event) => void;
};

const Button = ({ name, onClick } : ButtonProps) => (
  <button onClick={onClick}>{name}</button>
);
```

16. Don't let to your component know a lot about states in our application, use `IoC` to pass necessary props to your children component (**low coupling**)
```typescript
// DON'T:
interface Client {
  name: string;
};

interface Order {
  id: string;
};

interface ClientView {
  client?: Client;
}

interface OrderView {
  order?: Order;
}

const ClientView = () => {
  const client: ClientView  = useGetClient();
  return <div>{ client ? {client.name} : '' }</div>
};
const OrderView = () => {
  const order: OrderView = useGetOrder();
  return <div>{ order ? {order.id} : '' }</div>  
}

const ChildrenComponent = () => {
  const client: ClientView  = useGetClient();
  const order: OrderView = useGetOrder();
  return (
    <>
      {client ? <div>Client info</div> : ''}
      <ClientView />
      {order ? <div>Order info</div> : ''}
      <OrderView />
    </>
  )
}

const MainComponent = () => {
  /* some code here */
  <ChildrenComponent />
}

// DO:
interface Client {
  name: string;
};

interface Order {
  id: string;
};

interface ClientView {
  client?: Client;
}

interface OrderView {
  order?: Order;
}

interface ClientViewProps {
  client: Client;
}

interface OrderViewProps {
  order: Order;
}

interface ChildrenComponentProps {
  client: Client;
  order: Order;
};

const ClientView = ({ client }: ClientViewProps) => <span>{client.name}</span>;
const OrderView = ({ order }: OrderViewProps) => <span>{order.id}</span>

const ChildrenComponent = ({ client, order }: ChildrenComponentProps) => {
  <>
    {<div>Client info</div>}
    <ClientView client={client} />
    {<div>Order info</div>}
    <OrderView order={order} />
  </>
}
const MainComponent = () => {
  /* some code here */
  const client: ClientView = useGetClient();
  const order: OrderView = useGetOrder();
  const hasData = Boolean(order && client);
  return (
    {hasData && <ChildrenComponent order={order} client={client} />}
  )
}

```

TypeScript config properties:
- [strictNullChecks](https://www.typescriptlang.org/tsconfig/#strictNullChecks)
- [allowUnreachableCode](https://www.typescriptlang.org/tsconfig/#Type_Checking_6248)
- [exactOptionalPropertyTypes](https://www.typescriptlang.org/tsconfig/#exactOptionalPropertyTypes)
- [noImplicitReturns](https://www.typescriptlang.org/tsconfig/#noImplicitReturns)

Additional rules that should  be turned on:
- [no-unnecessary-boolean-literal-compare](https://typescript-eslint.io/rules/no-unnecessary-boolean-literal-compare)
- [no-unnecessary-condition](https://typescript-eslint.io/rules/no-unnecessary-condition)
- [no-unused-expressions](https://eslint.org/docs/latest/rules/no-unused-expressions)
- [no-unused-vars](https://eslint.org/docs/latest/rules/no-unused-vars)
- [no-unused-vars](https://typescript-eslint.io/rules/no-unused-vars)
- [no-use-before-define](https://eslint.org/docs/latest/rules/no-use-before-define)
- [no-return-await](https://eslint.org/docs/latest/rules/no-return-await)
- [no-duplicate-imports](https://eslint.org/docs/latest/rules/no-duplicate-imports)
- [consistent-return](https://eslint.org/docs/latest/rules/consistent-return)
- [default-case](https://eslint.org/docs/latest/rules/default-case)
- [no-nested-ternary](https://eslint.org/docs/latest/rules/no-nested-ternary)
- [no-unneeded-ternary](https://eslint.org/docs/latest/rules/no-unneeded-ternary)
- [no-unused-expressions](https://eslint.org/docs/latest/rules/no-unused-expressions)
- [no-useless-return](https://eslint.org/docs/latest/rules/no-useless-return)

Additional rules that can be turned on:
- [no-base-to-string](https://typescript-eslint.io/rules/no-base-to-string)
- [no-confusing-non-null-assertion](https://typescript-eslint.io/rules/no-confusing-non-null-assertion)
- [no-duplicate-enum-values](https://typescript-eslint.io/rules/no-duplicate-enum-values)
- [no-duplicate-type-constituents](https://typescript-eslint.io/rules/no-duplicate-type-constituents)
- [no-dynamic-delete](https://typescript-eslint.io/rules/no-dynamic-delete)
- [no-empty-function](https://typescript-eslint.io/rules/no-empty-function)
- [no-empty-interface](https://typescript-eslint.io/rules/no-empty-interface)
- [no-extra-non-null-assertion](https://typescript-eslint.io/rules/no-extra-non-null-assertion)
- [no-extraneous-class](https://typescript-eslint.io/rules/no-extraneous-class)
- [no-floating-promises](https://typescript-eslint.io/rules/no-floating-promises)
- [no-for-in-array](https://typescript-eslint.io/rules/no-for-in-array)
- [no-implied-eval](https://typescript-eslint.io/rules/no-implied-eval)
- [no-inferrable-types](https://typescript-eslint.io/rules/no-inferrable-types)
- [no-mixed-enums](https://typescript-eslint.io/rules/no-mixed-enums)
- [no-non-null-asserted-nullish-coalescing](https://typescript-eslint.io/rules/no-non-null-asserted-nullish-coalescing)
- [no-non-null-asserted-optional-chain](https://typescript-eslint.io/rules/no-non-null-asserted-optional-chain)
- [no-redundant-type-constituents](https://typescript-eslint.io/rules/no-redundant-type-constituents)
- [no-useless-empty-export](https://typescript-eslint.io/rules/no-useless-empty-export)
