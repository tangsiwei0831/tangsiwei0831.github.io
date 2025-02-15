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


### **Feature Branch Strategy vs. Trunk-Based Development (TBD)**  

| **Aspect**              | **Feature Branch Strategy**                             | **Trunk-Based Development (TBD)**               |
|-------------------------|---------------------------------------------------------|------------------------------------------------|
| **Branching Model**     | Developers create long-lived feature branches.         | Developers commit directly to the trunk (`main`) or use short-lived branches. |
| **Merge Frequency**     | Less frequent, often after days/weeks of work.         | Very frequent, usually multiple times per day. |
| **Merge Complexity**    | High—long-lived branches cause complex merges (merge conflicts). | Low—small, incremental changes reduce conflicts. |
| **Integration Approach**| Features are integrated late, often leading to large, risky merges. | Continuous integration ensures features are merged and tested quickly. |
| **Feature Visibility**  | Features remain hidden in branches until merged.       | Feature flags are used to hide incomplete features in production. |
| **Risk of Merge Hell**  | High—long-lived branches increase merge conflicts.     | Low—short-lived changes reduce risk. |
| **Testing Strategy**    | Testing often happens at the end of the feature cycle. | Automated testing in CI/CD ensures quick feedback. |
| **Deployment Frequency**| Less frequent due to large, bundled releases.         | Frequent, even multiple times per day. |
| **Best for...**         | Projects where features take longer and require isolation. | Fast-paced, agile teams practicing continuous deployment. |

### **Key Takeaways:**
- **Feature Branch Strategy** is better when teams need feature isolation for longer development cycles.
- **TBD** is best for **high-speed, continuous delivery environments**, reducing integration complexity.