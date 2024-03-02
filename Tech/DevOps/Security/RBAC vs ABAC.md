---
tags: Security, Devops
---
Identity management techniques protect your sensitive digital assets. But what form should that protection take?

Knowing the difference between [role-based access control (RBAC)](https://www.okta.com/identity-101/what-is-role-based-access-control-rbac/) vs. [attribute-based access control (ABAC)](https://www.okta.com/blog/2020/09/attribute-based-access-control-abac/) can help you make a smart decision.

The main difference between RBAC vs. ABAC is the way each method grants access. RBAC techniques allow you to grant access by **roles**. ABAC techniques let you determine access by user characteristics, object characteristics, action types, and more.

Let’s dig into the details.

## **What Is Role-Based Access Control?**

Someone logs into your computer system. What can that person do? If you use RBAC techniques, the answer to that question depends [on that person's role](https://www.okta.com/identity-101/iam-compliance/).

A role in RBAC language typically refers to a group of people that share certain characteristics, such as:

- Departments
- Locations
- Seniority levels 
- Work duties

![Role-Based Access Control](https://www.okta.com/sites/default/files/styles/tinypng/public/media/image/2020-10/Role-Based-Access-Control-Graphic.png?itok=lroEbU_0)

With a role defined, you can assign permissions. Those might involve:

- **Access.** What can the person see?
- **Operations.** What can the person read? What can the person write? Can the person create or delete files?
- **Sessions.** How long can the person stay in the system? When will the login work? When will the login expire?

All RBAC systems work like this. But the National Institute of Standards and Technology [defines four subtypes of RBAC](https://csrc.nist.gov/CSRC/media/Publications/conference-paper/2000/07/26/the-nist-model-for-role-based-access-control-towards-a-unified-/documents/sandhu-ferraiolo-kuhn-00.pdf) in case you need a bit more flexibility.

- **Flat:** All employees have at least one role that defines permissions, but some have more than one.
- **Hierarchical:** Seniority levels define how roles work together. Senior executives have their own permissions, but they also have those attained by their underlings.
- **Constrained:** Separation of duties is added, and several people work on one task together. This helps to ensure security and prevent fraudulent activities.
- **Symmetrical:** Role permissions are reviewed frequently, and permissions change as the result of that review.

These roles build upon one another, and they can be arranged by security level.

- **Level 1, Flat:** This is the least complex form of RBAC. Employees use roles to gain permissions.
- **Level 2, Hierarchical:** This builds on the Flat RBAC rules, and it adds role hierarchy.
- **Level 3, Constrained:** This builds on Hierarchical RBAC, and it adds separation of duties.
- **Level 4, Symmetrical:** This builds on the Constrained RBAC model, and it adds permission reviews. 

## **What Is Attribute-Based Access Control?** 

Someone logs into your computer system. What can that person do? ABAC protocols answer that question via the user, the resource attributes, or the environment.

As the administrator of a system using ABAC, you can set permissions by:

- **User.** A person's job title, typical tasks, or seniority level could determine the work that can be done.
- **Resource attributes.** The type of file, the person who made it, or the document's sensitivity could determine access.
- **Environment.** Where the person is accessing the file, the time of day, or the calendar date could all determine access.

![Attribute-Based Access Control](https://www.okta.com/sites/default/files/styles/tinypng/public/media/image/2020-12/abac-diagram.png?itok=TEZrQcD7)

Administrators have a great deal of nuanced control in a system like this. You could set permissions based on a variety of attributes, all working together to keep documents safe. In theory, you could even give the same person different permissions based on where the person logs in or what the person tries to do on a different day of the week.

In ABAC, elements work together in a coordinated fashion.

- **Subjects:** Who is trying to do the work? 
- **Objects:** What file within the network is the user trying to work with?
- **Operation.** What is the person trying to do with said file?

Relationships are defined by if/then statements. For example:

- If the user is in accounting, then the person may access accounting files. 
- If the person is a manager, then that person may read/write files. 
- If the company policy specifies “no Saturday work” and today is Saturday, then no one may access any files today.

## **RBAC vs. ABAC: Pros & Cons**

RBAC and ABAC both concern [control and access](https://www.okta.com/identity-101/what-is-identity-management-and-access-control/). Both approaches protect your systems, but they each come with definite benefits and drawbacks.

|   |   |
|---|---|
|**ABAC Pro**|**ABAC Cons**|
|**Well-defined control**  <br>Administrators can define, enhance, and manage many variables, ensuring a high level of control. You can develop very specific and granular rules that protect your assets.|**Time constraints**  <br>Defining variables and configuring your rules is a massive effort, especially at project kickoff.|
||**Expertise**  <br>As researchers point out, appropriate ABAC rules lead to [accurate implementation](https://link.springer.com/chapter/10.1007/978-3-030-04834-1_2). If you set up the system wrong at the outset, the fix could be time-consuming.|

|   |   |
|---|---|
|**RBAC Pro**|**RBAC Con**|
|**Simplicity**  <br>Rules within the RBAC system are simple and easy to execute. The work happens quickly, and it needs less processing power.|**Role explosions**  <br>To add granularity to their systems, some administrators add more roles. That can lead to what researchers call "[role explosions](https://www.researchgate.net/profile/D_Kuhn2/publication/260584013_Adding_Attributes_to_Role-Based_Access_Control/links/55c4b38008aeca747d617b22/Adding-Attributes-to-Role-Based-Access-Control.pdf)" with hundreds or even thousands of rules to manage.|

## **5 Identity Management Scenarios to Study** 

These examples can help you understand when RBAC systems are best and when ABAC systems might work better. We've also included an example of using both together, as sometimes that works well too.

Companies should consider the question of RBAC vs. ABAC when dealing with:

**1. Small workgroups.** RBAC is best. Defining work by role is simple when the company is small and the files are few.

If you work within a construction company with just 15 employees, an RBAC system should be efficient and easy to set up.

**2. Geographically diverse workgroups.** ABAC is a good choice**.** You can define access by employee type, location, and business hours. You could only allow access during business hours for the specific time zone of a branch.

**3. Time-defined workgroups.** ABAC is preferred**.** Some sensitive documents or systems shouldn't be accessible outside of office hours. An ABAC system allows for time-based rules.

**4. Simply structured workgroups.** RBAC is best**.** Your company is large, but access is defined by the jobs people do.

For example, a doctor's office would allow read/write scheduling access to receptionists, but those employees don't need to see medical test results or billing information. An RBAC system works well here.

**5. Creative enterprises.** ABAC is ideal because creative companies often use their files in unique ways. Sometimes, everyone needs to see certain documents; other times, only a few people do. Access needs change by the document, not by the roles.

For example, the creative staff within your company, including artists and writers, create files that are easily distributed by other employees. But those employees, including billing departments and account executives, might need to see those files. And the marketing team might want to share them.

The complexity of who should see these documents, and how they are handled, is best accomplished with ABAC.

Many times neither RBAC or ABAC will be the perfect solution to cover all the use cases you need. That’s why most organizations use a hybrid system, where high-level access is accomplished through RBAC and then fine-grained controls within that structure are accomplished through ABAC.

For example, you might use the RBAC system to hide sensitive servers from new employees. Then, you might use ABAC systems to control how people alter those documents once they do have access.

Researchers say blending RBAC and ABAC can help administrators get the best of both systems. RBAC offers leak-tight protection of sensitive files, while ABAC allows for dynamic behavior. Blending them [combines the strengths of both](https://dl.acm.org/doi/abs/10.1145/3316615.3316667). 

## **Make a Smart Decision**

Not sure which identity management model is the best for your organization? That’s okay.

Okta uses granular permissions and user access factors to let you create a secure path to access and authorization. Discover what Okta can do for your organization.