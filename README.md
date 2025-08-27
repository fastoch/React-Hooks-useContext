src = https://www.youtube.com/watch?v=HYKDUF8X3qI  

TO READ: https://deku.posstree.com/en/react/context-api/  

# What is Context in React?

Think of it as a way to store **data** and have it be **accessible** to any component, no matter where they are in the App **tree**.  
**Contexts** let our components access data without having to pass that data through **props**.  

In other terms, it's a way to have some **state** that is accessible by multiple components without having to do any **tedious prop drilling**.  

The general idea is: REMOVE `props` and make the corresponding `states` accessible to multiple components.  

# How to create a Context?

We usually create our context files within a `/src/contexts` folder.

```tsx
import { createContext ] from "react";

export const DashboardContext = createContext(undefined);

```

---
@2/16
