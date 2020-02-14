**Angular Best Practices with Jim Cooper - Pluralsight**
1. File and Folder Structure
 	- LIFT
    	- Locate code quickly
    	- Identify code at a glance
    	- Flattest structure possible
    	- Try to be DRY

2. Single responsibility

3. Immutability

4. Angular modules:
	- Angular app is a set of components which have other components etc. -> It's a TREE
	- It's good practice to bundle up associated branches in modules
	- General structure:
		- App Module (provided automatically
		- Code Module
			- shared singleton services (ex. service containing current user object)
			- app-level components (ex. navbar)
		- Shared Module
			- shared components, directives, pipes (ex. loading spinner component)
		- Feature Modules
			- multiple modules grouping together components, direvtives, services etc.
