## NPM (Node Package Manager) is used to manage the dependencies of your application.

Because you usually have to use a bunch of dependencies in your program, it is useful to have a way of keeping track of them. Say, for example, that you need a random number for your application. You can fetch that easily, but the difficulty comes with trying to keep it updated (or not updated). If your random number generator were to receive an update that would break the functionality of your program, you can use NPM to stay on the current version. You can also use NPM to only update these dependencies on major/minor updates, so you don't have to worry about it.

---
## Dependency-ception

If the dependencies you're using also require dependencies, NPM can manage those too. It will also manage versioning and keep track of conflicting dependencies. For example, if you used a clock dependency on your program, and that clock used the `random` dependency in version 2.0, but you also used the `random` dependency in your program in version 1.2, NPM can keep those separate and manage both independently.

---
## Libraries

When you use package files in your programs, NPM will create a `.lock` file of the packages which makes sure that you will always have the exact same files that you were working with. This means that even if the developer of the packages you're using updates them, NPM will make sure that you're using the exact same files by checking them against a SHA key, or a commit on git, which itself is a SHA key.
