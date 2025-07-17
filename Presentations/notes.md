<!-- night.css Theme -->

# <span style="color:gold">Notes</span>

Nikolaos Vlachogiannakis, 2025

---

## Table of Contents

<div style="display: flex; gap: 0em; text-align: left;">

<div style="flex: 1;">

- [Connect Remotely with SSH](#/2)
  - [Password](#/2/1)
  - [Passwordless](#/2/2)
- [Create Repository & Git Push](#/3)
  - [Manually](#/3/1)
  - [Using gh](#/3/2)
  - [Push Changes on GitHub](#/3/3)
  - [LazyGit](#/3/4)
  - [Check GitHub Connection](#/3/5)
  - [Additional Information](#/3/6)
    - [What is SSH?](#/3/7)
    - [What is gh?](#/3/7)
    - [What is LazyGit?](#/3/8)
- [Create a Project and use Poetry](#/4)
- [Advanced Workflow](#/5)

</div>

<div style="flex: 1;">

- [Reveal.js Presentation](#/6)
  - [Install Node.js](#/6/1)
  - [Get a Template](#/6/2)
  - [Change Theme](#/6/4)
  - [Change Presentations with the same template](#/6/5)
  - [Load Slides Locally](#/6/6)
  - [Hosting Slides on GitHub Pages](#/6/7)
  - [Public vs Private Repos and Ways to Share Slides](#/6/9)
  - [Managing Large Files with Git LFS](#/6/12)
  - [Additional Information](#/6/15)
    - [What is Reveal.js](#/6/16)
    - [HTML File](#/6/17)
    - [CSS File](#/6/18)
    - [Markdown File](#/6/19)
    - [JSON File](#/6/20)

</div>

</div>

---

# <span style="color:gold">How to connect to a Remote Machine?</span>

--

# <span style="color:gold">With Password</span>

<pre><code class="language-bash" data-trim>
ssh user@remote-server
</code></pre>

--

# <span style="color:gold">Passwordless Login</span>

--

## <span style="color:gold">1.Generate an SSH key(If you don't have one)</span>

Locally, on your computer, open Terminal(for Windows, Git Bash):

<pre><code class="language-bash" data-trim>
ssh-keygen -t ed25519 -C "your_email@example.com"
</code></pre>

- Press Enter to accept the default path: C:\Users\YourName\.ssh\id_ed25519

- Choose no passphrase to enable passwordless login

## <span style="color:gold">2.Copy the Public Key to the Remote Server</span>

<pre><code class="language-bash" data-trim>
ssh-copy-id user@remote-server
</code></pre>

--

## <span style="color:gold">3.Use SSH Config File</span>
Create or edit:
<pre><code class="language-bash" data-trim>
~/.ssh/config
</code></pre>
Add:
<pre><code class="language-bash" data-trim>
Host myserver
  HostName REMOTE_SERVER
  User USERNAME
  IdentityFile ~/.ssh/id_ed25519
</code></pre>
Now you can simply run:
<pre><code class="language-bash" data-trim>
ssh myserver
</code></pre>

## <span style="color:gold">4.Test the Passwordless SSH Login</span>
<pre><code class="language-bash" data-trim>
ssh user@remote-server
</code></pre>
You should be logged in without a password.

---

# <span style="color:gold">Create a Repository on GitHub</span>
# <span style="color:gold">& Push Changes</span>

--

# <span style="color:gold">Manually</span>

- Create a Repository on GitHub(either public or private) with a ReadMe file

- Create a folder:
<pre><code class="language-bash" data-trim>
mkdir folder_name
</code></pre>
- Go to its path:
<pre><code class="language-bash" data-trim>
cd folder_name
</code></pre>
- Copy the SSH key(ssh_key) from your repository in GitHub:
<pre><code class="language-bash" data-trim>
git clone ssh_key
</code></pre>
- You can open the file we cloned in VScode and work on it.

--

# <span style="color:gold">Using gh</span>

After the successful installation of gh:

- To create a new repo in the current folder, do:
<pre><code class="language-bash" data-trim>
git init
git add .
git commit -m "Initial commit"
gh repo create REPO_NAME --public --source=. --push
</code></pre>
- Clone a repo:
<pre><code class="language-bash" data-trim>
gh repo clone arampatzis/shell
</code></pre>
- See all your repos in Github:
<pre><code class="language-bash" data-trim>
gh repo list
</code></pre>

--

# <span style="color:gold">Push changes on GitHub</span>

<pre><code class="language-bash" data-trim>
cd repository_name
</code></pre>

<pre><code class="language-bash" data-trim>
git status
</code></pre>
With this command we can see all changed files which are 
<span style="color:red">red</span>.

So, we use:
<pre><code class="language-bash" data-trim>
git add (all files on red separated by comma)
</code></pre>
**OR**
<pre><code class="language-bash" data-trim>
git add .
</code></pre>
Then:
<pre><code class="language-bash" data-trim>
git commit -m "This is a message explaining what we changed"
</code></pre>
Finally:
<pre><code class="language-bash" data-trim>
git push
</code></pre>
Now, all the changes you made are uploaded in your repository on Github!

--

# Otherwise, Use <span style="color:gold">LazyGit</span>

- Inside the folder you want to push, write:
<pre><code class="language-bash" data-trim>
lazygit
</code></pre>
- A new window will appear where, in the upper left side it will be all the files that 
have changes and in order to include each file in your push Press 
<span style="color:gold">Spacebar</span>.

- After selecting all the files we want to push, Press <span style="color:gold">c</span>
 and write what changes you have made and then Press 
 <span style="color:gold">Enter</span>.

- To push these changes in your GitHub Press <span style="color:gold">Shift+P</span>
- To Exit LazyGit Press <span style="color:gold">q</span>

--

# <span style="color:gold">GitHub Connection</span>

If we want to check if GitHub is connected with our machine using ssh:
<pre><code class="language-bash" data-trim>
ssh -T git@github.com
</code></pre>
and you should get a message like:
<pre><code class="language-bash" data-trim>
Hi username!You've successfully authenticated, but GitHub does not provide shell access.
</code></pre>

--

# <span style="color:gold">Additional Information</span>

## <span style="color:gold">What is SSH?</span>

<span style="color:gold">SSH</span> (Secure Shell) is a cryptographic network protocol 
that allows you to securely connect to remote computers or servers over an unsecured 
network. It's commonly used for accessing and managing servers, transferring files, and 
authenticating with services like GitHub. SSH works by using a key pair: a private key 
stored on your device and a public key shared with the remote service. When you connect, 
SSH verifies your identity using these keys, ensuring encrypted, secure communication 
without needing to send passwords.

## <span style="color:gold">What is gh?</span>

It is Github's command line interface (CLI) that can be used to manage your github 
without leaving the command line and the keyboard.

--

## <span style="color:gold">What is LazyGit?</span>

<span style="color:gold">Lazygit</span> is a simple, fast, terminal-based UI for Git.

It lets you manage your Git repositories more easily than typing out commands — you can 
stage, commit, push, pull, resolve conflicts, and browse logs interactively, all from a 
clean text interface.

It’s especially useful when you want the power of Git without memorizing all the 
commands, and it works entirely in your terminal without needing a graphical IDE.

## <span style="color:gold">IDE(Integrated Development Environment)</span>

It’s a software application that provides a set of tools for programmers to write, test,
 and debug their code more efficiently - all in one place. Examples of IDEs: PyCharm, 
 Visual Studio, Eclipse.

---

# <span style="color:gold">Create a Project and use Poetry</span>

--



--

# <span style="color:gold">Additional Information</span>

--

## <span style="color:gold">What is Poetry?</span>

<span style="color:gold">Poetry</span> is a tool for managing Python projects. It helps 
you declare, install, and resolve dependencies, build your package, and publish it. 
Unlike using pip and virtualenv separately, Poetry handles both dependency management 
and virtual environments seamlessly.

It’s widely used because it creates a pyproject.toml file (modern Python standard), 
ensures reproducible builds, and makes it easy to develop and distribute Python 
packages.

--

## <span style="color:gold">TOML File</span>

A <span style="color:gold">.toml</span> file (short for Tom’s Obvious, Minimal Language)
 is a simple configuration file format that is easy for humans to read and write.

In Python projects - especially when using tools like Poetry - the pyproject.toml file 
is used to define the project’s metadata, dependencies, build settings, and tool 
configurations.

---

# <span style="color:gold">Advanced Workflow</span>

---

# <span style="color:gold">Reveal.js Presentation</span>

--

## <span style="color:gold">1. Install Node.js</span>

You need Node.js (which includes npm) installed on your system.

**Windows / macOS / Linux:**

- Visit: [nodejs](https://nodejs.org/)
- Download the LTS version and install it

- To verify:
<pre><code class="language-bash" data-trim>
node -v
npm -v
</code></pre>

--

## <span style="color:gold">2. Get a Template</span>

- Go to a template on GitHub
- Click Code -> Download ZIP
- Extract it on your computer
- Rename the folder as you wish

--

## <span style="color:gold">Editing Your Slides</span>

Slides are written in slides.md using Markdown.

Use **---** to separate horizontal slides, and **--** for vertical slides:

<pre><code class="language-markdown" data-trim>
# Welcome

&#45;&#45;&#45;

## Slide 1

Some text

&#45;&#45;

- Vertical slide 1

&#45;&#45;

- Vertical slide 2
</code></pre>

--

# <span style="color:gold">Customizing Theme</span>

Reveal.js themes are in the dist/theme folder.

To change the theme, edit index.html and modify this line:

<pre><code class="language-html" data-trim>
<link rel="stylesheet" href="dist/theme/black.css" id="theme">
</code></pre>

You can try other themes like <span style="color:gold">white.css</span>, 
<span style="color:gold">night.css</span>, <span style="color:gold">moon.css</span>,etc.

--

# <span style="color:gold">Change Markdown File</span>

Inside the file that contains the template, you can create a folder called 
<span style="color:gold">Presentations</span> for example. There you can store all 
Markdown files for every presentation you make.

#### To change the presentation, you can do the following:

- Go to <span style="color:gold">index.html</span>
- Find:
<pre><code class="language-markdown" data-trim>
data-markdown="slides.md"
</code></pre>
- Change it to:
<pre><code class="language-markdown" data-trim>
data-markdown="Presentations/notes.md"
</code></pre>

- Now, the presentation written in the <span style="color:gold">notes.md</span> file 
will appear.

--

# <span style="color:gold">Load Slides Locally</span>

- Inside the folder, open a terminal and run:
<pre><code class="language-bash" data-trim>
npm install
</code></pre>
This installs local development tools like the server (gulp).

- Run the Presentation:
<pre><code class="language-bash" data-trim>
npm start
</code></pre>
- Open your browser at:
<pre><code class="language-bash" data-trim>
http://localhost:8000
</code></pre>
You should see your slides rendered.

--

# <span style="color:gold">Hosting Slides on GitHub Pages</span>

You can publish your presentation online for free using GitHub Pages.

#### **Step-by-Step:**

1. Make sure your repository includes:

- index.html
- slides.md
- dist/ and plugin/ folders from Reveal.js
- empty .nojekyll file(prevents GitHub from using Jekyll)

2. Go to your GitHub repository -> Settings -> Pages

3. Under Build and Deployment, set:

- Source: Deploy from branch
- Branch: main
- Folder: / (root)
- Click Save

4. Wait 1-10 minutes. GitHub will show a <span style="color:green">green</span> message 
like the following:
<pre><code class="language-markdown" data-trim>
Your site is live at https://your-username.github.io/repo-name/
</code></pre>

--

## <span style="color:gold">If the green message does</span> 
## <span style="color:red">NOT</span> <span style="color:gold">appear</span>

- Add a .nojekyll file(empty file with that name)
- Make a small commit to trigger a rebuild:
<pre><code class="language-markdown" data-trim>
git commit --allow-empty -m "Trigger rebuild"
git push
</code></pre>

- Visit your slides live in the browser!

--

# <span style="color:gold">Public vs Private Repos and Ways to Share Slides</span>

--

### ✅ <span style="color:gold">Option 1: GitHub Pages (Public Repo)</span>

- If your repository is public, GitHub Pages will host your presentation at:
<pre><code class="language-markdown" data-trim>
https://your-username.github.io/repo-name/
</code></pre>
- This is the <span style="color:green">easiest</span> and recommended way to publish 
Reveal.js slides.

### 🚫 <span style="color:gold">Option 2: GitHub Pages (Private Repo)</span>

- GitHub Pages does <span style="color:red">not</span> work for private repos unless you
 are on a GitHub Enterprise plan.

- You will see a 404 or the site won’t build.

--

### <span style="color:blue">🛠</span> <span style="color:gold">Option 3: Local Server (for Private Sharing)</span>

- You can run your slides locally with:
<pre><code class="language-bash" data-trim>
npm start
</code></pre>

- Then access them at:
<pre><code class="language-bash" data-trim>
http://localhost:8000/
</code></pre>

- This works for development or sharing over a local network.

### 📤 <span style="color:gold">Option 4: Export to PDF</span>

- Reveal.js can export slides to PDF:
<pre><code class="language-bash" data-trim>
npm install -g decktape
decktape reveal http://localhost:8000 slides.pdf
</code></pre>

- Useful for email, offline viewing, or submissions.

--

# 📦 <span style="color:gold">Managing Large Files with Git LFS</span>

If you need to include large binary files in your presentation (e.g. .mp4 videos, .pdf 
documents), it is recommended to use Git Large File Storage (LFS) instead of committing 
them directly to the repository.

--

## ✅ <span style="color:gold">Install Git LFS</span>

- Ubuntu/Debian:
<pre><code class="language-bash" data-trim>
sudo apt install git-lfs
</code></pre>

- Windows:  
Download and install from [git-lfs.com](https://git-lfs.com/)

--

### ✅ <span style="color:gold">Set Up Git LFS in Your Repo</span>

1. Run this ince per machine:
<pre><code class="language-bash" data-trim>
git lfs install
</code></pre>
2. Track the file types you want to store with LFS:
<pre><code class="language-bash" data-trim>
git lfs track "*.mp4"
git lfs track "*.pdf"
</code></pre>
3. Add and commit:
<pre><code class="language-bash" data-trim>
git add .gitattributes
git add media/my-video.mp4
git commit -m "Add video using Git LFS"
git push
</code></pre>

## ℹ️ <span style="color:gold">Notes</span>

- GitHub’s free LFS plan includes 1 GB of storage and 1 GB/month bandwidth.

- You can view tracked LFS files using: <span style="color:lime">
<code class="language-bash" data-trim>git lfs ls-files</code></span>

To avoid LFS entirely, you can also upload media externally (e.g. Google Drive or a web 
server) and link to them in your slides.

--

# <span style="color:gold">Additional Information</span>

--

## <span style="color:gold">What is Reveal.js?</span>
Reveal.js is an open-source framework for creating HTML-based presentations using 
Markdown or HTML. Instead of writing slides in a visual editor (like Keynote or 
PowerPoint), you write them in plain text - and Reveal.js turns them into a modern, 
interactive slideshow in the browser.

--

## <span style="color:gold">HTML File</span>

- The HTML file is the main entry point of the presentation.
- It defines the page structure, loads Reveal.js and its plugins, configures settings, 
and optionally contains the slides directly if not using Markdown.

--

## <span style="color:gold">CSS File</span>

- The CSS file defines the styling of your presentation.
- It customizes the look and feel - fonts, colors, layout - either by overriding the 
default Reveal.js theme or adding new styles.

--

## <span style="color:gold">Markdown File</span>

- The Markdown (.md) file contains the slide content written in simple, readable 
Markdown syntax.
- It’s loaded by the HTML file (via the Reveal.js markdown plugin), making it easy to 
edit slides without touching HTML.

--

## <span style="color:gold">JSON File</span>

- A JSON file is optional and is used to store extra data or configuration for the 
presentation.
- It can define metadata, timing, or slide-related data that JavaScript in the HTML file
 can read and use.

---

# <span style="color:gold">How to take screenshot and store it as PDF instead of PNG to NOT lose quality</span>

---

# 🦧 That is all 🦧
