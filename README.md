<h1>Project Documentation: Portfolio using Next.js, Vercel, and Supabase</h1>

<h2>Table of Contents</h2>

<ol>
  <li><a href="#introduction">Introduction</a></li>
  <li><a href="#initial-setup">Initial Setup</a></li>
  <li><a href="#project-structure">Project Structure</a></li>
  <li><a href="#implementing-pwa">Implementing PWA</a></li>
  <li><a href="#storybook">Storybook</a></li>
  <li><a href="#test-driven-development-tdd">Test-Driven Development (TDD)</a></li>
  <li><a href="#deployment">Deployment</a></li>
  <li><a href="#project-management">Project Management</a></li>
  <li><a href="#ci-cd-with-github-actions">CI/CD with GitHub Actions</a></li>
</ol>

<hr>

<h2 id="introduction">1. Introduction</h2>

<h3>Project Description</h3>

<p>This project is a personal portfolio built using Next.js for the frontend framework, Vercel for hosting, and Supabase as the database. It includes basic features such as a homepage, personal portfolio display, and contact information.</p>

<h3>Project Goals</h3>

<ul>
  <li>Showcase a professional portfolio with responsive and modern design.</li>
  <li>Implement PWA for enhanced user experience.</li>
  <li>Utilize Storybook for isolated component development.</li>
  <li>Implement TDD to ensure high code quality.</li>
</ul>

<h2 id="initial-setup">2. Initial Setup</h2>

<h3>Installing Next.js</h3>

<pre><code>npx create-next-app@latest your-portfolio
cd your-portfolio
</code></pre>

<h3>Configuring Vercel</h3>

<ul>
  <li>Install Vercel CLI: <code>npm install -g vercel</code></li>
  <li>Login to Vercel: <code>vercel login</code></li>
  <li>Deploy: <code>vercel</code></li>
</ul>

<h3>Setting up Supabase</h3>

<ul>
  <li>Sign up and create a project on <a href="https://supabase.io/">Supabase</a>.</li>
  <li>Install Supabase client: <code>npm install @supabase/supabase-js</code></li>
  <li>Configure Supabase client:</li>
</ul>

<pre><code>import { createClient } from '@supabase/supabase-js'

const supabaseUrl = process.env.NEXT_PUBLIC_SUPABASE_URL
const supabaseKey = process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY

export const supabase = createClient(supabaseUrl, supabaseKey)
</code></pre>

<h2 id="project-structure">3. Project Structure</h2>

<h3>Folder Structure Overview</h3>

<pre><code>/components
  /Button
    Button.tsx
    Button.stories.tsx
    Button.test.tsx
/pages
  index.tsx
  _app.tsx
  _document.tsx
/public
  /icons
    icon-192x192.png
    icon-512x512.png
  manifest.json
/styles
  globals.css
/lib
  supabaseClient.ts
</code></pre>

<h3>Main File Descriptions</h3>

<ul>
  <li><code>index.tsx</code>: Main portfolio homepage.</li>
  <li><code>_app.tsx</code>: Customizes the App Component.</li>
  <li><code>_document.tsx</code>: Custom Document for SSR.</li>
</ul>

<h2 id="implementing-pwa">4. Implementing PWA</h2>

<h3>Installing and Configuring Next.js <a href="https://ducanh-next-pwa.vercel.app/docs/next-pwa/getting-started">PWA</a></h3>

<ul>
  <li>Install: <code>npm i @ducanh2912/next-pwa && npm i -D webpack</code></li>
  <li>Configure <code>next.config.mjs</code>:</li>
</ul>

<pre><code>import withPWAInit from "@ducanh2912/next-pwa";

const withPWA = withPWAInit({
  dest: "public",
});

export default withPWA({
  // Your Next.js config
});
</code></pre>

<h2 id="storybook">5. Storybook</h2>

<h3>Installing Storybook</h3>

<pre><code>npx sb init
</code></pre>

<h3>Configuring Storybook</h3>

<ul>
  <li>Run Storybook: <code>npm run storybook</code></li>
</ul>

<h3>Example Usage of Storybook</h3>

<ul>
  <li>Develop and test isolated UI components.</li>
</ul>

<h2 id="test-driven-development-tdd">6. Test-Driven Development (TDD)</h2>

<h3>Setting up Jest and Testing Library</h3>

<ul>
  <li>Install: <code>npm install --save-dev jest @testing-library/react @testing-library/jest-dom</code></li>
  <li>Configure <code>jest.config.js</code>:</li>
</ul>

<pre><code>module.exports = {
  testPathIgnorePatterns: ['&lt;rootDir&gt;/.next/', '&lt;rootDir&gt;/node_modules/'],
  setupFilesAfterEnv: ['&lt;rootDir&gt;/jest.setup.js'],
  transform: {
    '^.+\\.(js|jsx|ts|tsx)$': 'babel-jest',
  },
}
</code></pre>

<h3>Writing Unit Tests for Components</h3>

<ul>
  <li>Example: <code>Button.test.tsx</code></li>
</ul>

<pre><code>import { render, screen } from '@testing-library/react'
import Button from './Button'

test('renders button', () => {
  render(&lt;Button&gt;Click me&lt;/Button&gt;)
  expect(screen.getByText('Click me')).toBeInTheDocument()
})
</code></pre>

<h2 id="deployment">7. Deployment</h2>

<h3>Deploying to Vercel</h3>

<ul>
  <li>Ensure all configurations and testing are complete.</li>
  <li>Run <code>vercel</code> command to deploy the project.</li>
</ul>

<h2 id="project-management">8. Project Management</h2>

<h3>Maintenance and Future Development</h3>

<ul>
  <li>Update dependencies with <code>npm update</code>.</li>
  <li>Common troubleshooting for local development.</li>
  <li>Update documentation as the project evolves.</li>
</ul>

<h2 id="ci-cd-with-github-actions">9. CI/CD with GitHub Actions</h2>

<p>Automate the deployment process using GitHub Actions. This workflow ensures that changes pushed to the <code>main</code> branch trigger a new deployment to Vercel.</p>

<h3>Setting up GitHub Actions</h3>

<p>Create a workflow file (e.g., <code>.github/workflows/deploy.yml</code>) with the following content:</p>

<pre><code>name: Deploy to Vercel

on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Install dependencies
        run: npm install

      - name: Build
        run: npm run build

      - name: Deploy to Vercel
        run: npx vercel --token ${{ secrets.VERCEL_TOKEN }} --prod
</code></pre>

<p>Ensure to set up <code>VERCEL_TOKEN</code> as a secret in your GitHub repository settings. Obtain the token from Vercel dashboard (Settings &gt; Tokens).</p>

---
