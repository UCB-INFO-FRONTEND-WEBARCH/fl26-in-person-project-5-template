# Week 7: Migrating Your Interactive Portfolio to Vite + React
## From Vanilla JavaScript to React - The First Step

### Learning Objectives
By completing this project, you will:
- Set up a modern React development environment using Vite
- Convert HTML markup to JSX syntax (className, self-closing tags, fragments)
- Migrate vanilla JavaScript event handlers into React component methods
- Understand the monolithic component pattern as a stepping stone to React
- Import and use CSS in a React application
- Successfully run a React development server and see your portfolio in React

### Prerequisites
Before starting, you should have:
- Completed Week 7 lecture on React Introduction
- Reviewed prep work chapters: React Front To Back Chapters 1-2 (Intro & JSX)
- Your code from Week 6 Interactive Portfolio project (highly recommended)
- Basic understanding of npm and command line tools

### Project Overview
This week, you'll take your interactive portfolio from Week 6 and migrate it into a React application using Vite. Rather than breaking it into multiple components (that comes in Week 8), you'll wrap ALL your HTML in ONE giant App.jsx component and move ALL your JavaScript logic inside that component. This monolithic approach helps you understand how React works before learning component decomposition.

**What you're migrating:**
- Week 6 Interactive Portfolio with smooth scroll navigation, project filtering, form validation, mobile menu, and skill animations
- All HTML becomes JSX in a single App component
- All JavaScript event listeners become React event handlers
- All CSS imports as a module in React

### Getting Started

#### First: Create your own copy of this template
This repository is a GitHub **template** (GitHub Classroom is not used this term). Make your own copy of it, then clone that copy.

1. On this repository's page, click the green **Use this template** button and choose **Create a new repository**. On the next screen: Owner = your own GitHub account, Repository name = `in-person-project-5-<your-github-username>`, visibility **Public**. Click **Create repository**.

2. **Clone your new repository:**
   ```bash
   git clone https://github.com/<your-github-username>/in-person-project-5-<your-github-username>.git
   cd in-person-project-5-<your-github-username>
   ```

#### Second: Install and run (start this now)
Unlike Week 6, this project needs Node and an install step before anything shows in the browser. Start the install first; on classroom wifi it can take a few minutes, and you can read the rest of this page while it runs.

1. **Check Node:** `node -v` must print `v18` or newer. If it prints an older version or "command not found", install the current LTS from https://nodejs.org, then open a new terminal.
2. **Install dependencies** (once per clone):
   ```bash
   npm install
   ```
3. **Start the development server:**
   ```bash
   npm run dev
   ```
4. **Open your browser** to the URL shown in the terminal (usually `http://localhost:5173`). You should see the starter with its TODO comments and no content yet. Keep this terminal running; Vite reloads the page every time you save.

#### Third: Choose your path

#### Option A: Continue from Week 6 (Recommended)
If you completed the Week 6 project and want to migrate your own code:

1. **Keep the starter files.** Do not delete `package.json`, `vite.config.js`, `index.html` or anything in `src/`.
2. **Copy your Week 6 files into this repository** so they sit beside the React code and you can open both side by side in your editor:
   ```bash
   # from inside your cloned repository
   mkdir my-week6
   cp /path/to/your/week6/index.html my-week6/
   cp /path/to/your/week6/styles.css my-week6/
   cp /path/to/your/week6/script.js  my-week6/
   ```
   These three files are only a reference for copying from; Vite ignores them.
3. **Follow the migration steps** in Part 2 below. Everywhere a step says "your Week 6 file", use the file in `my-week6/`.

#### Option B: Fresh Start
If you did not finish Week 6, migrate the class solution instead. It is already in this repository under `week6-reference/` (`index.html`, `styles.css`, `script.js`): a complete Week 6 Interactive Portfolio with smooth scroll, project filtering, a mobile menu, skill bars and form validation. Option B means migrating that folder: everywhere a step says "your Week 6 file", use the matching file in `week6-reference/`. The steps are identical for both paths.

### Step-by-Step Instructions

**Today's core is Parts 1 to 3** (setup, JSX, CSS): finish those first. Parts 4 and 5 (JavaScript migration, testing) are next if you have time, and the Extension Activities come after that.

#### Part 1: Understanding the Vite React Setup (10 minutes)

**What is Vite?**
Vite (French for "fast") is a modern build tool that provides a faster development experience than Create React App. It uses native ES modules and provides instant hot module replacement.

**Explore the starter structure:**
```
week7-starter/
├── index.html          # Entry point (minimal, only loads main.jsx)
├── package.json        # Dependencies and scripts
├── vite.config.js      # Vite configuration
├── src/
│   ├── main.jsx        # React initialization: ReactDOM.createRoot(...).render(<App />)
│   ├── App.jsx         # Your main component (TODO comments only, for now)
│   ├── App.css         # Your styles (Week 6 styles.css will go here)
│   └── index.css       # Base styles loaded by main.jsx; leave it alone
```

**Key differences from Week 6:**
- **index.html**: Much simpler! It only loads the JavaScript
- **main.jsx**: Creates the React root with `createRoot` and renders the App component into the DOM
- **App.jsx**: Where ALL your HTML and JavaScript will live (for now)
- **App.css**: Where your CSS will live

**Try it out:**
1. Run `npm run dev` and open the browser
2. Make a change to `App.jsx` (add some text)
3. Save and watch it update instantly!

What's happening here:
- Vite bundles your React code in real-time
- React renders the App component into the `<div id="root">` in index.html
- Hot Module Replacement (HMR) updates the page without full reload

**Checkpoint (end of Part 1):** `npm run dev` is running, the browser shows the starter, and a text change in `App.jsx` appears without a manual reload.

#### Part 2: Converting HTML to JSX (20 minutes)

**JSX Rules to Remember** (these are the ones that break a pasted Week 6 page):
1. `class` becomes `className`
2. `for` on a `<label>` becomes `htmlFor`
3. All tags must be closed (`<img>` becomes `<img />`; the same for `<input>`, `<br>` and `<hr>`)
4. Style attributes use objects: `style={{ color: 'red' }}`
5. Comments use `{/* comment */}` syntax. An HTML comment `<!-- ... -->` is a syntax error in JSX, so convert or delete every one before you save
6. One root element (use fragments `<>...</>` if needed)
7. Attribute names are camelCase: `onclick` becomes `onClick`, `onsubmit` becomes `onSubmit`, `tabindex` becomes `tabIndex`, `maxlength` becomes `maxLength`, `autocomplete` becomes `autoComplete`
8. Remove any `<script>` tag from the pasted markup; the JavaScript moves into the component in Part 4

**Step 1: Copy your Week 6 HTML structure**

Open your Week 6 `index.html` and copy everything INSIDE the `<body>` tag (but not the `<body>` tag itself).

Paste it into `App.jsx` inside the return statement:

```jsx
function App() {
  return (
    <>
      {/* Paste your Week 6 HTML here */}
    </>
  )
}
```

**Step 2: Convert class to className**

Find and replace ALL instances:
- `class="` → `className="`

**Step 3: Fix self-closing tags**

Look for tags without closing tags and add ` />`:
- `<img src="...">` → `<img src="..." />`
- `<input type="text">` → `<input type="text" />`

**Step 4: Handle inline styles**

If you have any inline styles (like in skill progress bars), convert them:

Week 6 HTML:
```html
<div class="skill-progress" style="--skill-level: 90%"></div>
```

React JSX:
```jsx
<div className="skill-progress" style={{ '--skill-level': '90%' }}></div>
```

**Step 5: Test your JSX**

Save the file and check the browser. You should see:
- Your portfolio structure (but no styles yet)
- Any JSX errors in the console

Common mistakes to avoid:
- Forgetting to wrap multiple elements in a fragment `<>...</>`
- Missing closing tags
- Using `class` instead of `className`, or `for` instead of `htmlFor`
- Leaving an HTML comment `<!-- -->` or a `<script>` tag in the pasted markup

**Checkpoint (end of Part 2):** the page renders your whole portfolio as unstyled text and images, and the browser console shows no red JSX error.

#### Part 3: Importing and Using CSS (10 minutes)

**Step 1: Copy your Week 6 CSS**

Copy the entire contents of your Week 6 `styles.css` file.

**Step 2: Paste into App.css**

Open `src/App.css` and replace everything with your Week 6 CSS.

**Step 3: Import CSS in App.jsx**

At the top of `App.jsx`, add:
```jsx
import './App.css'
```

**Step 4: Check the browser**

Save and your portfolio should now be styled! If something looks off:
- Check that all `class` attributes are now `className`
- Verify CSS selectors match your JSX
- Look for browser console errors

**Checkpoint (end of Part 3):** the page looks the same as your Week 6 portfolio, including the skill bars (they fill from the `--skill-level` value you set in Part 2). Nothing is clickable yet; that is Part 4.

#### Part 4: Migrating JavaScript Logic (25 minutes)

This is where it gets interesting! You'll move ALL your Week 6 JavaScript into the App component.

**Understanding React's approach:**
- Vanilla JS: Select elements, attach event listeners
- React: Use event attributes directly on JSX elements

**Step 1: Remove script tags**

In your JSX, remove any `<script>` tags - we'll handle this in React.

**Step 2: Convert navigation smooth scroll**

Week 6 JavaScript:
```javascript
const navLinks = document.querySelectorAll('.nav-link');
navLinks.forEach(link => {
    link.addEventListener('click', (e) => {
        e.preventDefault();
        // smooth scroll logic
    });
});
```

React approach (inside App component):
```jsx
function App() {
  // Event handler function
  const handleNavClick = (e) => {
    e.preventDefault();
    const targetId = e.currentTarget.getAttribute('href');
    const targetSection = document.querySelector(targetId);
    if (targetSection) {
      targetSection.scrollIntoView({ behavior: 'smooth', block: 'start' });
    }
  };

  return (
    <>
      <nav id="navbar">
        <ul className="nav-menu">
          <li><a href="#home" className="nav-link" onClick={handleNavClick}>Home</a></li>
          <li><a href="#about" className="nav-link" onClick={handleNavClick}>About</a></li>
          {/* etc */}
        </ul>
      </nav>
      {/* rest of JSX */}
    </>
  )
}
```

Key differences:
- No `querySelectorAll` needed
- `onClick` attribute directly on the element
- Event handler is a function in the component

**Step 3: Convert project filtering**

Week 6 JavaScript:
```javascript
const filterButtons = document.querySelectorAll('.filter-btn');
filterButtons.forEach(button => {
    button.addEventListener('click', () => {
        // filtering logic
    });
});
```

React approach:
```jsx
function App() {
  const handleFilterClick = (e) => {
    const filterValue = e.currentTarget.getAttribute('data-filter');
    const projectCards = document.querySelectorAll('.project-card');

    projectCards.forEach(card => {
      const cardCategory = card.getAttribute('data-category');
      if (filterValue === 'all' || cardCategory === filterValue) {
        card.style.display = 'block';
        setTimeout(() => {
          card.style.opacity = '1';
          card.style.transform = 'scale(1)';
        }, 10);
      } else {
        card.style.opacity = '0';
        card.style.transform = 'scale(0.9)';
        setTimeout(() => {
          card.style.display = 'none';
        }, 300);
      }
    });

    // Update active button
    document.querySelectorAll('.filter-btn').forEach(btn =>
      btn.classList.remove('active')
    );
    e.currentTarget.classList.add('active');
  };

  return (
    <>
      {/* navigation */}
      <div className="filter-buttons">
        <button className="filter-btn active" data-filter="all" onClick={handleFilterClick}>
          All Projects
        </button>
        <button className="filter-btn" data-filter="frontend" onClick={handleFilterClick}>
          Frontend
        </button>
        {/* etc */}
      </div>
      {/* rest of JSX */}
    </>
  )
}
```

Note: Yes, we're still using DOM manipulation here! In Week 8, you'll learn to do this the "React way" with state.

**Step 4: Convert form validation**

Week 6 JavaScript:
```javascript
const contactForm = document.querySelector('#contact-form');
contactForm.addEventListener('submit', (e) => {
    e.preventDefault();
    // validation logic
});
```

React approach:
```jsx
function App() {
  const handleFormSubmit = (e) => {
    e.preventDefault();

    const nameInput = document.querySelector('#name');
    const emailInput = document.querySelector('#email');
    const messageInput = document.querySelector('#message');

    // Your validation logic here
    const nameValue = nameInput.value.trim();
    const emailValue = emailInput.value.trim();
    const messageValue = messageInput.value.trim();

    let isValid = true;

    if (nameValue.length < 2) {
      // show error
      isValid = false;
    }

    // etc - all your Week 6 validation logic

    if (isValid) {
      console.log('Form submitted!');
      // Show success message
    }
  };

  return (
    <>
      {/* navigation and other sections */}
      <form id="contact-form" className="contact-form" onSubmit={handleFormSubmit}>
        <div className="form-group">
          <label htmlFor="name">Name *</label>
          <input type="text" id="name" name="name" required />
        </div>
        {/* other form fields */}
        <button type="submit" className="btn btn-primary">Send Message</button>
      </form>
      {/* rest of JSX */}
    </>
  )
}
```

Note: `for` attribute becomes `htmlFor` in React!

**Step 5: Add the window scroll listener (copy this block now; `useEffect` is explained on Oct 19)**

Active nav highlighting needs a listener on `window`, not on an element. In React you must not call `window.addEventListener` directly in the component body: React runs that body on every render, so each render would add another listener (and StrictMode doubles it in development). The React tool for "do this once when the component loads, and undo it when it goes away" is `useEffect`. Copy the block as written; Week 8 covers what it does.

```jsx
import { useEffect } from 'react'
import './App.css'

function App() {
  // Event handlers
  const handleNavClick = (e) => { /* ... */ };
  const handleFilterClick = (e) => { /* ... */ };
  const handleFormSubmit = (e) => { /* ... */ };

  // Scroll handler for active nav highlighting
  const handleScroll = () => {
    const sections = document.querySelectorAll('section');
    const scrollPos = window.scrollY + 100;

    sections.forEach(section => {
      const sectionTop = section.offsetTop;
      const sectionHeight = section.offsetHeight;
      const sectionId = section.getAttribute('id');

      if (scrollPos >= sectionTop && scrollPos < sectionTop + sectionHeight) {
        document.querySelectorAll('.nav-link').forEach(link =>
          link.classList.remove('active')
        );
        const activeLink = document.querySelector(`.nav-link[href="#${sectionId}"]`);
        if (activeLink) {
          activeLink.classList.add('active');
        }
      }
    });
  };

  // Copy this block as is: attach the listener once on load, remove it on unload
  useEffect(() => {
    window.addEventListener('scroll', handleScroll);
    return () => window.removeEventListener('scroll', handleScroll);
  }, []);

  return (
    <>
      {/* All your JSX */}
    </>
  )
}
```

**Skill bars:** in Week 6 the bars animated when you scrolled to the skills section. Today they do not need any JavaScript: the `--skill-level` value you set in Part 2 Step 4 drives the CSS `width` and the fill animation, so leave the Week 6 `animateSkills` code out. (Tier 1 of the Extension Activities turns the skills into data.)

**Step 6: Mobile menu toggle**

```jsx
function App() {
  const handleMobileMenuToggle = () => {
    const navMenu = document.querySelector('.nav-menu');
    const navToggle = document.querySelector('.nav-toggle');
    navMenu.classList.toggle('active');
    navToggle.classList.toggle('active');
  };

  return (
    <>
      <nav id="navbar">
        <div className="nav-brand">
          <span>Alex Johnson</span>
        </div>
        <ul className="nav-menu">
          {/* nav items */}
        </ul>
        <div className="nav-toggle" onClick={handleMobileMenuToggle}>
          <span></span>
          <span></span>
          <span></span>
        </div>
      </nav>
      {/* rest of JSX */}
    </>
  )
}
```

What's happening here:
- All event handler functions are defined inside the App component
- They have access to each other and can be used throughout the JSX
- We're still using DOM manipulation (document.querySelector) - this is OK for Week 7!
- Next week, you'll learn the React way using state and props

Common mistakes to avoid:
- Calling the function instead of passing it: `onClick={handleClick()}` (wrong) vs `onClick={handleClick}` (right)
- Forgetting to use `e.preventDefault()` on form submissions and navigation links
- Calling `window.addEventListener` in the component body instead of inside the `useEffect` block from Step 5

**Checkpoint (end of Part 4):** nav links scroll smoothly, the filter buttons show and hide cards, the mobile menu opens and closes at a narrow width, the form refuses an empty name, and the active nav link changes as you scroll.

#### Part 5: Testing and Debugging (15 minutes)

**Step 1: Test each feature**

Go through each interactive feature:
- [ ] Smooth scroll navigation works
- [ ] Active nav link updates on scroll
- [ ] Project filter buttons work
- [ ] Mobile menu toggle works
- [ ] Skill bars show their widths
- [ ] Form validation works
- [ ] Form submission shows the success message (if your Week 6 code had one)

**Step 2: Check for console errors**

Open browser DevTools (F12) and look for:
- Red error messages
- JSX syntax errors
- Event handler issues

**Step 3: Common issues and fixes**

| Issue | Cause | Fix |
|-------|-------|-----|
| Nothing shows up | JSX syntax error | Check console, fix unclosed tags or className issues |
| Styles not applied | CSS not imported | Add `import './App.css'` at top of App.jsx |
| Events don't work | Wrong event syntax | Use `onClick` not `onclick`, pass function reference not call |
| Form doesn't prevent default | Missing preventDefault | Add `e.preventDefault()` in form handler |

**Step 4: Compare with Week 6**

Open your Week 6 portfolio in another tab and compare:
- Does the layout look the same?
- Do all interactions work identically?
- Are animations smooth?

**Checkpoint (end of Part 5):** every box in Step 1 is ticked and the console is clean. Commit and push now (see Submit below), then continue with the Extension Activities.

### Understanding the Monolithic Component Pattern

**Why start with one giant component?**

You might wonder: "Isn't this messy? Why put everything in one component?"

Great question! Here's why:
1. **Learning progression**: Understanding how React handles events before learning component decomposition
2. **Migration strategy**: Easier to migrate everything first, then refactor
3. **Seeing the differences**: Clearly see what's different from vanilla JS
4. **Preparation for Week 8**: Next week you'll break this apart into logical components

**What makes this different from Week 6?**

Even though we're still using some DOM manipulation:
- We're using JSX instead of HTML
- We're using React's event system
- We're importing CSS as a module
- We're using a React development server with HMR
- We're setting up for proper React patterns (coming in Week 8)

**Preview of Week 8:**

Next week, you'll learn to:
- Break App.jsx into multiple components (Navbar, Hero, Skills, Projects, Contact, Footer)
- Use props to pass data between components
- Manage state properly instead of DOM manipulation
- Create reusable, modular component architecture

### Customization Ideas

Once everything works, try these enhancements:

1. **Update the content**
   - Change name and title in the hero section
   - Add your own projects with real images
   - Update skills to match your experience
   - Customize colors in CSS variables

2. **Add console logs**
   - Log when event handlers fire
   - Log form data before submission
   - See React's event system in action

3. **Experiment with JSX**
   - Try adding new sections
   - Practice conditional rendering with ternary operators
   - Map over arrays to render lists

### Extension Activities (requirements and hints only, no code given)

If you finish the five parts early, these are not bonus busywork. They are the same kind of thinking you did in Parts 1 to 5, aimed at the next step React asks of you, and there is no code to copy: each task gives you the requirement, what "done" looks like, one hint, and why it matters. Everything is doable with today's tools (JSX, `className`, `onClick`, `.map()` from Week 6, and for Tier 2 one `useState`, which the lecture previewed). Work through the tiers in order.

#### Tier 1 (about 10 to 15 minutes): do this one

**1. Skills as data**
- **Goal:** Replace the six hard-coded skill blocks in your JSX with one `const skills = [...]` array at the top of `App.jsx` and a `.map()` that renders them.
- **Done when:** the same six bars render with the same names and widths; the console shows no "key" warning; changing one entry in the array (a name or a percentage) changes the page after you save; adding a seventh entry adds a seventh bar with no other edit.
- **Hint:** Each array entry holds exactly what one block needs (a name and a level). `.map()` returns one JSX block per entry, and React wants a `key` on the outer element of each one; the skill name is a fine key. The `--skill-level` value becomes `style={{ '--skill-level': skill.level }}`.
- **Why it matters:** rendering a list from data instead of copying markup is the first thing React is for, and Assignment 2's task list is exactly this shape.

#### Tier 2 (about 15 to 20 minutes): for the student who finished Tier 1

**2. Menu open state, the React way**
- **Goal:** Replace the `classList.toggle` mobile menu from Part 4 Step 6 with one piece of state: `const [menuOpen, setMenuOpen] = useState(false)` and a className that follows it.
- **Done when:** the menu opens and closes at a narrow width exactly as before; the toggle handler contains no `document.querySelector`; the `active` class on `.nav-menu` and `.nav-toggle` is written as a ternary in the JSX; smooth scroll and the filter buttons still work.
- **Ceiling, stop here:** only the menu moves to state. Leave the filter buttons and the form as they are; those come next week.
- **Hint:** `import { useState } from 'react'` next to `useEffect`. The handler becomes `() => setMenuOpen(!menuOpen)`, and the element reads `className={menuOpen ? 'nav-menu active' : 'nav-menu'}`. You never touch the DOM; you change the value and React changes the class.
- **Why it matters:** this is the pattern Assignment 2 is built on (state in, className out), and you will have done it once before the Oct 19 class teaches it.

#### Tier 3 (about 15 to 25 minutes, a bounded task): an Assignment 2 rehearsal

**3. A fresh Vite app with a list of task components**
- **Goal:** In a scratch folder outside this repository, run `npm create vite@latest rehearsal -- --template react`, then render a hard-coded `tasks` array of three objects (`id`, `text`, `completed`) through a `TaskItem` component that receives one task as props.
- **Done when:** `npm run dev` shows the three tasks; `TaskItem` lives in its own file under `src/components/` and takes `task` as a prop; the console shows no "key" warning; `npm run build` finishes without an error.
- **Ceiling, stop here:** no state, no form, no buttons. Hard-coded data and props only.
- **Hint:** The fresh scaffold is not identical to today's starter: look at what `main.jsx`, `App.jsx` and `index.css` contain before you delete anything, and note that the default `index.css` centers the page. A component is a function that returns JSX; the parent passes `task={t}` and the child reads `props.task.text`.
- **Why it matters:** Assignment 2 starts from this exact scaffold, and the differences from today's starter are better met on Monday afternoon than at 11 PM on a deadline.

### Troubleshooting Guide

**Problem:** `node -v` prints an old version, or "command not found"

**Solution:**
- Vite needs Node 18 or newer. Install the current LTS from https://nodejs.org, close the terminal, open a new one, run `node -v` again.
- On a lab machine where you cannot install, ask for help before class ends rather than losing the hour.

---

**Problem:** `npm install` is taking forever

**Solution:**
- On classroom wifi it can take several minutes. Leave it running and read Part 2 while you wait.
- If it stops with a network error, run `npm install` again; it resumes from the cache.

---

**Problem:** Vite dev server won't start

**Solution:**
```bash
# macOS / Linux / Git Bash
rm -rf node_modules package-lock.json
npm install
npm run dev
```
```powershell
# Windows PowerShell
Remove-Item -Recurse -Force node_modules, package-lock.json
npm install
npm run dev
```

---

**Problem:** "Port 5173 is in use"

**Solution:**
- You already have a dev server running in another terminal (often the one you forgot about). Stop it with Ctrl+C there, or let Vite use the next port it prints (5174) and open that URL instead.

---

**Problem:** The page is blank and the console shows a JSX error

**Solution:**
- Read the first error line; it names the line in `App.jsx`. The usual causes: an HTML comment `<!-- -->` left in the markup, a `<script>` tag, an attribute in the wrong case (`onclick`, `tabindex`, `maxlength`, `autocomplete`), `for` instead of `htmlFor`, `class` instead of `className`, or an unclosed `<img>`, `<input>` or `<br>`.
- Fix the first error only, save, read the next one.

---

**Problem:** "React is not defined" error

**Solution:**
- This starter uses the automatic JSX runtime, so `App.jsx` does not need `import React from 'react'`. If you see this error, you copied code from an older tutorial that calls `React.something` directly; either delete that call or add `import React from 'react'` at the top of the file that uses it.

---

**Problem:** CSS not loading

**Solution:**
- Check `import './App.css'` is at top of App.jsx
- Verify App.css is in the src folder
- Check browser Network tab to see if CSS loaded

---

**Problem:** onClick events not firing

**Solution:**
- Use `onClick` not `onclick` (capital C)
- Pass function reference: `onClick={handleClick}` not `onClick={handleClick()}`
- Check console for JavaScript errors

---

**Problem:** Smooth scroll not working

**Solution:**
- Verify `e.preventDefault()` is called in nav click handler
- Check that target sections have matching id attributes
- Ensure CSS for scroll-behavior isn't conflicting

---

**Problem:** The active nav link flickers or scrolling feels slow

**Solution:**
- You attached the scroll listener in the component body instead of inside the `useEffect` block from Part 4 Step 5, so every render adds another listener. Move it into `useEffect` and reload the page once.

---

**Problem:** Form validation not showing errors

**Solution:**
- Check that error elements are being created and inserted
- Verify CSS styles for `.error-message` class exist
- Use browser DevTools to inspect DOM and see if error elements are present

### Resources & References

**Official Documentation:**
- [Vite Documentation](https://vitejs.dev/)
- [React Documentation](https://react.dev/)
- [JSX Introduction](https://react.dev/learn/writing-markup-with-jsx)

**Week 7 Prep Work:**
- React Front To Back Chapter 1 - Introduction
- React Front To Back Chapter 2 - React Basics and JSX

**Related Week Projects:**
- Week 6: Interactive Portfolio (JavaScript functionality you're migrating)
- Week 8: Component-Based Portfolio (where you'll decompose this monolith)

**Tools:**
- [React Developer Tools](https://react.dev/learn/react-developer-tools) - Browser extension
- [Vite Guide](https://vitejs.dev/guide/) - Understanding the build tool
- [JSX Transformer](https://transform.tools/html-to-jsx) - Quick HTML to JSX conversion

### Submit

1. Commit and push your work before you leave (the `.gitignore` in this repository keeps `node_modules` out of the push, so it is quick):
   ```bash
   git add .
   git commit -m "In-person project 5"
   git push
   ```
2. On the bCourses assignment page click **Submit Assignment** and paste the URL of your repository (the repository page, not a file) as the Website URL.
3. Grading is complete/incomplete: a pushed repository that runs with `npm run dev`, shows the styled portfolio, and whose navigation scrolls and filter buttons work (Parts 1 to 4, Steps 2 and 3) earns complete.

### Project Checklist

Before submitting, ensure you have:

**Setup & Configuration:**
- [ ] Vite project runs with `npm run dev`
- [ ] No console errors when page loads
- [ ] Hot Module Replacement (HMR) works when you save files

**HTML to JSX Conversion:**
- [ ] All `class` attributes converted to `className`
- [ ] All self-closing tags have ` />` syntax
- [ ] No HTML comments (only `{/* JSX comments */}`)
- [ ] All inline styles converted to object syntax

**CSS Import:**
- [ ] `import './App.css'` at top of App.jsx
- [ ] All styles from Week 6 present and working
- [ ] Page looks identical to Week 6 portfolio

**JavaScript Migration:**
- [ ] Smooth scroll navigation works on nav link clicks
- [ ] Active nav link updates based on scroll position (the `useEffect` block from Part 4 Step 5)
- [ ] Project filter buttons work and show correct projects
- [ ] Mobile menu toggle opens/closes navigation at a narrow width
- [ ] Skill bars fill to their `--skill-level` widths
- [ ] Form validation works on all required fields
- [ ] Form submission shows the success message (if your Week 6 code had one)

**Code Quality:**
- [ ] All event handlers defined inside App component
- [ ] `onClick`, `onSubmit` used instead of addEventListener
- [ ] `e.preventDefault()` called where needed
- [ ] No unused code or commented-out blocks
- [ ] Code is readable with proper indentation

**Functionality:**
- [ ] All Week 6 features work identically
- [ ] No broken links or images (placeholder images count as working)
- [ ] Responsive design works on mobile
- [ ] Scrolling is smooth (no duplicate scroll listeners)

**Bonus (Optional):**
- [ ] Added personal customization
- [ ] Finished Tier 1 of the Extension Activities (skills as data)
- [ ] Tried Tier 2 or Tier 3

### Reflection Questions

After completing the project, consider:

1. **What surprised you about JSX?**
   - How is it different from HTML?
   - What advantages does it provide?

2. **How is React's event system different?**
   - Compare `addEventListener` vs `onClick`
   - When would you use each?

3. **Why use a build tool like Vite?**
   - What does it provide that vanilla JS doesn't?
   - How does HMR improve development?

4. **What feels messy about the monolithic component?**
   - What would you want to break into separate components?
   - How might you organize it differently?

Keep these thoughts in mind for Week 8 when you'll learn component decomposition!

---

**Congratulations!** You've successfully migrated your portfolio to React. This is a huge step toward modern web development. In Week 8, you'll learn to organize this code into clean, reusable components. But for now, celebrate turning your vanilla JavaScript portfolio into a React application!
