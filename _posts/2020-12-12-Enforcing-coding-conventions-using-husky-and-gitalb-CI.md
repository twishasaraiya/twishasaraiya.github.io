Is it becoming difficult to maintain the code ? Is your team pushing changes frequently ? and are you owner of any of these features or entire module? If your answer to any one of them is yes then this blog might be worth your time

So you have your code and you have a team and you need to meet deadlines AND **you also need to ensure that code is maintainable** and big AND **this is your first time owning a module**. In this blog I am going to put across some ideas how we can make it easier. At the bottom, I have also shared some of the talks, blogs that I found during my reseacrh

> Disclaimer: The examples and tools are specific to Javascript language and Gitlab CI but the generic idea could be used across any language

## **Setup CI jobs** - Make your reviewers life easier

If you have a CI/CD setup, then awesome :+1: we will talk about how to make it better. If not, you should definitely start looking into it first right now.:pray: 

We had a CI/CD pipeline setup just to ease the deployments, nothing more. But we can do better, right ? we can find the issues right before they even get merged. This reduces a lot of effort and time in the development lifecycle. Your developers know the issues before it goes for testing and your reviewer's job is automated too so they can spend time on other areas 

Few of basic checks that you can have in your CI pipeline are

 - **Check if you build is passing** : Consider a scenarios where one of the new developers on the team has made some breaking changes. We realise that after the deployment fails. This could affect the developer's confidence. We dont want that. So basically this check will block your mrerge request and let your developer know that something has broken. We setup our build job in way that for different target branches then it should build for a different configurations/environments.
 - **Check if you any lint errors** : Do you have more than 2-3 people working on the same codebase,then you need this check. You can just setup a `eslint.yaml` with your rules configured or use one of the standards provided by `google` or `airbnb`. For every merge request it will run and show a list of errors in the (gitlab or github)UI itself. The reviewer need not manually check for this every time and make comment for the developer to resolve. Again this could be a blocking check too for the merge request.
 - **Check if all your test are passing** : You are building new features but at the same time you dont want the current features to stop working. Adding this check helps us realize faster if any of the current features are breaking because of the new changes. Now for this check to give you proper result you need to have good amount and quality of test cases written. Another scenario might be you are enchancing the current feature but the testcases are not updated. This check will tell your developer to update the testcase as well with the enhancements. This way your tests remain updated too.

Some of the other checks that could be added are

-  **[Code quality](https://docs.gitlab.com/ee/user/project/merge_requests/code_quality.html)** : You can have a code quality requirements that must be passed before it goes to production. This check provides you reports and how much more effort would be required on development side. You can provide data rather than words to the project manager. 
- **Visual Tests** : Visual test in the merge request helps to understand how the look and feel of the button or any UI component has changed. If you are building a project with a frontend team, for every new feature or css changes it would be difficult for developers to provide screenshots for the reviewer. There are multiple approaches to automate this. I will update this post with my take on different approaches later or maybe write a separate blog.
- **[Security scanning](https://docs.gitlab.com/ee/user/application_security/)**: Keeping your application secure is very important part of the development process. It may so happen that your dependencies are outdated or have security issues. Or a team member has pushed some keys/access tokens/passwords in the codebase. There are many reasons your should have some form of automated security setup done for the project if not for every pull request.

Now you have multiple jobs running in your pipeline which take let's say any time between 5-15 mins to run. After the pipeline runs you realise that have some minor lint errors or your build is failing. Does the developer have to wait this long ? Can't we know it earlier ? So another thing that could be done is to stop the pipeline if your basic checks are failing and run other checks in parallel once your base check has passed. For large databases, even running all the unit tests would take time. So in the next step we will talk about how enforce the standards before you even push the changes and make a merge request

## **Fail Locally** - Make your developers life easier

Developers need to ensure that code is formatted, linted, tested and most importantly it builds. As humans, it happens that we may unintentionally include sime error in our code while pushing them. This is where [husky](https://www.npmjs.com/package/husky) comes into the picture. As per their official documentation,

> You can use it to lint your commit messages, run tests, lint code, etc... when you commit or push

Husky allows you to run scripts for your git commit, push and others as well. As for any npm package you need to install it first as a dev dependency

```javascript
npm install husky --save-dev
```

Now you can setup different git hooks for reasons like

 - pre-commit hook: 
 
      To run prettier and eslint before your code is committed. This ensures all your commits are formatted and linted and you dont need to make separate commits just to fix them when the error occurs in your CI pipeline. The developers are aware of the error earlier, therefore it saves time. Additionaly you could also do a lint check on the commit message here as well. Let's say you want all the developers to follow standard practices for commit messages so that it becomes easier to find which commit introduced a particular change. Depending on the standards and pattern that is to be followed a check can be added for commit messages as well. [Commitlint](https://www.npmjs.com/package/commitlint) is one the package that helps you with this.
      
 - pre-push: 
 
      For a small project, pre-push would be a good place to run `npm build`. Why ? Running it for every commit seems like a over kill. At the same time if your project is too big and takes time to build it would be better to add the build job to the CI pipeline as we discussed above since we dont want your developers to lose context of what they have to do while the project build for 7-10 mins. Keep your build check in the CI pipeline as well even if you are adding it to pre-push hook.
      
      Also, this could be a place to run `npm test` as well. You can either run all your tests. Or you can run just the tests related to the files the developer has changed and then run all your test in the CI pipeline.



For example: A very simple setup that can be done for your project is shown below :arrow_down:. What this will do is for every commit it will format your javascript/typescript files and auto fix all the eslint issues. If any of this fails, it will not allow the developer to commit the changes.  

```json
// package.json


"husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "*.{js,jsx,ts,tsx}": [
      "eslint --fix",
      "prettier --write"
    ],
    "*.{json,md}": [
      "prettier --write"
    ]
  }

```

What is `lint-staged` right? [lint-staged](https://www.npmjs.com/package/lint-staged) allows you to run lint and prettier only on the files that are staged by `git add` and not on all files. Besides you can run any command for the hooks, like for example


```json
// package.json
"husky": {
    "hooks": {
      "pre-commit": "prettier --write",
      "pre-push": "npm build"
    }
  }

```

You can also run multiple commands by appending them by `&&` 

```json
// package.json
"husky": {
    "hooks": {
      "pre-commit": "prettier --write && eslint --fix",
      "pre-push": "npm test && npm build"
    }
  }

```

Thank you reading so far, if you like the article or found it useful, feel free to share and let me know

## Other Resources

- Paul Armstrong's talk on ["Move fast with confidence"](https://www.youtube.com/watch?v=ikn_dBSski8&t=899s)
- Angie Jones talk on ["Visual Tests in every pull request"](https://githubuniverse.com/Visual-tests-on-every-pull-request/)

  
