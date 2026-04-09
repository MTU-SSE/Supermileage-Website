# How To Change The Website

**Please Please Please email John (john@umbriac.com) with any questions you may have! He will be happy to help, or even make the change for you!**

Editing the SSE website is simple! As soon as a change is made to a file in this repository on the main branch (what you see by default), that edit will be reflected on the public website. Please keep this in mind. Again,

***EDITS MADE TO FILES HERE WILL SHOW UP ON THE PUBLIC WEBSITE IMMEDIATELY***
(This is currently broken, bug john to fix it sometime soon)

You can edit a file within Github using the built in editor. I recommend this for quick/easy changes like wording or uploading a new picture. Because you can't preview how it will look, you will have to be careful.

You can also clone the repository (download it to your computer) and push the changes back. If you have not used git before, I recommend downloading the Github app, and using their helpful user interface. The advantage of getting the code on your computer is that you can see what your changes will look like before you apply them!

## John you beautiful human, how did you get changes here to automatically apply to the website?

Thank you, and great question! There is a webhook in `public_html` on the computer running the website that gets run everytime a push to this repository occurs. This calls `deploy.sh` which is in `repsitories/Supermileage-Website/` which copies the changes to the folder for the public site.

The "advanced" section of cpanel has a terminal! Very useful :)
You can access cpanel from the link in the social media account passwords page (different from the main passwords page!).

# Supermileage Website Templating System
## Important to know if you need to make more structural changes to the website

This website uses a simple, custom templating system powered by JavaScript to reuse common elements like headers and footers across multiple pages. This guide provides a non-technical overview of how it works.

### How Templating Works

The system relies on two key HTML attributes to function: `data-include` and `slot`.

#### 1. Including Files with `data-include`

You can include the content of one HTML file into another using the `data-include` attribute. The system will find the file specified in the attribute and replace the element with the file's content.

For example, to include a navbar on a page, you can do this:

```html
<header data-include="templates/navbar.html"></header>
```
When the page loads, the `<header>` element will be replaced by the complete HTML content of `templates/navbar.html`.

#### 2. Filling Content with `slots`

Templates can have placeholders called "slots" that you can fill with custom content for each specific page. A placeholder in a template file is marked with a `data-slot="slot-name"` attribute.

To provide content for a slot, you create an element with a matching `slot="slot-name"` attribute inside the `data-include` element.

### Example: Creating a Page Layout

Let's walk through a common use case: creating a main layout and using it on the home page.

**Step 1: Create the Main Layout**

First, we define a main layout in `templates/layout.html`. This file includes the navbar and footer, and defines a `data-slot` named "content" where the unique page content will go.

```html
<!-- templates/layout.html -->

<header data-include="templates/navbar.html"></header>

<main>
  <!-- This is the placeholder for page-specific content -->
  <div data-slot="content">
    <p>If a page doesn't provide content, this default text will appear.</p>
  </div>
</main>

<footer data-include="templates/footer.html"></footer>
```

**Step 2: Use the Layout on a Page**

Now, in our `index.html` file, we can use this layout. We create a `<div>` with the `data-include` attribute pointing to our layout file. Inside this `div`, we add another `div` with the `slot="content"` attribute to provide the unique HTML for the home page.

```html
<!-- index.html -->

<body>
  <!-- Include the main layout -->
  <div data-include="templates/layout.html">

    <!-- Provide the HTML that will fill the "content" slot in the layout -->
    <div slot="content">
      <h1>Welcome to the Home Page!</h1>
      <p>This is the unique content for our index page. It will be placed inside the <main> tag of our layout.</p>
    </div>

  </div>

  <!-- This script runs the templating system -->
  <script src="static/template-loader.js"></script>
</body>
```

### What Happens When the Page Loads?

When `index.html` is opened in a browser:
1. The `template-loader.js` script starts running.
2. It finds `<div data-include="templates/layout.html">`.
3. It fetches the content of `templates/layout.html`.
4. It then looks inside the `data-include` element and finds `<div slot="content">`.
5. It takes the HTML from this "slot" element (the `<h1>` and `<p>` tags).
6. In the fetched layout content, it finds the placeholder `<div data-slot="content">`.
7. It replaces the content of the placeholder with the HTML it saved from the "slot" element.
8. Finally, it replaces the original `<div data-include="templates/layout.html">` with the fully assembled page content.

This process allows us to maintain a consistent structure across the entire site while keeping the content for each page separate and easy to manage.
