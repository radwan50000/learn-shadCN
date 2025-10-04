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

## What is 'sr-only'

> **``` sr-only ```** is using to the people who didn't see (blind) and use the screen reader of the website it's make the text hidden but readable by the screen reader to help those people , Example :

```tsx
import { Button } from "@/components/ui/button"
import { ArrowBigUp } from "lucide-react";

export default function Page() {
  return (
    <Button disabled>
      <ArrowBigUp />
      Login
      <span className='sr-only'> Login Button </span>
    </Button>
  )
}

```

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


### Change Color

> To change color simply add ``` variant ```  attribute

```tsx
import { Button } from "@/components/ui/button"

export default function Page() {
  return (
    <Button variant='destructive'>
      Click Me
    </Button>
  )
}
```

## 🌗 Using Themes in ShadCN + Next.js

### ShadCN components support dark mode out of the box. To manage themes (light/dark/system), the library uses [next-themes](https://github.com/pacocoursey/next-themes)

## 1. Install dependencies

```bash

npm install next-themes


```

## 2. Wrap your app with ``` ThemeProvider ```

<h4>
  In app/layout.tsx (or app/providers.tsx if you split it):
</h4>

```tsx

import "./globals.css"
import { ThemeProvider } from "@/components/theme-provider"

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en" suppressHydrationWarning>
      <body>
        <ThemeProvider
          attribute="class"
          defaultTheme="system"
          enableSystem
          disableTransitionOnChange
          // superHydrationWarning is dev-only (optional)
        >
          {children}
        </ThemeProvider>
      </body>
    </html>
  )
}


```


## 3. Create  theme-provider.tsx

<h4>
  Inside components/ :
</h4>

```tsx

"use client"

import * as React from "react"
import { ThemeProvider as NextThemesProvider } from "next-themes"
import { type ThemeProviderProps } from "next-themes/dist/types"

export function ThemeProvider({ children, ...props }: ThemeProviderProps) {
  return <NextThemesProvider {...props}>{children}</NextThemesProvider>
}


```


## 4. Add a Theme Toggle (Light/Dark Switch)

```tsx

"use client"

import { useTheme } from "next-themes"
import { Sun, Moon } from "lucide-react"
import { Button } from "@/components/ui/button"

export function ThemeToggle() {
  const { theme, setTheme } = useTheme()

  return (
    <Button
      variant="outline"
      size="icon"
      onClick={() => setTheme(theme === "light" ? "dark" : "light")}
    >
      <Sun className="h-5 w-5 rotate-0 scale-100 transition-all dark:-rotate-90 dark:scale-0" />
      <Moon className="absolute h-5 w-5 rotate-90 scale-0 transition-all dark:rotate-0 dark:scale-100" />
      <span className="sr-only">Toggle theme</span>
    </Button>
  )
}


```


### 5. Tailwind Config

```tsx

module.exports = {
  darkMode: ["class"],
  content: [
    "./pages/**/*.{js,ts,jsx,tsx,mdx}",
    "./components/**/*.{js,ts,jsx,tsx,mdx}",
    "./app/**/*.{js,ts,jsx,tsx,mdx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}


```


## 6. Common Options in ThemeProvider

<ul>

<li>
attribute="class" → toggles class="light" / class="dark"
</li>

<li>
defaultTheme="system" → follows system theme if no preference is saved
</li>

<li>
enableSystem → allows detecting OS preference
</li>

<li>
disableTransitionOnChange → removes ugly transition flicker
</li>

<li>
superHydrationWarning → dev-only console warning if SSR theme ≠ client theme
</li>

</ul>

# Sidebar

> Side Bar is Important Thing and it's very beauty and usefull U can use it from here https://ui.shadcn.com/docs/components/sidebar

### Important Topic U must know in it

<ul>
  <li>
    SideBar Structure
  </li>
  <li>
    SideBar Provider
  </li>
  <li>
    Using Custom Button Toggle Button by useSideBar() hook
  </li>
  <li>
    SideBar with using drop down menu
  </li>
  <li>
    SideBar Menu Badge
  </li>
  <li>
    SideBar Actions (very Usefull)
  </li>
  <li>
    Collabsable SideBar Group
  </li>
</ul>
