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

# Charts
> you can use charts from these link ->  https://ui.shadcn.com/charts/area

## Example On Bar Chart :

> Chart1.tsx

```tsx

"use client"

import * as React from "react"
import * as RechartsPrimitive from "recharts"

import { cn } from "@/lib/utils"

// Format: { THEME_NAME: CSS_SELECTOR }
const THEMES = { light: "", dark: ".dark" } as const

export type ChartConfig = {
  [k in string]: {
    label?: React.ReactNode
    icon?: React.ComponentType
  } & (
    | { color?: string; theme?: never }
    | { color?: never; theme: Record<keyof typeof THEMES, string> }
  )
}

type ChartContextProps = {
  config: ChartConfig
}

const ChartContext = React.createContext<ChartContextProps | null>(null)

function useChart() {
  const context = React.useContext(ChartContext)

  if (!context) {
    throw new Error("useChart must be used within a <ChartContainer />")
  }

  return context
}

function ChartContainer({
  id,
  className,
  children,
  config,
  ...props
}: React.ComponentProps<"div"> & {
  config: ChartConfig
  children: React.ComponentProps<
    typeof RechartsPrimitive.ResponsiveContainer
  >["children"]
}) {
  const uniqueId = React.useId()
  const chartId = `chart-${id || uniqueId.replace(/:/g, "")}`

  return (
    <ChartContext.Provider value={{ config }}>
      <div
        data-slot="chart"
        data-chart={chartId}
        className={cn(
          "[&_.recharts-cartesian-axis-tick_text]:fill-muted-foreground [&_.recharts-cartesian-grid_line[stroke='#ccc']]:stroke-border/50 [&_.recharts-curve.recharts-tooltip-cursor]:stroke-border [&_.recharts-polar-grid_[stroke='#ccc']]:stroke-border [&_.recharts-radial-bar-background-sector]:fill-muted [&_.recharts-rectangle.recharts-tooltip-cursor]:fill-muted [&_.recharts-reference-line_[stroke='#ccc']]:stroke-border flex aspect-video justify-center text-xs [&_.recharts-dot[stroke='#fff']]:stroke-transparent [&_.recharts-layer]:outline-hidden [&_.recharts-sector]:outline-hidden [&_.recharts-sector[stroke='#fff']]:stroke-transparent [&_.recharts-surface]:outline-hidden",
          className
        )}
        {...props}
      >
        <ChartStyle id={chartId} config={config} />
        <RechartsPrimitive.ResponsiveContainer>
          {children}
        </RechartsPrimitive.ResponsiveContainer>
      </div>
    </ChartContext.Provider>
  )
}

const ChartStyle = ({ id, config }: { id: string; config: ChartConfig }) => {
  const colorConfig = Object.entries(config).filter(
    ([, config]) => config.theme || config.color
  )

  if (!colorConfig.length) {
    return null
  }

  return (
    <style
      dangerouslySetInnerHTML={{
        __html: Object.entries(THEMES)
          .map(
            ([theme, prefix]) => `
${prefix} [data-chart=${id}] {
${colorConfig
  .map(([key, itemConfig]) => {
    const color =
      itemConfig.theme?.[theme as keyof typeof itemConfig.theme] ||
      itemConfig.color
    return color ? `  --color-${key}: ${color};` : null
  })
  .join("\n")}
}
`
          )
          .join("\n"),
      }}
    />
  )
}

const ChartTooltip = RechartsPrimitive.Tooltip

function ChartTooltipContent({
  active,
  payload,
  className,
  indicator = "dot",
  hideLabel = false,
  hideIndicator = false,
  label,
  labelFormatter,
  labelClassName,
  formatter,
  color,
  nameKey,
  labelKey,
}: React.ComponentProps<typeof RechartsPrimitive.Tooltip> &
  React.ComponentProps<"div"> & {
    hideLabel?: boolean
    hideIndicator?: boolean
    indicator?: "line" | "dot" | "dashed"
    nameKey?: string
    labelKey?: string
  }) {
  const { config } = useChart()

  const tooltipLabel = React.useMemo(() => {
    if (hideLabel || !payload?.length) {
      return null
    }

    const [item] = payload
    const key = `${labelKey || item?.dataKey || item?.name || "value"}`
    const itemConfig = getPayloadConfigFromPayload(config, item, key)
    const value =
      !labelKey && typeof label === "string"
        ? config[label as keyof typeof config]?.label || label
        : itemConfig?.label

    if (labelFormatter) {
      return (
        <div className={cn("font-medium", labelClassName)}>
          {labelFormatter(value, payload)}
        </div>
      )
    }

    if (!value) {
      return null
    }

    return <div className={cn("font-medium", labelClassName)}>{value}</div>
  }, [
    label,
    labelFormatter,
    payload,
    hideLabel,
    labelClassName,
    config,
    labelKey,
  ])

  if (!active || !payload?.length) {
    return null
  }

  const nestLabel = payload.length === 1 && indicator !== "dot"

  return (
    <div
      className={cn(
        "border-border/50 bg-background grid min-w-[8rem] items-start gap-1.5 rounded-lg border px-2.5 py-1.5 text-xs shadow-xl",
        className
      )}
    >
      {!nestLabel ? tooltipLabel : null}
      <div className="grid gap-1.5">
        {payload
          .filter((item) => item.type !== "none")
          .map((item, index) => {
            const key = `${nameKey || item.name || item.dataKey || "value"}`
            const itemConfig = getPayloadConfigFromPayload(config, item, key)
            const indicatorColor = color || item.payload.fill || item.color

            return (
              <div
                key={item.dataKey}
                className={cn(
                  "[&>svg]:text-muted-foreground flex w-full flex-wrap items-stretch gap-2 [&>svg]:h-2.5 [&>svg]:w-2.5",
                  indicator === "dot" && "items-center"
                )}
              >
                {formatter && item?.value !== undefined && item.name ? (
                  formatter(item.value, item.name, item, index, item.payload)
                ) : (
                  <>
                    {itemConfig?.icon ? (
                      <itemConfig.icon />
                    ) : (
                      !hideIndicator && (
                        <div
                          className={cn(
                            "shrink-0 rounded-[2px] border-(--color-border) bg-(--color-bg)",
                            {
                              "h-2.5 w-2.5": indicator === "dot",
                              "w-1": indicator === "line",
                              "w-0 border-[1.5px] border-dashed bg-transparent":
                                indicator === "dashed",
                              "my-0.5": nestLabel && indicator === "dashed",
                            }
                          )}
                          style={
                            {
                              "--color-bg": indicatorColor,
                              "--color-border": indicatorColor,
                            } as React.CSSProperties
                          }
                        />
                      )
                    )}
                    <div
                      className={cn(
                        "flex flex-1 justify-between leading-none",
                        nestLabel ? "items-end" : "items-center"
                      )}
                    >
                      <div className="grid gap-1.5">
                        {nestLabel ? tooltipLabel : null}
                        <span className="text-muted-foreground">
                          {itemConfig?.label || item.name}
                        </span>
                      </div>
                      {item.value && (
                        <span className="text-foreground font-mono font-medium tabular-nums">
                          {item.value.toLocaleString()}
                        </span>
                      )}
                    </div>
                  </>
                )}
              </div>
            )
          })}
      </div>
    </div>
  )
}

const ChartLegend = RechartsPrimitive.Legend

function ChartLegendContent({
  className,
  hideIcon = false,
  payload,
  verticalAlign = "bottom",
  nameKey,
}: React.ComponentProps<"div"> &
  Pick<RechartsPrimitive.LegendProps, "payload" | "verticalAlign"> & {
    hideIcon?: boolean
    nameKey?: string
  }) {
  const { config } = useChart()

  if (!payload?.length) {
    return null
  }

  return (
    <div
      className={cn(
        "flex items-center justify-center gap-4",
        verticalAlign === "top" ? "pb-3" : "pt-3",
        className
      )}
    >
      {payload
        .filter((item) => item.type !== "none")
        .map((item) => {
          const key = `${nameKey || item.dataKey || "value"}`
          const itemConfig = getPayloadConfigFromPayload(config, item, key)

          return (
            <div
              key={item.value}
              className={cn(
                "[&>svg]:text-muted-foreground flex items-center gap-1.5 [&>svg]:h-3 [&>svg]:w-3"
              )}
            >
              {itemConfig?.icon && !hideIcon ? (
                <itemConfig.icon />
              ) : (
                <div
                  className="h-2 w-2 shrink-0 rounded-[2px]"
                  style={{
                    backgroundColor: item.color,
                  }}
                />
              )}
              {itemConfig?.label}
            </div>
          )
        })}
    </div>
  )
}

// Helper to extract item config from a payload.
function getPayloadConfigFromPayload(
  config: ChartConfig,
  payload: unknown,
  key: string
) {
  if (typeof payload !== "object" || payload === null) {
    return undefined
  }

  const payloadPayload =
    "payload" in payload &&
    typeof payload.payload === "object" &&
    payload.payload !== null
      ? payload.payload
      : undefined

  let configLabelKey: string = key

  if (
    key in payload &&
    typeof payload[key as keyof typeof payload] === "string"
  ) {
    configLabelKey = payload[key as keyof typeof payload] as string
  } else if (
    payloadPayload &&
    key in payloadPayload &&
    typeof payloadPayload[key as keyof typeof payloadPayload] === "string"
  ) {
    configLabelKey = payloadPayload[
      key as keyof typeof payloadPayload
    ] as string
  }

  return configLabelKey in config
    ? config[configLabelKey]
    : config[key as keyof typeof config]
}

export {
  ChartContainer,
  ChartTooltip,
  ChartTooltipContent,
  ChartLegend,
  ChartLegendContent,
  ChartStyle,
}


```
