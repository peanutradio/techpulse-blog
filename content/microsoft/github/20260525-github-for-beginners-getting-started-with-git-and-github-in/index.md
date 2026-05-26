---
categories:
- MS
- GitHub
date: '2026-05-25T16:00:00+00:00'
description: Welcome back to GitHub for Beginners. We&rsquo;ve covered a lot this
  season, so make sure to check out our other episodes. Our most recent one was all
  about ope
draft: false
original_url: https://github.blog/developer-skills/github/github-for-beginners-getting-started-with-git-and-github-in-vs-code/
source: GitHub Blog
tags:
- Developer skills
- GitHub
- Git
- Github
- GitHub for beginners
- VS Code
title: 'GitHub for Beginners: Getting started with Git and GitHub in VS Code'
---

Welcome back to GitHub for Beginners. We&rsquo;ve covered a lot this season, so make sure to check out our other episodes. Our most recent one was all about open source, what it is and how to contribute to the community.



This time, we&rsquo;re going to take a look at VS Code, a free popular source code editor provided by Microsoft. It has a fair amount of functionality built in that integrates with GitHub, which is what we&rsquo;ll be taking a look at today. Using GitHub in VS Code reduces context switching, streamlines your workflow, and boosts your productivity. By the end of this post, you&rsquo;ll understand how to use VS Code to initialize a repository, switch branches, as well as stage, commit, and push your changes. And the best part is, you&rsquo;ll be able to do all this without leaving the editor.



Note that if you want to follow along with this blog post (or the video), you will need to install both Git and VS Code. If you need a refresher on how to install Git, you can check out one of our earlier GitHub for Beginners episodes.



As always, if you prefer to watch the video or want to reference it, we have all of our GitHub for Beginners episodes available on YouTube.



First some basics



You probably already know that GitHub is a resource that hosts only copies of your code in repositories. So what is Git? Git is the program for managing that source code, and it can be used in multiple different ways (e.g., from the command line, through VS Code, etc.). Visual Studio Code, often abbreviated as VS Code, leverages Git to enable you to manage your code in GitHub.



Initializing a folder



The first step to using Git with VS Code is initializing a folder to reflect your repository on GitHub.




Open VS Code.



Select the top icon (the Explorer icon) in the left-hand column. It looks like two files on top of each other.



Click the Open Folder button.



In the file explorer, select a folder that has some code you want to upload to GitHub, and click Open.



After opening your code, select the Source Control icon. By default, it&rsquo;s the third icon from the top.



Click the Initialize Repository button.




At this point, a few things will change in your UI. First, you can see the branch name in the bottom bar on the left-hand side. The default is main. You can rename your branch by using the Command Palette.




To open the Command Palette, press Shift-Command-P on Mac or Ctrl-Shift-P on PC.



In the Command Palette, start typing &ldquo;rename&rdquo; and select the Git: Rename Branch command.



In the box, provide the new name of the branch and press Enter.




At this point, you can see that the name of the branch in the bottom-left corner has been updated to the new name. You can rename it back to main by following the same steps.



Another change you&rsquo;ll see after initializing your repository is that each of the files in the Source Control Panel have a &ldquo;U&rdquo; next to them. &ldquo;U&rdquo; stands for untracked, meaning that these files are new or changed, but have not been added to the repository. To track a file, you just need to click the plus sign adjacent to the file name. If you want to track all of the files, you can click the top plus next to the word CHANGES.



When you do this, the file(s) that you select will be staged, and the letter next to them will change to &ldquo;A&rdquo;. This means the file is staged, but not yet uploaded to GitHub. In order to upload the changes, you&rsquo;ll need to commit the files.




Enter a message in the text box at the top of the Source Control window describing the commit. Alternatively, you could click the Copilot icon in the text box to have Copilot generate a commit message for you.



Select the Commit button underneath the text box to commit your changes.




It&rsquo;s that simple!



Creating and changing branches



Right now, you&rsquo;re likely on the main branch. Remember that you can check the branch by looking in the bottom-left corner of your window. If this were a major app and you were adding new code or features, you&rsquo;d want to create a new branch and use that for your work.




Open the Command Palette by pressing Shift-Command-P on Mac or Ctrl-Shift-P on PC.



Enter &ldquo;create branch&rdquo; in the text box.



Select Git: Create Branch&hellip; from the list of options.



Enter the name of the new branch in the box. For example, &ldquo;new-features&rdquo;.



Press Enter.




Once you do this, VS Code will create the new branch and automatically transfer you to this branch. You can verify this by looking at the branch name in the bottom-left corner.



Tracking changes you make



Now that you&rsquo;re in your working branch, go ahead and enter a line of code in a file. When you do this, you&rsquo;ll notice that a thin green line appears on the right side of your editor next to the code you added. This section of the editor is called the gutter, and this green line reflects a new line of code that you added.



Move to a different line and make some changes in the line of code that already exists. When you do this, you&rsquo;ll see a blue line with a pattern across it in the gutter. This line indicates that you&rsquo;ve made changes to an existing line of code.



Finally, move to an unchanged line in the file and delete it. Notice that the gutter adds a red arrow. This indicates that a line of code was removed from the file.



When you modify this file, you can see that the file appeared in your Source Control window under the CHANGES header. If you hover over the filename, you&rsquo;ll see several buttons appear. There are buttons to open a file, discard your changes, and stage your changes. Similar to before, if you have multiple files with changes, you can hover over the CHANGES header and see buttons that will let you review unstaged changes, open the changes, discard all the changes, and stage all the changes.



Viewing diffs



Sometimes you want to see the changes that you made in a file. VS Code lets you do this by performing side-by-side diffs without needing another tool. To see the changes on a file, click on the name of the file you want to see in the Source Control window. From here you can see the changes in the file and compare the differences.



Depending on your preferences, you can also view your diffs in what is called an inline view.




Click the three dots (&hellip;) in the top-right of the diff view.



Select Inline View from the drop-down menu.




This lets you see all of the changes in a single window without splitting it up over two separate views. From this view, you can even make edits inside of the diff view.



Once you&rsquo;ve made any changes you want to make to the file, it&rsquo;s time to upload them to GitHub. Following the steps we went over before, go ahead and stage your changes, and then commit your staged changes. Once you finish these steps, you shouldn&rsquo;t have any files displayed in the Source Control window.



Merging branches



Note that changes you&rsquo;ve uploaded will still be in your working branch. If you navigate back to the main branch, you&rsquo;ll see the original code before the changes you made.




Click the branch name in the bottom-left of the window.



Branches names appear in the drop-down at the top of the program. Select the main branch.




In order to get these changes into your main branch, you&rsquo;ll need to merge branches.




In the Source Code window, click the three dots (&hellip;) next to CHANGES.



In the context menu, hover over Branch and select Merge&hellip;



The box at the top will prompt you with branches that you want to merge from. Select the branch with the changes you want to merge into main.




Congratulations! Now your main branch has incorporated the changes!



Publishing to GitHub



Let&rsquo;s say you want to take your project and publish it up to GitHub. To do so, click the Publish Branch button in the Source Control window. VS Code will prompt you with whether you want to publish it as a private or as a public repository. Select the option you want, and then VS Code handles the rest.



Once VS Code finishes the publishing process, it will notify you that the project has been published to a repository on GitHub. You can click the Open on GitHub button to visit your project on GitHub and see it online.



Cloning a repository



Now let&rsquo;s say you want to clone a repository so that you can work on it on your machine. This creates a local copy that you can use and sync the changes between the two locations. There are multiple ways that you can clone a repository, and this is an easy way to do it in VS Code.




Navigate to the home page of the repository you want to clone.



Click the green &lt;&gt; Code button at the top of the repository file list.



In the drop-down menu, select the Copy URL to clipboard button next to the box containing the repository&rsquo;s URL.



Open VS Code.



Open the Command Palette by pressing Shift-Command-P on Mac or Ctrl-Shift-P on PC.



Type in &ldquo;clone&rdquo;.



Select Git: Clone from the list of options.



Paste the URL into the box.



Select Clone from URL with the URL you pasted after it.



A pop-up window will ask you for a location. Choose the folder where you want to store the project files.



Click the Select as Repository Destination button.



A pop-up manu will appear asking if you want to open the repository. Select Open.




Congratulations! You&rsquo;ve just cloned the repository to your machine and can start to work on it in your local environment!



Model Context Protocol



Did you know that you don&rsquo;t have to do everything manually in VS Code? You could leverage Model Context Protocol (MCP) to safely let AI tools access external tools and data. The first step is to add the GitHub MCP extension.




In the left navigation bar, click the Extensions icon.



In the search box, enter &ldquo;@mcp github&rdquo;.



Select the GitHub extension from the list.



In the description for the extension, click Install.



A pop-up appears, asking you to allow the MCP server to authenticate. Select Allow.



Select your GitHub username from the list.




At this point, the GitHub MCP server is installed. You can verify this by looking at the bottom of the Extensions view and seeing the section for installed MCP servers. With the MCP server installed, you can use Copilot chat to create some code for you, and it will do so by leveraging external tools where necessary.




Open the chat window by selecting the Chat icon next to the Command Palette window.



Enter a prompt asking Copilot to add some features to your project.




For an example of how this works, check out the video version of this episode which walks through asking Copilot to create several issues to improve a flash card application.



Next steps



And that&rsquo;s it! We covered some of the most common ways developers use VS Code to interact with Git. We&rsquo;ve gone over everything from creating repositories, to publishing on GitHub, and even threw in a little bit of using AI at the end. There are more advanced tips, but these elements are what you&rsquo;ll use most frequently.



Happy coding!

The post GitHub for Beginners: Getting started with Git and GitHub in VS Code appeared first on The GitHub Blog.

---
*원문: [https://github.blog/developer-skills/github/github-for-beginners-getting-started-with-git-and-github-in-vs-code/](https://github.blog/developer-skills/github/github-for-beginners-getting-started-with-git-and-github-in-vs-code/)*
