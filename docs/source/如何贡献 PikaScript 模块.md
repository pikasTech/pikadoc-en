# How to contribute to modules
## Help improve existing modules


### Pull the latest module

- When adding new content to an existing module, make sure you have pulled the latest module.

- The way to pull the latest module is to use the latest version in requestment.txt.

E.g:
````
STM32G0==latest
````

- **Delete the modules that need to be developed in reqeustment.txt**, to prevent misoperation (such as pulling the module again) causing the module being developed to be overwritten.
### Modify the module and test

- Add new Python interface for modules --> [module].pyi
- Or provide a better implementation --> pikascript-lib/[module]/*.c

- (Optional) Update module information in pikascript/README.md and pikascript/README_zh.md.

### Submit the module's files
   - fork a pikascript repository, then clone it locally.

![](assets/1638664526181-09b00c29-fc72-429a-bb99-3f009eae141e.png)

   - Copy [module].pyi to pikascript-lib/[module] folder.
   - Copy the entire modified pikascript-lib/[module] folder into the forked pikascript/package folder.
   - git add adds files, and git commit commits once.
   - git log View the commit id after the commit, fill in the new version name in pikascript/packages.toml after fork, and copy the current commit id.

E.g:

````
[[packages]]
name = "STM32G0"
releases = [
  "v1.0.2 0052a28582ac8a85cc48e1d676d9a3be5cb1b93f",
  "<new version name> <current commit id>",
]
````

   - git commit -a commits again, adding modifications to packages.toml.
   - git push to your forked repository.
   - Submit a pull request.

![](assets/1638664500423-e4ad59fa-e476-48f0-b7ec-89f98eb70e6c.png)
## Commit the new module

- Create a new [module].pyi file and pikascript-lib/[module] folder.
- Develop and test new modules.
- (Optional) Update module information in pikascript/README.md and pikascript/README_zh.md.
- Submit the module's files
   - Fork a pikascript repository, then clone it locally.

![](assets/1638664526181-09b00c29-fc72-429a-bb99-3f009eae141e.png)

   - Copy [module].pyi to pikascript-lib/[module] folder.
   - Copy the entire pikascript-lib/[module] folder to the forked pikascript/package folder.
   - git add adds files, and git commit commits once.
   - git log View the submitted commit id, add a new module to pikascript/packages.toml after fork, and fill in the module name, version name and current commit id.

E.g:

````
[[packages]]
name = "<new module name>"
releases = [
  "<new version name> <current commit id>",
]
````

   - git commit -a commits again, adding modifications to packages.toml.
   - git push to your forked repository.
   - Submit a pull request.

![](assets/1638664500423-e4ad59fa-e476-48f0-b7ec-89f98eb70e6c.png)
