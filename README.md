sources: 
- https://www.youtube.com/watch?v=HYKDUF8X3qI  
- https://www.youtube.com/watch?v=n7xQVRpYHYY

TO READ: https://deku.posstree.com/en/react/context-api/  

# What is a Context in React?

Think of it as a way to store **data** and have it be **accessible** to any component.  
**Contexts** let our components access data without having to pass that data through **props**.  

`useContext` lets our components access **shared data** from anywhere in the **component tree**, 
without having to pass that data to mutliple components...  

With `useContext`, our data lives in one place, and any component that needs it can just "grab" it.  
No need to "bother" any other component.  

It's the easy direct way to share a **state** in React.  

In 2018, React added the **Context API** to help with the annoying issue of **prop drilling**.  
In 2019, the `useContext` hook made accessing context values as easy as calling a function inside your component.  

# How does `useContext` work?

Think of it like a magic **portal** that sends data to any component in your app without needing to pass
it through every layer.  

To create this portal, we use `createContext()` and store it in a variable.  
This portal can hold any data we want to share.  

To open the portal, we need to use the `Context.Provider`.  
Then include all the components that can access the portal, and load the provider with all 
the items we want to send.  
 
<img src="image.png" alt="snippet showing createContext in action" width="600">  
  

To use the portal in any component, we use the `useContext` hook, just passing it the portal we 
created with `createContext()`.  
Then extract the data into a variable :

<img src="image-1.png" alt="snippet showing useContext in action" width="600">  

# How to create a Context?

We usually create our context files within a `/src/contexts` folder.

Here's an example of a context file, which would be named `DashboardContext.tsx`:
```tsx
import { createContext } from "react";
import type { User } from "../types";

export const DashboardContext = createContext<User | undefined>(undefined);
```

We want to create a context that can be of `User` type or `undefined`.  
But why `undefined`?  

Our context shouldn't hold nothing but a `user`.  
However, since the context gets created outside of any component, we do not have access
to the `user` at this point.  

# How to provide a Context to a component?

Once we've created a context, we need to provide it to a certain set of components.  
For that, we need to use the `Provider` keyword.  

In the targeted component, we need to import and then **provide** the context:
```tsx
import { useState } from "react";
import { DashboardContext } from "../contexts/DashboardContext";
import type { User } from "../types";
import { UserProfile } from "./UserProfile"; // The component that will consume the context

export default function DashboardPage() {
  const [user, setUser] = useState<User>({
    isSubscribed: true,
    name: 'You',
  });

  return (
    <DashboardContext.Provider value={user}>
      <h1>Dashboard</h1>
      {/* UserProfile is a child component that can now access the context */}
      <UserProfile />
    </DashboardContext.Provider>
  )
}
```
As you can see, we also need to specify a `value`.  
The line `<DashboardContext.Provider value={user}>` makes the user object available to any child component that asks for it, like `<UserProfile />`.  

# How to consume a Context?

1. We create a context
2. We provide it to a component via the `.Provider` keyword
3. We are going to **consume** the context via the `useContext()` hook.  

To access the context's value, a child component must use the `useContext` hook.  
Here is the code for the `<UserProfile />` component we used above.  
It **imports** the same context and uses the `useContext` hook to read the `user` data:
```tsx
import { useContext } from "react";
import { DashboardContext } from "./contexts/DashboardContext";

export const UserProfile = () => {
  // Here we "consume" the context value
  const user = useContext(DashboardContext);

  // It's good practice to handle the case where the context might be undefined,
  // as we defined it initially when calling createContext.
  if (!user) {
    return <div>User not found</div>;
  }

  return (
    <section>
      <h2>Welcome, {user.name}!</h2>
      <p>Subscription Active: {user.isSubscribed ? 'Yes' : 'No'}</p>
    </section>
  );
};
```
Now, the `UserProfile` component has access to the `user` state from `DashboardPage` without it 
ever being passed as a **prop**! This is the **power** of the **Context API**.  