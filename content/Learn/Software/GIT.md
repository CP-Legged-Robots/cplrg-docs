
Git is a file sharing system first created by Linus Torvalds, the creator of Linux. Just like saving a video game, GIT lets you save the state of your code as a "commit". You can go back to previous commits or share them for other users. 

github.com is a website for sharing git repositories and is the industry standard for software development teams to work on software simultaneously. Other services use git as a foundation and attempt to simplify the user interface such as bitbucket. 

If you have never learned git before check out these resources before moving on further.

The Best Git Intro I have ever watched
https://www.youtube.com/watch?v=mJ-qvsxPHpY


# Recommended workflow. 
Git workflows are a hot topic under discussion without any "perfect" solution. I offer this as a starting point. Use it until you find a good reason not to. It will minimize merge conflicts and stop you from fighting bugs introduced by other team members commits.

If you disagree read the philosophy section. Feel free to disregard and just use the workflow if you trust me. 
## Philosophy
Our goals in software development besides getting the job done are to save time, and stay simple. When we work in a team we are capable of tackling way more than any single person could accomplish but have some challenges. Code that one person writes is dependent on another person's code. If a dependency is changed or broken in development it can break code, and this costs a lot of time. 

**Branches**
The main branch on GitHub is sacred. Do not push bleeding work in the main branch. If you have a bug you will break everything for all other users. You will introduce merge conflicts and it will cost a lot of time to fix improper merges. This begs repeating so I will say it again in all capitals. THE MAIN BRANCH IS SACRED. DO NOT PUSH BLEEDING UNPROVEN CODE TO THE MAIN BRANCH. 

### Workflow
Create a branch for each area of development. Develop features for that area of development in that branch. Only merge into the main branch when the code has been verified to work in your branch. 

Isolate files so that developers do not have to edit the same file. When this occurs you will go through a merge conflict process. This isn't the end of the world but try to minimize it. Sometimes you will have to work on the same file and run merges.

#### Example
For the Switch project there are 3 branches.
	main
	Hardware-Dev
	MPC

The project is broken up into packages. Jack if focused on the MPC packaged, Jeremy is working on several other packages but doesn't edit any files in MPC. 

Both Developers do have to edit the Dockerfile to update dependencies. When they do they isolate their additions with comments to document their changes. 

**Monday**
Jeremy is working on code for the motor interfaces and pushes them to the Hardware-Dev branch at the end of the night. There are all sorts of errors in the code, but he saves his progress so he can access the code anywhere he has internet. 

Jack is working on Model Predictive Code in the MPC branch and breaks all sorts of stuff. He decides to revert to an earlier commit on MPC. 

Jack and Jeremy never get any errors that the other introduced. 

**Tuesday**
Jeremy while creating something new runs into an error with code that Jack wrote. He sends him a message to fix it. He can delete the code from his branch in the meantime so he can keep working.

**Wednesday**
Jack fixes the code in the MPC branch and merges it into main. Jeremy can pull the code and merge main into the Hardware-dev branch to receive the change. 

**Thursday**
Nick the new guy is exited to help but too nervous to ask questions and writes some code and pushes it to main. Everyone cries. In the distance, sirens. 


When Jeremy tries to push code to main he gets a merge error. 



 
Typical
	git add .
	git commit -m "message"
	git push


Override local branch with git
	git fetch origin
	git reset --hard origin/branch
	git clean fd