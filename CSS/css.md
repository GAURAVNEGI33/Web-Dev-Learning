# CSS Learning Journey

This repository contains the CSS concepts I learned while building my resume project. Instead of only learning the theory, I practiced each concept by applying it to a real project.

---

# Day 4 – CSS Basics

## What I Learned

### Introduction to CSS
- CSS is used to style HTML webpages.
- It controls colors, spacing, fonts, layouts, and the overall appearance of a website.

### Ways to Add CSS
I learned three different ways to apply CSS:

- Inline CSS
- Internal CSS
- External CSS

Among these, **External CSS** is the best approach because it keeps the HTML clean and makes the stylesheet reusable.

---

### CSS Selectors

Selectors help target specific HTML elements so styles can be applied only where needed.

I also learned the difference between:

- Element selectors
- Class selectors
- ID selectors

**Classes** can be reused on multiple elements, while **IDs** should always be unique.

---

### Linking CSS

External CSS is linked inside the `<head>` section of the HTML document.

```html
<link rel="stylesheet" href="style.css">
```

---

### Developer Tools

I learned how to inspect a webpage using browser Developer Tools to:

- View applied styles
- Experiment with CSS
- Debug layout issues

---

### Hover Effect

Using the `:hover` pseudo-class, I can change the appearance of an element when the user places the mouse over it.

---

# Day 5 – Applying CSS

Today was less about theory and more about applying CSS to my resume project.

---

## Margin & Padding

I finally understood the difference between the two.

- **Margin** creates space outside an element.
- **Padding** creates space inside an element.

I used both to improve spacing throughout my resume.

---

## Centering Elements

I learned different ways to center elements.

For images, I practiced centering them properly inside their container instead of manually adjusting their position.

---

## Styling My Resume

I transformed my plain HTML resume into a properly designed webpage by adding:

- Better spacing
- Borders
- Hover effects
- Sidebar layout
- Improved typography

This made the project look much cleaner and more professional.

---

## Google Fonts

I learned how to use custom fonts from Google Fonts instead of relying on default browser fonts.

The process involved:

1. Choosing a font.
2. Adding the provided `<link>` tag.
3. Applying the font using `font-family`.

---

## Image Optimization

I learned that large image files slow down page loading.

To improve performance, I:

- Converted images to **WebP**
- Used image dimensions to reduce layout shifts

This helps webpages load faster while maintaining good image quality.

---

# Day 6 – Resume Project

This session focused on building a complete responsive resume using modern CSS.

---

## CSS Reset

Started with a CSS reset to remove browser default spacing.

```css
*{
  margin:0;
  padding:0;
  box-sizing:border-box;
}
```

Using `box-sizing: border-box` makes element sizing much easier because padding and borders are included inside the defined width.

---

## Typography

I used the **Inter** font with a fallback font family.

I also adjusted the line height to improve readability.

---

## Grid Layout

I built the overall page layout using **CSS Grid**.

The layout consists of:

- Fixed-width sidebar
- Flexible main content area

This made the page structured and easier to manage.

---

## Sidebar Design

Inside the sidebar, I learned how to:

- Create circular profile images using `border-radius`
- Prevent image stretching using `object-fit: cover`
- Add custom bullets with `::before`
- Target specific sections using `:nth-of-type()`

---

## Main Content Styling

I styled section headings using the `::after` pseudo-element to create decorative divider lines.

I also learned about the **child combinator (`>`)**, which targets only direct child elements.

---

## Grid Alignment

Inside each experience section, I used Grid again to align:

- Job title
- Date
- Employer details

This created a clean and professional layout.

---

## Responsive Design

Using media queries, I made the resume responsive.

On smaller screens:

- The two-column layout changes into a single-column layout.
- The sidebar moves above the main content.

---

# Important Concepts I Learned

- CSS Reset
- Box Model
- Margin vs Padding
- Classes vs IDs
- CSS Grid
- Flexbox (basic understanding)
- Google Fonts
- Image Optimization
- Responsive Design
- Media Queries
- Pseudo-elements (`::before`, `::after`)
- Child Combinator (`>`)
- `:nth-of-type()`
- `object-fit: cover`
- Hover Effects

---

# Key Takeaways

- CSS is not just about colors; it controls the complete layout of a webpage.
- Understanding the Box Model is essential before building layouts.
- Grid is a great choice for overall page layouts, while Flexbox works well for smaller alignments.
- Writing clean and organized CSS makes projects easier to maintain.
- Responsive design ensures websites work well on different screen sizes.
- Optimizing fonts and images improves both user experience and website performance.

---

# What I Practiced

- Styling an HTML resume from scratch
- Creating responsive layouts
- Using CSS Grid for page structure
- Working with spacing and typography
- Applying hover effects
- Using custom fonts
- Optimizing images
- Building a cleaner and more professional UI