# shadCN - UI Library

### Init ShadeCN in the project

> ShadeCN can controll by saying ``` npx shadcn@latest [command] ``` .

> For installing Run ``` npx shadcn@latest init ``` .

<h5> Note ! </h5>

> ShadeCN is not fully downloaded to the project only the dependencies did , when you need to install component just install by the following way ``` npx shadcn@latest add button ```

### ShadCN Comes with some default props that can change in setting , Example:

```tsx

import * as React from "react"
import { Slot } from "@radix-ui/react-slot"
import { cva, type VariantProps } from "class-variance-authority"

import { cn } from "@/lib/utils"

const buttonVariants = cva(
  "inline-flex items-center justify-center gap-2 whitespace-nowrap rounded-md text-sm font-medium transition-all disabled:pointer-events-none disabled:opacity-50 [&_svg]:pointer-events-none [&_svg:not([class*='size-'])]:size-4 shrink-0 [&_svg]:shrink-0 outline-none focus-visible:border-ring focus-visible:ring-ring/50 focus-visible:ring-[3px] aria-invalid:ring-destructive/20 dark:aria-invalid:ring-destructive/40 aria-invalid:border-destructive",
  {
    variants: {
      variant: {
        default: "bg-primary text-primary-foreground hover:bg-primary/90",
        destructive:
          "bg-destructive text-white hover:bg-destructive/90 focus-visible:ring-destructive/20 dark:focus-visible:ring-destructive/40 dark:bg-destructive/60",
        outline:
          "border bg-background shadow-xs hover:bg-accent hover:text-accent-foreground dark:bg-input/30 dark:border-input dark:hover:bg-input/50",
        secondary:
          "bg-secondary text-secondary-foreground hover:bg-secondary/80",
        ghost:
          "hover:bg-accent hover:text-accent-foreground dark:hover:bg-accent/50",
        link: "text-primary underline-offset-4 hover:underline",
      },
      size: {
        default: "h-9 px-4 py-2 has-[>svg]:px-3",
        sm: "h-8 rounded-md gap-1.5 px-3 has-[>svg]:px-2.5",
        lg: "h-10 rounded-md px-6 has-[>svg]:px-4",
        icon: "size-9",
      },
    },
    defaultVariants: {
      variant: "default",
      size: "default",
    },
  }
)

function Button({ // This is Button Props that can add to the Button as Props and change some setting
  className, // Adding Custom Tailwind if needed
  variant, // Refer to the color Pallete
  size, // Refer to the Size of the Button
  asChild = false, // Will Explain in next point
  ...props
}: React.ComponentProps<"button"> &
  VariantProps<typeof buttonVariants> & {
    asChild?: boolean
  }) {
  const Comp = asChild ? Slot : "button"

  return (
    <Comp
      data-slot="button"
      className={cn(buttonVariants({ variant, size, className }))}
      {...props}
    />
  )
}

export { Button, buttonVariants }


```

## Understanding `asChild` in shadcn/ui

In **shadcn/ui** (which is built on top of Radix UI primitives), the `asChild` prop is used to tell a component to **render a different element instead of its default one**, while still keeping all the styles, behavior, and accessibility features of the shadcn/Radix component.

---

### 🔹 Example: Button without `asChild`

```tsx
import { Button } from "@/components/ui/button"

export default function Page() {
  return (
    <Button>
      Click Me
    </Button>
  )
}
```

👉 By default, `<Button>` renders as a `<button>` element.

---

### 🔹 Example: Button with `asChild`

```tsx
import Link from "next/link"
import { Button } from "@/components/ui/button"

export default function Page() {
  return (
    <Button asChild>
      <Link href="/dashboard">Go to Dashboard</Link>
    </Button>
  )
}
```

👉 Here:

- The `Button` won’t render a `<button>` at all.  
- Instead, it will render the child (`<Link>`) as the main element, but **with all the styles and interactions of the Button component** applied.  

---

### ✅ Why it matters

- Useful when you want to use `shadcn/ui` styling + behavior but render **a different HTML tag** (`<a>`, `<div>`, `<Link>`, etc.).  
- Keeps things accessible and consistent.  
- Powered by Radix UI’s [`Slot`](https://www.radix-ui.com/docs/primitives/utilities/slot) component under the hood.

---

👉 **In short:**  
`asChild` makes the component a wrapper that passes its styles/props to the child element you provide.

<hr>

**as Button is Rendered as Button Element in the HTML in final we can also adding the ``` disabled ``` attribute to it**

```tsx

import { Button } from "@/components/ui/button"

export default function Page() {
  return (
    <Button disabled>
      Click Me
    </Button>
  )
}

```

## Adding Icons 

> if we want to add icons to the element it's ease **ShadCN** is using open source library called Lucide [https://lucide.dev/] just copy the JSX format and add the to the code like that :

```tsx
import { Button } from "@/components/ui/button"
import { ArrowBigUp } from "lucide-react";

export default function Page() {
  return (
    <Button disabled>
      <ArrowBigUp />
      Click Me
    </Button>
  )
}

```
## CN - Class Name Merging

> When we install ShadeCN their is a folder called 📂**lib** have a file called 🖹**utils.ts** this allow us to make conditions when typing the tailwind with good way :

```tsx

import { Button } from "@/components/ui/button"
import { ArrowBigUp } from "lucide-react";
import {useState} from 'react';

export default function Page() {
  const [number , setNumber] = useState<number>(0);

  return (
    <Button
      className={cn('m-8' , number ? 'bg-red-600':null , 'p-8' , number > 10 ? 'bg-gree-800':null)}
      onClick={() => setNumber(perv => perv + 1)}
    >
      <ArrowBigUp />
      Click Me
    </Button>
  )
}
```
