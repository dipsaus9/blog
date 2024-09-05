# From Atoms to Excellence: Elevate Your Design System with Atomic design Principles

Design systems are gaining traction, and rightfully so. A design system elevates your way of working and improves consistency across all applications. So how do you structure your design system properly? By using atomic design principles. In this article, I'll dive into how to do this.

Table of contents:

-   [Setting the Foundation: Importance of Structure in Design systems](#setting-the-foundation-importance-of-structure-in-design-systems)
-   [Understanding Atomic design](#understanding-atomic-design)
-   [Atomic design in Action](#atomic-design-in-action)
-   [From Atoms to Functionality](#from-atoms-to-functionality)
-   [Designing with purpose](#designing-with-purpose)
-   [Atomic design in Perspective](#atomic-design-in-perspective)
-   [The Atomic Way Forward](#the-atomic-way-forward)

## Setting the Foundation: Importance of Structure in Design systems

Building a design system is a bit like planning and constructing a city. Imagine if a city didn't have proper roads, neighborhoods, or parks – it would be confusing and chaotic. Just like in a city, having a good foundation for a design system is incredibly important. It's like the strong base that holds everything together and helps the whole system work well.

Now, think about creating a map for a city that shows where all the roads, buildings, and parks should go. This is similar to what atomic design can do for a design system. atomic design breaks down all the different parts of a design into tiny pieces called "atoms." These atoms are like the building blocks for everything else. When you put these atoms together in a smart way, they create bigger and more complex things, just like how roads and houses make up a city.

The reason we use atomic design in design systems is to make things organized and consistent. Like a well-planned city, it makes sure everything stays organized and works smoothly, even when things grow and change.

## Understanding Atomic design

Just as the natural world is made up of tiny atomic elements that come together to form the building blocks of matter, the design world can follow a similar principle with the use of atomic design. Think of the periodic table in chemistry – it categorizes elements based on their properties and behaviors. In a similar fashion, atomic design categorizes design elements into small, reusable units called atoms.

At its core, atomic design breaks down into five hierarchical levels: atoms, molecules, organisms, templates, and pages. Atoms are the elementary units – the basic building blocks like buttons, labels, or icons. These atoms then combine to form molecules, which are more complex components like forms or cards.

Moving up the hierarchy, we encounter organisms. Organisms are groups of molecules that work together to create sections or functional units within a design, for example a header or a complete form. Templates come next, providing the overarching layout structure for various pages. Finally, pages represent the specific instances where templates are populated with real content.

![Atomic design example](images/from-atoms-to-excellence/atomic-design-process.jpeg)

This organized method goes beyond just putting things into categories. It actually helps to make different parts that can be used again and again, like building blocks. For example, a basic piece like a button can be used in lots of different parts, like small sections and bigger layouts. This keeps everything the same and makes it quicker to build things.

## Designing with purpose

Why should atomic design be on your radar? Atomic design for me, is more than just a design trick; it's a method for teamwork. It's like a universal language that brings designers and developers together, and it's a game-changer for design systems. It's not just about making things look good; it's about making things work better. When you adopt atomic design, you open the door to a lot of benefits for your design systems:

1. **Consistency**: atomic design ensures that every piece of your puzzle will fit together. This means your website or app looks polished and professional.

2. **Scalability**: atomic design grows with you. It's an expandable toolbox. When your project gets bigger, atomic design adapts seamlessly.

3. **Faster Development**: atomic design is a speed booster. By using pre-designed atoms, you can build new components and pages faster.

Atomic design tells our architects and builders (designers and developers) how to create a city that works seamlessly. It’s a structure that helps us build things faster and better. It's like having a master city planner. A city planner lays out the plans for roads, buildings, and parks, atomic design provides a clear structure for designers and developers. This structure ensures that everyone is on the same page. It helps different construction teams to work together efficiently, atomic design streamlines the collaboration between design and development teams.

## Atomic design in Action

So, what's the connection between atomic design and what we do every day?
Well, it's like building with LEGO bricks. We start with tiny pieces, like individual LEGO bricks, and we combine them to make bigger structures. Similarly, atomic design breaks down design into small building blocks called "atoms," which we put together to create larger structures.

Let's take a closer look at how this works. Imagine you have a button – a simple atom. It's like painting a LEGO brick a certain color. No matter where we put this button, its color and size stay the same, just like how a painted LEGO brick looks the same no matter where you use it. Besides color, an atom like this button can have multiple properties. Now, we can use the HTML's border-box model to give this button its own unique style. This model shows how an atom will react as a standalone element. Just like a LEGO brick, an atom can not have properties that will affect other atoms, it’s a standalone element.

![Atomic design example](images/from-atoms-to-excellence/box-model.jpg)

When we style atoms and ensure they look and work the same no matter where they're used, we create a design system that's dependable and simple to use. Think of it like having a collection of neatly arranged LEGO bricks that fit together perfectly.

## From Atoms to Functionality

Let's think about building our town again. Our atoms function as our tiny puzzle pieces. By themselves, they don't make much sense. But when we put them together in specific ways, they form something bigger. When we build a building, we use the pieces we already have. We can't add new, non-existing pieces. It works the same as with atoms – we work with the atoms we have, we can't make any new ones.

Now, imagine molecules are the glue that holds our pieces together. Molecules show how atoms connect and work together, just like streets connect different parts of your town. It’s really important that molecules do not contain behaviors outside of their environment, they only contain the blueprint of the atoms they consume.

For this example we will write a simple breadcrumb molecule that consists of already created atoms, like a breadcrumb item and a breadcrumb separator. The breadcrumb molecule is the glue that holds these atoms together. Before writing our code setup let's write down how our molecule should glue together our existing atoms. I'd like to construct my molecules as a React component that accepts children. This way we can pass in our atoms as children and the molecule will glue them together.

```tsx
import type { ReactNode } from 'react'

type BreadcrumbListProps = {
    children: ReactNode
}

export function BreadcrumbList({ children }: BreadcrumbListProps) {
    return <div className="breadcrumb-list">{children} // This is where our Atoms will be glued together, but how?</div>
}
```

This setup allows “any” React children to be entered. But how do we glue them together? We can use the React.Children API to get all the children and iterate over them. This way we can add our breadcrumb separator between the breadcrumb items.

```tsx
import { cloneElement, Children, type ReactElement, type ReactNode } from 'react'
import clsx from 'clsx'

import { BreadcrumbDivider } from './BreadcrumbDivider'

/**
 * Gets only the valid children of a component,
 * and ignores any nullish or falsy child.
 *
 * @param children the children
 */
function getValidChildren(children: ReactNode) {
    return Children.toArray(children).filter((child) => isValidElement(child)) as ReactElement[]
}

interface BreadcrumbItemProps {
    children: ReactNode
}

export function BreadcrumbItem({ children }: BreadcrumbItemProps) {
    const validChildren = getValidChildren(children)
    const totalItems = validChildren.length

    const clones = validChildren.map((child, index) =>
        cloneElement(child, {
            children: (
                <>
                    {child?.props?.children || null}
                    {index < totalItems - 1 && <BreadcrumbDivider />}
                </>
            ),
            className: clsx('some-custom-breadcrumb-item-styling', child?.props?.className) // Here we can provide extra properties as glue to our atoms
        })
    )

    return <div className="breadcrumb-item">{clones}</div>
}
```

With this setup we still allow our consumers to provide extra properties to our atoms, so we do not limit them. This is very important because we do not want to limit our consumers in their creativity. We only want to provide a blueprint of how our atoms should be glued together. This way we can ensure that our atoms are used in a way that makes sense and works smoothly. This is how I create molecules from atoms.

If we want to use our breadcrumb molecule we can do it as follows:

```tsx
import { BreadcrumbList } from './BreadcrumbList'
import { BreadcrumbItem } from './BreadcrumbItem'

function BreadcrumbsView() {
    return (
        <BreadcrumbList>
            <BreadcrumbItem>
                <a href="/">Home</a>
            </BreadcrumbItem>
            <BreadcrumbItem>
                <a href="/about">About</a>
            </BreadcrumbItem>
            <BreadcrumbItem>
                <a href="/contact">Contact</a>
            </BreadcrumbItem>
        </BreadcrumbList>
    )
}
```

## Atomic design in Perspective

Now, let's consider an alternative to atomic design – the monolithic design. In a monolithic design, the focus is on building a single, comprehensive structure, like a mansion. In this approach, everything is bundled together into one unit. In this mansion you can have different rooms that serve different purposes, like a living area, a bedroom, and a kitchen. But they're all under the same roof. This means that if you want to change something in one room, you have to change it in all the other rooms too. Monolithic design forms a unity, but it also creates lots of dependencies.

Both approaches have their strengths and are suited for different contexts in the world of design and development. Design systems for large organizations are best suited with the atomic design method. It's a more scalable approach that allows for flexibility and collaboration, one of the key aspects of a design system. Keep in mind that the monolithic design is not a bad approach, it serves a different purpose. Most organizations are not large enough to benefit from the atomic design method. The monolithic design is a great approach for smaller organizations that do not have the need for a design system.

## The Atomic Way Forward - Conclusion

As we wrap up our exploration of design systems, let's highlight why atomic design should be your go-to approach. Atomic design is not just a design methodology; it's a design philosophy. It's a compass guiding us toward a more efficient, consistent, and user-focused future in design and development. By breaking components into smaller pieces, atomic design gives you more structure in your design and development process. By also applying the same philosophy in your development process, you can create more consistent code quality.

I encourage you to give atomic design a try in your own projects. It's not just a design method; it's a smart way to give structure. Take the leap into the atomic era, and let's shape a better digital future together.
