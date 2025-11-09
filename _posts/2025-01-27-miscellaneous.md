---
title: 'Miscellaneous'
date: 2025-02-15
permalink: /posts/miscellaneous
tags:
  - Software
  - Data
---
# Excel
1. When download online Google sheet with multiple tabs, the only version to choose is xlsx which is excel. 

2. Note that on Google sheet, the tab of Google sheet does not have length limit, but in excel, the limit is 30 characters. So the tab name of downloaded version may cut a few characters in this case.

# Github
1. For a fork repo, the github action yaml will not shown in action section initially. You need to make some modifications to yaml script and then it will become normal. Note that the configuration will not be inherited from original repo.

# Oracle
1. DPY-3013: unsupported Python type int for database type DB_TYPE_VARCHAR

    The problem happened due to the data insertion into Orcle DB is separated into different chunks, therefore, for some of the columns, the value may be null for the first chunk, then Oracle database regards tis column as VARCHAR type, which will be conflicted with the real type value (float) later. Check [issue](https://github.com/oracle/python-oracledb/issues/187) for the details.

    According to comments in the link, the issue can be resolved quickly by create a new cursor each chunk. 
    ```
    curr = conn.cursor()
    curr.executemany(query, values)

    curr.close()
    conn.commit()
    ```

# Trunk Based Development
  Trunk-Based Development (TBD) is a branching strategy where all developers work in a single main branch, often called the "trunk" (typically `main` or `master`). Feature branches, if used, are short-lived and merged quickly, ensuring that code is continuously integrated and deployed.

### **Key Principles:**
1. **Single Long-Lived Branch** – All changes go into the trunk (`main`).
2. **Frequent Commits & Merges** – Developers commit small, incremental changes multiple times a day.
3. **Short-Lived Feature Branches (or Direct Commits)** – If feature branches are used, they are very short-lived (hours or a day).
4. **Feature Flags** – Incomplete or risky features are hidden behind feature toggles instead of long-lived branches.
5. **Continuous Integration & Deployment (CI/CD)** – Code is built, tested, and deployed automatically.

### **Benefits:**
**Reduces Merge Conflicts** – No long-lived branches, so merging is easier.  
**Faster Feedback Loop** – Changes are tested and deployed quickly.  
**Encourages Collaboration** – Everyone works on the same branch, reducing silos.  
**More Reliable Releases** – Small, incremental updates are less risky than large merges.

### **Challenges & Mitigations:**
- **Code Stability Risks** → Use feature flags, strong CI/CD, and automated tests.  
- **Requires Discipline** → Developers must commit small changes frequently.  
- **Not Always Ideal for Large Teams** → May require additional coordination tools like stacked diffs (e.g., Meta’s `Sapling`).  

# Canary Deployment
Canary deployment is a deployment strategy where you gradually roll out a new version of an application to a subset of users before making it available to everyone. This approach minimizes risk by allowing you to monitor the new version’s performance and catch potential issues before a full release.


# Node package maintenance
1. Use npm outdated command to list uotdated packages or use [npm-check-updates](https://www.npmjs.com/package/npm-check-updates) package to check for updates
2. Update dependencies
    - Minor/Patch Updates: Update version to the latest
    - Major Updates
      - Review CHANGELOG in official Github repository of the package for any breaking changes
      - If no breaking changes, update to latest
      - If there is breaking changes, create a scoping ticket to evaluate impact
3. testing: for packages, ensure all test passes, rebuild packages and use [yalc](https://github.com/wclr/yalc) to locally link packages to a sample app for testing
