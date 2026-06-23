# Bootstrap 5

## What is Bootstrap?

Bootstrap is a CSS framework that helps developers build responsive and modern websites faster.

It provides:

- Ready-made classes
- Responsive grid system
- Ready components like Navbar, Cards, Carousel, Buttons, Forms, Modals, Tabs, and more

Bootstrap helps developers save time instead of writing everything from scratch using CSS and JavaScript.

---

## Bootstrap Version

This reference is based on **Bootstrap v5**.

Bootstrap 5 is different from Bootstrap 4.

Bootstrap 5 uses `ms-*` and `me-*` instead of `ml-*` and `mr-*` because Bootstrap 5 supports RTL layouts better.

**Official Website:** https://getbootstrap.com

---

## 1) Install Bootstrap

### Step 1 — Download Bootstrap

Download Bootstrap files from the official website. You can download the Compiled CSS and JS files.

### Step 2 — Extract the ZIP/RAR file

After downloading, extract the Bootstrap folder.

### Step 3 — Move Files

Copy these files into your project folders:

- `bootstrap.min.css`
- `bootstrap.bundle.min.js`

```
project/
│
├── css/
│   └── bootstrap.min.css
│
├── js/
│   └── bootstrap.bundle.min.js
```

### Step 4 — Link Bootstrap CSS

Add inside `<head>`:

```html
<link rel="stylesheet" href="css/bootstrap.min.css"/>
```

### Step 5 — Link Bootstrap JS

Add before closing `</body>`:

```html
<script src="js/bootstrap.bundle.min.js"></script>
```

---

## 2) Bootstrap Containers

Bootstrap provides containers to control page width and make layouts responsive and organized.

### Types of Containers

**Fixed Container**

```html
<div class="container">...</div>
```

Gives responsive fixed width. The width changes depending on screen size.

**Full Width Container**

```html
<div class="container-fluid">...</div>
```

Takes full width of the screen.

**Responsive Containers**

```html
<div class="container-md">...</div>
<div class="container-lg">...</div>
```

Container becomes fluid before the specified breakpoint.

> **Important:** Most Bootstrap layouts should start with `container` because it gives proper spacing, centers content, and prevents layout problems.
> 

---

## 3) Bootstrap Grid System

Bootstrap divides the page into **12 columns**. Every row contains 12 columns.

```html
col-md-6
```

- `6` = takes 6 columns from total 12
- So the element takes half the row width

### Row and Col

```
container
  -> row
      -> col
```

```html
<div class="container">
  <div class="row">

    <div class="col-md-6">
      <div class="inner bg-primary text-white p-3">
        Left Side
      </div>
    </div>

    <div class="col-md-6">
      <div class="inner bg-dark text-white p-3">
        Right Side
      </div>
    </div>

  </div>
</div>
```

### Important Notes

- `row` already has `display: flex` automatically
- `row` must always be inside `container` to prevent horizontal scrolling
- Always add an inner `div` inside `col` instead of working directly inside it — makes styling and spacing easier

### Bootstrap Breakpoints

| Breakpoint | Size |
| --- | --- |
| sm | ≥576px |
| md | ≥768px |
| lg | ≥992px |
| xl | ≥1200px |
| xxl | ≥1400px |

`col-md-6` means full width on small screens and half width starting from medium screens.

### Offset

```html
<div class="col-md-6 offset-md-2">...</div>
```

Leaves 2 columns empty before the element.
Offset Is Add Margin At The Left Of The Element

### Gutters (Spacing Between Columns)

```html
<div class="row gx-4 gy-4">...</div>
```

- `gx` = horizontal spacing
- `gy` = vertical spacing

---

## 4) Bootstrap Display Utilities

### Basic Display Classes

| Class | Effect |
| --- | --- |
| `d-block` | display: block |
| `d-inline` | display: inline |
| `d-inline-block` | display: inline-block |
| `d-none` | hides element completely |
| `d-flex` | display: flex |
| `d-inline-flex` | display: inline-flex |

### Responsive Display

```html
<!-- Hidden on small, visible from md -->
<div class="d-none d-md-block">...</div>

<!-- Visible on small, hidden from md -->
<div class="d-block d-md-none">...</div>
```

### Flex Direction

```html
<div class="d-flex flex-column flex-md-row">...</div>
```

Column on small screens, row on medium and larger.

### Flex Alignment

```html
justify-content-center
justify-content-between
justify-content-around
justify-content-evenly

align-items-start
align-items-center
align-items-end
```

### Flex Wrap and Gap

```html
<div class="d-flex flex-wrap gap-3">...</div>
```

- `flex-wrap` allows elements to move to the next line
- `gap-1` to `gap-5` adds spacing between flex items

---

## 5) Bootstrap Colors System

### Common Color Names

```
primary   secondary   success
danger    warning     info
light     dark
```

### Text Colors

```html
<p class="text-primary">Primary text</p>
<p class="text-danger">Danger text</p>
```

### Background Colors

```html
<div class="bg-primary">...</div>
<div class="bg-success">...</div>
<div class="bg-dark">...</div>
```

### Text + Background Together

```html
<div class="text-bg-primary">...</div>
```

Adds background color and text color automatically together.

### Bootstrap CSS Variables

Bootstrap 5 uses CSS variables internally:

```css
--bs-primary
--bs-danger
--bs-success
```

These variables help customize Bootstrap colors easily.

---

## 6) Common Bootstrap Utilities

### Overflow

```html
<div class="overflow-auto">...</div>
```

Adds scroll when content overflows.

### Height and Width

```html
<div class="vh-100">...</div>   <!-- Full viewport height -->
<div class="w-25">...</div>     <!-- Width = 25% -->
<div class="h-50">...</div>     <!-- Height = 50% -->
```

### Opacity

```html
opacity-25    opacity-50    opacity-75    opacity-100
```

### Shadows

```html
<div class="shadow">...</div>
<div class="shadow-sm">...</div>
<div class="shadow-lg">...</div>
```

### Object Fit (for images)

```html
<img class="object-fit-cover">
<img class="object-fit-contain">
```

### Text Truncate

```html
<p class="text-truncate">Very long text that will be cut...</p>
```

Cuts long text with `...`

### Border

```html
border          <!-- adds border -->
border-1        <!-- border size -->
border-primary  <!-- border color -->
rounded-circle  <!-- circle shape -->
```

### Padding

| Class | Effect |
| --- | --- |
| `p-3` | padding all directions |
| `px-3` | horizontal padding |
| `py-3` | vertical padding |
| `pt-3` | padding top |
| `pb-3` | padding bottom |
| `ps-3` | padding start (left) |
| `pe-3` | padding end (right) |

Responsive: `p-md-3` applies padding starting from medium screens.

### Margin

Same system as padding: `m-3`, `mx-3`, `my-3`, `ms-3`, `me-3`, etc.

### Position

```html
position-relative
position-absolute
position-fixed
position-sticky
```

**Position Helpers:**

```html
top-50    bottom-50    start-50    end-50
```

**Center Element Perfectly:**

```html
<div class="position-absolute top-50 start-50 translate-middle">...</div>
```

### Z-index

```html
z-0    z-1    z-2    z-3
```

### Text Utilities

```html
text-center      <!-- center text -->
fw-bold          <!-- bold text -->
display-6        <!-- large heading size -->
fs-6             <!-- font size -->
text-uppercase   <!-- uppercase text -->
text-muted       <!-- gray text -->
```

---

## 7) Bootstrap Buttons

```html
<button class="btn btn-primary">Click Me</button>
```

### Button Colors

```html
btn-primary    btn-secondary    btn-success
btn-danger     btn-warning      btn-info
btn-dark       btn-light
```

### Outline Buttons

```html
<button class="btn btn-outline-primary">Click Me</button>
<button class="btn btn-outline-danger">Delete</button>
```

### Button Sizes

```html
<button class="btn btn-primary btn-lg">Large</button>
<button class="btn btn-primary btn-sm">Small</button>
```

### Full Width Button

```html
<div class="d-grid">
  <button class="btn btn-dark">Submit</button>
</div>
```

### Disabled Button

```html
<button class="btn btn-secondary" disabled>Disabled</button>
```

---

## 8) Bootstrap Forms

Forms are one of the most used Bootstrap components.

### Input

```html
<div class="mb-3">
  <label class="form-label">Email address</label>
  <input type="email" class="form-control" placeholder="Enter your email">
</div>
```

### Select

```html
<select class="form-select">
  <option>Open this select menu</option>
  <option>Option 1</option>
  <option>Option 2</option>
</select>
```

### Checkbox

```html
<div class="form-check">
  <input class="form-check-input" type="checkbox" id="rememberMe">
  <label class="form-check-label" for="rememberMe">
    Remember me
  </label>
</div>
```

### Radio

```html
<div class="form-check">
  <input class="form-check-input" type="radio" name="options" id="option1">
  <label class="form-check-label" for="option1">
    Option 1
  </label>
</div>
```

### Input Group

```html
<div class="input-group">
  <span class="input-group-text">@</span>
  <input type="text" class="form-control" placeholder="Username">
</div>
```

### Form Validation

```html
<input type="text" class="form-control is-valid">   <!-- valid state -->
<input type="text" class="form-control is-invalid"> <!-- invalid state -->

<div class="valid-feedback">Looks good!</div>
<div class="invalid-feedback">Please enter a valid value.</div>
```

---

## 9) Bootstrap Navbar

```html
<nav class="navbar navbar-expand-lg bg-body-tertiary">
  <div class="container">

    <a class="navbar-brand" href="#">Logo</a>

    <button
      class="navbar-toggler"
      type="button"
      data-bs-toggle="collapse"
      data-bs-target="#navbarNav">
      <span class="navbar-toggler-icon"></span>
    </button>

    <div class="collapse navbar-collapse" id="navbarNav">
      <ul class="navbar-nav ms-auto">

        <li class="nav-item">
          <a class="nav-link active" href="#">Home</a>
        </li>

        <li class="nav-item">
          <a class="nav-link" href="#">About</a>
        </li>

      </ul>
    </div>

  </div>
</nav>
```

### Important Navbar Notes

- Use `container` instead of `container-fluid` for centered layouts
- Use `ms-auto` to push navbar items to the right
- Dark navbar: add `navbar-dark bg-dark`

---

## 10) Bootstrap Cards

```html
<div class="card shadow">

  <img src="image.jpg" class="card-img-top" alt="Product Image">

  <div class="card-body">
    <h5 class="card-title">Product Title</h5>
    <p class="card-text">Product Description goes here.</p>
    <a href="#" class="btn btn-primary">Read More</a>
  </div>

</div>
```

Cards are used to display products, team members, blog posts, and services.

---

## 11) Bootstrap Carousel

```html
<div id="mainSlider" class="carousel slide" data-bs-ride="carousel">

  <div class="carousel-inner">

    <div class="carousel-item active" data-bs-interval="3000">
      <img src="image1.jpg" class="d-block w-100" alt="Slide 1">
    </div>

    <div class="carousel-item" data-bs-interval="3000">
      <img src="image2.jpg" class="d-block w-100" alt="Slide 2">
    </div>

    <div class="carousel-item" data-bs-interval="3000">
      <img src="image3.jpg" class="d-block w-100" alt="Slide 3">
    </div>

  </div>

  <button class="carousel-control-prev" type="button"
    data-bs-target="#mainSlider" data-bs-slide="prev">
    <span class="carousel-control-prev-icon"></span>
  </button>

  <button class="carousel-control-next" type="button"
    data-bs-target="#mainSlider" data-bs-slide="next">
    <span class="carousel-control-next-icon"></span>
  </button>

</div>
```

### Important Carousel Notes

| Attribute/Class | Purpose |
| --- | --- |
| `carousel-inner` | contains all slides |
| `carousel-item` | represents one slide |
| `active` | defines the first visible slide |
| `data-bs-ride="carousel"` | enables automatic sliding |
| `data-bs-interval="3000"` | controls slide timing (ms) |

---

## 12) Bootstrap Modal

### Button to Open Modal

```html
<button
  class="btn btn-primary"
  data-bs-toggle="modal"
  data-bs-target="#myModal">
  Open Modal
</button>
```

### Modal Structure

```html
<div class="modal fade" id="myModal">
  <div class="modal-dialog modal-dialog-centered">
    <div class="modal-content">

      <div class="modal-header">
        <h5 class="modal-title">Modal Title</h5>
        <button class="btn-close" data-bs-dismiss="modal"></button>
      </div>

      <div class="modal-body">
        Modal body content goes here.
      </div>

      <div class="modal-footer">
        <button class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
        <button class="btn btn-primary">Save</button>
      </div>

    </div>
  </div>
</div>
```

### Modal Options

```html
modal-lg                  <!-- large modal -->
modal-sm                  <!-- small modal -->
modal-dialog-centered     <!-- vertically centered -->
modal-dialog-scrollable   <!-- scrollable when content is long -->
fade                      <!-- smooth open/close animation -->
```

---

## 13) Bootstrap Navs and Tabs

Tabs help switch between content sections without page reload.

```html
<ul class="nav nav-tabs">

  <li class="nav-item">
    <button class="nav-link active"
      data-bs-toggle="tab"
      data-bs-target="#home">
      Home
    </button>
  </li>

  <li class="nav-item">
    <button class="nav-link"
      data-bs-toggle="tab"
      data-bs-target="#profile">
      Profile
    </button>
  </li>

</ul>

<div class="tab-content mt-3">

  <div class="tab-pane fade show active" id="home">
    Home Content
  </div>

  <div class="tab-pane fade" id="profile">
    Profile Content
  </div>

</div>
```

### Important Tabs Notes

| Class/Attribute | Purpose |
| --- | --- |
| `nav-tabs` | creates tab navigation style |
| `tab-content` | wraps all tab content sections |
| `tab-pane` | represents one content section |
| `active` | defines the current visible tab |
| `data-bs-toggle="tab"` | activates Bootstrap tab behavior |

---

## 14) Bootstrap ScrollSpy

ScrollSpy automatically updates active navbar links while scrolling.

### Enable ScrollSpy

```html
<body data-bs-spy="scroll" data-bs-target="#mainNavbar">
```

The navbar must have a matching ID:

```html
<nav id="mainNavbar" class="navbar ...">
```

### Fix Hidden Sections with Fixed Navbar

When using a fixed navbar with ScrollSpy, sections may be hidden behind the navbar. Fix with:

```css
* {
  scroll-padding-top: 70px;
}
```

---

## 15) Bootstrap Icons

Bootstrap has its own icon library, separate from the main Bootstrap files.

**CDN Link** — add inside `<head>`:

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.0/font/bootstrap-icons.css">
```

### Usage

```html
<i class="bi bi-house"></i>
<i class="bi bi-person"></i>
<i class="bi bi-search"></i>
```

### Icons with Buttons

```html
<button class="btn btn-primary">
  <i class="bi bi-download"></i>
  Download
</button>
```

### Resize Icons

Icons follow font size, so use `fs-*` classes:

```html
<i class="bi bi-star fs-1"></i>   <!-- large -->
<i class="bi bi-star fs-5"></i>   <!-- small -->
```

**Full icon list:** https://icons.getbootstrap.com

---

## Important Rule

### Bootstrap is NOT a replacement for CSS

Bootstrap helps speed up development, but to become a strong front-end developer:

- Learn CSS deeply
- Understand Flexbox
- Understand Responsive Design
- Then use Bootstrap as a tool to speed up your workflow