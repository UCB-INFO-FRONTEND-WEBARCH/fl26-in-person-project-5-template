# Week 8: Migrating Your Interactive Portfolio to Vite + React
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
- Completed Week 8 lecture on React Introduction
- Reviewed prep work chapters: React Front To Back Chapters 1-2 (Intro & JSX)
- Your code from Week 7 Interactive Portfolio project (highly recommended)
- Basic understanding of npm and command line tools

### Project Overview
This week, you'll take your interactive portfolio from Week 7 and migrate it into a React application using Vite. Rather than breaking it into multiple 1components (that comes in Week 9), you'll wrap ALL your HTML in ONE giant App.jsx component and move ALL your JavaScript logic inside that component. This monolithic approach helps you understand how React works before learning component decomposition.

**What you're migrating:**
- Week 7 Interactive Portfolio with smooth scroll navigation, project filtering, form validation, mobile menu, and skill animations
- All HTML becomes JSX in a single App component
- All JavaScript event listeners become React event handlers
- All CSS imports as a module in React

### Getting Started

**First:** Accept the GitHub Classroom assignment link provided by your instructor. This will create a new repository with the Week 8 starter code.

1. **Clone your new repository:**
   ```bash
   git clone [your-github-classroom-repo-url]
   cd [your-repo-name]
   ```

2. **Choose your path:**

#### Option A: Continue from Week 7 (Recommended)
If you completed the Week 7 project and want to migrate your own code:

1. **Keep the starter files** - Don't delete them! You'll need `package.json` and `vite.config.js`
2. **Keep your Week 7 files nearby** - Open them in a separate window or tab
3. **Follow the migration steps** in Part 2 below to move your Week 7 code into the React structure

#### Option B: Fresh Start
If starting fresh, use the provided starter files:

1. **Install dependencies:**
   ```bash
   npm install
   ```

2. **Start the development server:**
   ```bash
   npm run dev
   ```

3. **Open your browser** to the URL shown in the terminal (usually `http://localhost:5173`)

4. **Follow the TODO comments** in the starter files to complete the migration

### Step-by-Step Instructions

#### Part 1: Understanding the Vite React Setup (10 minutes)

**What is Vite?**
Vite (French for "fast") is a modern build tool that provides a faster development experience than Create React App. It uses native ES modules and provides instant hot module replacement.

**Explore the starter structure:**
```
week8-starter/
├── index.html          # Entry point (minimal, just loads main.jsx)
├── package.json        # Dependencies and scripts
├── vite.config.js      # Vite configuration
├── src/
│   ├── main.jsx        # React initialization (ReactDOM.render)
│   ├── App.jsx         # Your main component (currently empty)
│   └── App.css         # Your styles (Week 7 styles.css will go here)
```

**Key differences from Week 7:**
- **index.html**: Much simpler! Just loads the JavaScript
- **main.jsx**: Initializes React and mounts App component to the DOM
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

#### Part 2: Converting HTML to JSX (20 minutes)

**JSX Rules to Remember:**
1. `class` becomes `className`
2. All tags must be closed (`<img>` becomes `<img />`)
3. Style attributes use objects: `style={{ color: 'red' }}`
4. Comments use `{/* comment */}` syntax
5. One root element (use fragments `<>...</>` if needed)

**Step 1: Copy your Week 7 HTML structure**

Open your Week 7 `index.html` and copy everything INSIDE the `<body>` tag (but not the `<body>` tag itself).

Paste it into `App.jsx` inside the return statement:

```jsx
function App() {
  return (
    <>
      {/* Paste your Week 7 HTML here */}
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

Week 7 HTML:
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
- Using `class` instead of `className`

#### Part 3: Importing and Using CSS (10 minutes)

**Step 1: Copy your Week 7 CSS**

Copy the entire contents of your Week 7 `styles.css` file.

**Step 2: Paste into App.css**

Open `src/App.css` and replace everything with your Week 7 CSS.

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

#### Part 4: Migrating JavaScript Logic (25 minutes)

This is where it gets interesting! You'll move ALL your Week 7 JavaScript into the App component.

**Understanding React's approach:**
- Vanilla JS: Select elements, attach event listeners
- React: Use event attributes directly on JSX elements

**Step 1: Remove script tags**

In your JSX, remove any `<script>` tags - we'll handle this in React.

**Step 2: Convert navigation smooth scroll**

Week 7 JavaScript:
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

Week 7 JavaScript:
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

Note: Yes, we're still using DOM manipulation here! In Week 9, you'll learn to do this the "React way" with state.

**Step 4: Convert form validation**

Week 7 JavaScript:
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

    // etc - all your Week 7 validation logic

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

**Step 5: Add window event listeners**

For scroll-based features like active nav highlighting and skill animations:

```jsx
function App() {
  // Event handlers
  const handleNavClick = (e) => { /* ... */ };
  const handleFilterClick = (e) => { /* ... */ };
  const handleFormSubmit = (e) => { /* ... */ };

  // Scroll event for active nav and skill animations
  const handleScroll = () => {
    // Active nav highlighting
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

    // Skill animations logic here
  };

  // Attach scroll listener when component loads
  window.addEventListener('scroll', handleScroll);

  return (
    <>
      {/* All your JSX */}
    </>
  )
}
```

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
- We're still using DOM manipulation (document.querySelector) - this is OK for Week 8!
- Next week, you'll learn the React way using state and props

Common mistakes to avoid:
- Calling the function instead of passing it: `onClick={handleClick()}` (wrong) vs `onClick={handleClick}` (right)
- Forgetting to use `e.preventDefault()` on form submissions and navigation links
- Not attaching scroll listeners (they need to be set up when component loads)

#### Part 5: Testing and Debugging (15 minutes)

**Step 1: Test each feature**

Go through each interactive feature:
- [ ] Smooth scroll navigation works
- [ ] Active nav link updates on scroll
- [ ] Project filter buttons work
- [ ] Mobile menu toggle works
- [ ] Skill animations trigger on scroll
- [ ] Form validation works
- [ ] Form submission shows success message

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

**Step 4: Compare with Week 7**

Open your Week 7 portfolio in another tab and compare:
- Does the layout look the same?
- Do all interactions work identically?
- Are animations smooth?

### Understanding the Monolithic Component Pattern

**Why start with one giant component?**

You might wonder: "Isn't this messy? Why put everything in one component?"

Great question! Here's why:
1. **Learning progression**: Understanding how React handles events before learning component decomposition
2. **Migration strategy**: Easier to migrate everything first, then refactor
3. **Seeing the differences**: Clearly see what's different from vanilla JS
4. **Preparation for Week 9**: Next week you'll break this apart into logical components

**What makes this different from Week 7?**

Even though we're still using some DOM manipulation:
- We're using JSX instead of HTML
- We're using React's event system
- We're importing CSS as a module
- We're using a React development server with HMR
- We're setting up for proper React patterns (coming in Week 9)

**Preview of Week 9:**

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

### Challenge Extensions

**Intermediate:**
- Add a loading message that shows while Vite is bundling
- Create a custom 404 page component
- Add environment variables for configuration

**Advanced:**
- Implement React StrictMode and fix any warnings
- Add PropTypes (even though we're not using props yet)
- Set up ESLint with React rules

**Graduate (253A):**
- Configure Vite for production optimizations
- Add lazy loading for the projects section
- Implement code splitting with React.lazy (preview for later weeks)

### Troubleshooting Guide

**Problem:** Vite dev server won't start

**Solution:**
```bash
rm -rf node_modules package-lock.json
npm install
npm run dev
```

---

**Problem:** "React is not defined" error

**Solution:** Add at top of App.jsx:
```jsx
import React from 'react'
```

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

**Week 8 Prep Work:**
- React Front To Back Chapter 1 - Introduction
- React Front To Back Chapter 2 - React Basics and JSX

**Related Week Projects:**
- Week 5: Static Portfolio (HTML/CSS foundation)
- Week 7: Interactive Portfolio (JavaScript functionality you're migrating)
- Week 9: Component-Based Portfolio (where you'll decompose this monolith)

**Tools:**
- [React Developer Tools](https://react.dev/learn/react-developer-tools) - Browser extension
- [Vite Guide](https://vitejs.dev/guide/) - Understanding the build tool
- [JSX Transformer](https://transform.tools/html-to-jsx) - Quick HTML to JSX conversion

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
- [ ] All styles from Week 7 present and working
- [ ] Page looks identical to Week 7 portfolio

**JavaScript Migration:**
- [ ] Smooth scroll navigation works on nav link clicks
- [ ] Active nav link updates based on scroll position
- [ ] Project filter buttons work and show correct projects
- [ ] Mobile menu toggle opens/closes navigation
- [ ] Skill animations trigger when scrolling to skills section
- [ ] Form validation works on all required fields
- [ ] Form submission shows success message

**Code Quality:**
- [ ] All event handlers defined inside App component
- [ ] `onClick`, `onSubmit` used instead of addEventListener
- [ ] `e.preventDefault()` called where needed
- [ ] No unused code or commented-out blocks
- [ ] Code is readable with proper indentation

**Functionality:**
- [ ] All Week 7 features work identically
- [ ] No broken links or images
- [ ] Responsive design works on mobile
- [ ] All animations smooth and performant

**Bonus (Optional):**
- [ ] Added personal customization
- [ ] Included console.log statements for learning
- [ ] Experimented with additional JSX features

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

Keep these thoughts in mind for Week 9 when you'll learn component decomposition!

---

**Congratulations!** You've successfully migrated your portfolio to React. This is a huge step toward modern web development. In Week 9, you'll learn to organize this code into clean, reusable components. But for now, celebrate turning your vanilla JavaScript portfolio into a React application!
