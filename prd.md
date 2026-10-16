---
title: Vision Access v2 PRD
---

# PRD: Frinks Vision Access v2 (Org Structure, Roles and Project Access)

**Product:** Frinks Vision  
**Status:** Draft for review  
**Date:** 16 October 2026  
**Prototype:** [vbv-fr.github.io/vision-access-v2](https://vbv-fr.github.io/vision-access-v2/)  
**Replaces:** the current Roles & Permissions model (FP-203 and its sub-tasks) and the Teams & Plants section

---

## 1. Summary

Vision's access control will follow how a factory is organised. Every user belongs to a **Plant → Division → Sub-division** and holds **one or more roles**. Roles are a small, fixed set: three admin roles and seven work roles, each covering one area of the product. Admins can add custom roles on top.

People see a project only when they have been **added to it**, and inside a project they see only the **instances on their part of the plant**. Everyone added to a project can view everything in it. Their roles decide what else they can do.

---

## 2. Why we are changing it

**What goes wrong today**
- Permissions are ticked item by item (every project, variant, camera, capture and dataset) inside each role. Every new project or dataset means editing roles, roles grow very large, and access quietly drifts.
- Who can see a project and what they can do on it are mixed in one place, so neither can be checked at a glance.
- Plants and teams are set up separately, with no link between them, and they don't limit what anyone sees.

**What the new model fixes**
- Two separate questions, answered in two places: the **project** decides who is on it, and the **role** decides what that person can do.
- Roles are a short, fixed checklist. A new project never requires editing a role.
- Instance visibility follows the plant structure automatically.

---

## 3. Goals and non-goals

**Goals**
1. Set up the organisation as one hierarchy: Plant → Division → Sub-division.
2. Give every user a placement in that hierarchy and one or more roles.
3. Provide ready-made, locked roles so most companies never need to build a role.
4. Let project creators choose exactly who can access a project.
5. Show each person only the instances that belong to their part of the plant.
6. Enforce every permission on the server, not just on screen.

**Non-goals (v2)**
- Permissions on individual datasets, captures or cameras.
- A different role for the same person on different projects.
- A separate "Project Admin" role (see §11).
- Single sign-on, or one person belonging to more than one company.
- An activity log. It is recommended as a follow-up (§12) but is not part of v2.

---

## 4. Key concepts

| Term | Meaning |
|---|---|
| **Organisation** | One customer company. |
| **Plant** | A factory site, e.g. *Pune*. |
| **Division** | A department inside a plant, e.g. *Body Shop*. |
| **Sub-division** | A line or cell inside a division, e.g. *Line A*. It replaces today's *Team*. |
| **Placement** | Where a user sits: plant, division and sub-division. |
| **Role** | A named set of permissions. A user can hold several. |
| **Project member** | A user who has been added to a project. |
| **Instance tag** | The plant, and optionally the division and sub-division, that an instance belongs to. |

---

## 5. Organisation structure

### 5.1 One hierarchy, one screen
- The current **Teams & Plants** section is replaced by a single page, **Plants & Divisions**.
- It shows a tree: Plant → Division → Sub-division.
- Each row shows how many users and instances sit at that level, and offers Add, Rename and Delete where allowed.

### 5.2 Who can change the structure

| Action | Org Admin | Plant Admin | Division Admin | Everyone else |
|---|---|---|---|---|
| Add, rename or delete a **plant** | Yes | No | No | No |
| Add, rename or delete a **division** | Any plant | Own plant | No | No |
| Add, rename or delete a **sub-division** | Anywhere | Own plant | Own division | No |
| View the tree | Yes | Yes | Yes | Section hidden |

### 5.3 Rules
- A plant, division or sub-division **can't be deleted** while it still has units under it, users placed in it, or instances tagged to it. The message names what is left, e.g. *"Line A still has 3 users and 1 instance. Move or remove them first."*
- Names must be unique among siblings, e.g. no two divisions called *Assembly* in the same plant.

---

## 6. Users

### 6.1 Fields
| Field | Required | Notes |
|---|---|---|
| Name | Yes | Accept real names, including accents, hyphens and apostrophes. |
| Email | Yes | Can't be changed after the invite. Accept all valid domains. |
| Phone | No | |
| Plant | Yes, except Org Admins | Org Admins show "All plants". |
| Division | Yes for non-admins and Division Admins | Plant Admins show "All divisions". |
| Sub-division | Yes for non-admins | Admins show "All sub-divisions". |
| Roles | At least one | Several allowed. |

The placement dropdowns are linked: choosing a plant filters the divisions, and choosing a division filters the sub-divisions.

### 6.2 Who can manage users

| Action | Org Admin | Plant Admin | Division Admin | Everyone else |
|---|---|---|---|---|
| See the Users list | All users | Users in own plant | Users in own division | Section hidden |
| Add a user | Anywhere | In own plant | In own division | No |
| Edit or deactivate a user | Anyone | Users in own plant, except Org Admins | Users in own division, except Plant and Org Admins | No |
| Give **Org Admin** | Yes | No | No | No |
| Give **Plant Admin** | Yes | Own plant | No | No |
| Give **Division Admin** | Yes | Own plant | Own division | No |
| Give built-in or custom work roles | Yes | Yes | Yes | No |

### 6.3 Safety rules
- Nobody can remove their own admin role or deactivate themselves.
- The last active Org Admin can't be deactivated or have Org Admin removed. The message reads *"Make someone else an Org Admin first."*
- Deactivated users are logged out immediately and keep no access.
- Invited users can be sent the invite again.

---

## 7. Roles

### 7.1 Built-in roles (locked: can't be edited or deleted)

**Admin roles**

| Role | Scope |
|---|---|
| **Org Admin** | Whole organisation |
| **Plant Admin** | Their own plant |
| **Division Admin** | Their own division |

All admins:
- see **every project** and have **every permission** inside each one;
- see instances within their scope;
- can add users and give roles within their scope (§6.2);
- can create custom roles;
- can change the structure within their scope (§5.2).

Admins hold all work permissions automatically, so they can hand out any work role.

**Work roles.** Each one gives Create, Edit, Delete and Download on its own area.

| Role | Create | Edit | Delete | Download | Also includes |
|---|---|---|---|---|---|
| **Project Management** | New projects | Name, description, classes, project access | Projects | — | Manage Access on projects they're a member of |
| **Recipe Management** | Variants, cameras, captures, modules | Module configuration | Recipe items | — | |
| **Datasets Management** | Datasets, image uploads | Datasets | Datasets and images | Images | Run pre-training analysis |
| **Annotation** | Start annotating existing images | Annotations | Annotations | Annotations | Run auto-annotation |
| **AI Models** | Train a model | Thresholds on the training results page | Models | Model files | |
| **Instance Management** | Instances | Instances | Instances | Instance package | FrinksEdge Download |
| **License Download** | — | — | — | Demo licenses | |

### 7.2 What everyone gets
- **View on everything** inside any project they're a member of. This is never switched off.
- **Manual Assistant** (chatbot and manuals).

### 7.3 Several roles
A user with several roles gets everything those roles allow combined. For example, *Annotation + Datasets Management* can upload images, annotate them, run auto-annotation and download both.

### 7.4 Custom roles
- Only admins can create custom roles.
- The role editor keeps today's two steps, **Basic Information → Permissions**. The Finalize step is removed.
- The Permissions step is a single grid:
  - rows: Project Details, Recipe, Datasets, Annotations, AI Models, Instances, Demo License;
  - columns: **Create, View, Edit, Delete, Download**;
  - View is always ticked and locked;
  - columns that don't apply to a row are left empty;
  - the extras (pre-training analysis, auto-annotation, FrinksEdge Download) are switches under their row.
- **Save** stays greyed out until something changes.
- A custom role can't be an admin role.
- A custom role can't be deleted while anyone holds it, active or deactivated. The message names the holders. If nobody holds it, delete after a confirmation popup.
- Role names are unique, and names of deleted roles can be reused.

### 7.5 The Roles page
- Columns: Role Name, Type (Admin / Built-in / Custom), Description, Users Assigned.
- Built-in roles show a lock and only a **View** option.
- Custom roles have View, Edit and Delete.

---

## 8. Projects and project access

### 8.1 Who sees a project
- **Admins** see every project.
- **Everyone else** sees only projects they're a member of.
- If a non-admin is on no project, the Projects page says *"You have not been added to any project yet. Please contact your administrator."*

### 8.2 Creating a project
- **Who can create:** admins and holders of **Project Management**.
- The existing Create Project drawer keeps its fields: name, description, inspection speed, product flow, cameras, variants and classes.
- A new section at the end, **Project Access**, lets the creator pick the users who can access the project:
  - filter by plant and division, and search by name;
  - each user's placement and roles are shown;
  - the creator is always included and can't be removed;
  - admins aren't listed, because they always have access.
- The section header shows the count: *"N users selected · Org, plant and division admins always have access."*

### 8.3 Changing access later
- The project card's ⋮ menu has **Manage Access**. It opens the same user picker.
- Available to admins, and to members who hold Project Management or created the project.

### 8.4 Inside a project
Each section shows its actions only to people allowed to use them. Everyone else sees the same content read-only.

| Section | Read-only for members | Extra actions by role |
|---|---|---|
| Recipe Management | View recipes | Add Variant, Configure, Delete (Recipe Management) |
| Datasets & Annotations | View datasets | Upload, Create Dataset, Delete, Download, Pre-training Analysis (Datasets); Annotate, Auto-annotate, Download (Annotation) |
| AI Models | View results | Train, Edit Thresholds, Download, Delete (AI Models) |
| Instances | View instances in their scope (§9) | Create, Edit, Delete, Download (Instance Management) |

---

## 9. Instance visibility

### 9.1 Tagging
- Every instance is tagged with a **plant** (required), and optionally a **division** and **sub-division**.
- This extends today's instance list, which already has Plant and Team columns. The Instances table shows Plant, Division and Sub-division.

### 9.2 The rule
A person sees an instance when **every level that is set on both the instance and the person's scope matches**.

| Person's scope |
|---|
| Org Admin: nothing set (sees all) |
| Plant Admin: plant |
| Division Admin: plant + division |
| Everyone else: plant + division + sub-division |

**Examples**

| Instance tag | Priya (Pune / Body Shop / Line B) | Rahul (Plant Admin, Pune) | Kavya (Division Admin, Chennai / Assembly) |
|---|---|---|---|
| Pune / Body Shop / Line A | No | Yes | No |
| Pune / Body Shop / Line B | Yes | Yes | No |
| Pune / Body Shop (no line) | Yes | Yes | No |
| Chennai (plant only) | No | No | Yes |
| Chennai / Assembly / Line 1 | No | No | Yes |

### 9.3 Creating an instance
- **Admins** can tag an instance anywhere in their scope.
- **Instance Management holders** create instances in their own sub-division by default (see open question Q2).
- The drawer says who will see it, e.g. *"Visible to people in this sub-division and its admins."*

---

## 10. Navigation

| Menu item | Shown to |
|---|---|
| Projects | Everyone |
| Plants & Divisions | Admins |
| Users & Permissions → Users, Roles | Admins |
| FrinksEdge Download | Admins and Instance Management holders |
| Manual Assistant | Everyone |
| Download Demo License button (Projects page) | Admins and License Download holders |

---

## 11. Decision: no separate "Project Admin" role

A "Project Admin" who can create projects was suggested. **Recommendation: don't add it.**
- **Project Management** already lets a person create projects, edit them, and choose who can access them.
- A fourth admin type whose only difference is creating projects would overlap with the three real admins, who can already do this.
- The need behind the suggestion is someone who runs projects across plants without being an Org Admin. That is met by **Project Management plus membership** of those projects.

---

## 12. Server rules and dependencies

These are required for v2 to be trustworthy.

1. **Every permission is checked on the server for every request,** including reads. Hiding a button is never the only protection.
2. **Company wall:** the server confirms that every item belongs to the user's company.
3. **The server works out the project, plant and company from the item itself,** never from what the browser sends.
4. **Changes apply immediately:** a role change, deactivation or access change takes effect on the person's next action, with no re-login.
5. **Locked roles are locked on the server,** not just on screen.
6. **Recommended follow-up: activity log.** Who changed which role, user, structure or project access, and when.

---

## 13. Moving existing data over

| Today | v2 | How |
|---|---|---|
| Plants | Plants | Kept as they are. |
| Teams (not linked to plants) | Sub-divisions | Each team is placed under a division in a plant. Divisions are created where needed. Needs an Org Admin to confirm the mapping. |
| User plant and team | User plant, division and sub-division | Taken from the team mapping above. |
| Org Admin role | Org Admin | Direct. |
| Standard User | (removed) | Users get View through project membership instead. |
| Custom roles (per-item ticks) | Built-in or custom roles | For each role, collect every permission type it has on any project and make that its standard. Match to built-in roles where they fit; keep the rest as custom roles. |
| Users with several roles | Users with several roles | Kept, now officially supported. |
| Access to a project | Project membership | Anyone with any permission on a project today becomes a member. |
| Instance plant and team | Instance plant, division and sub-division | Taken from the team mapping above. |

**Review step:** Deb produces a report per company before the switch-over. It covers:
- the team-to-sub-division mapping;
- which users gain access (where a permission on part of a project becomes access to the whole project);
- project membership.

The report is shared with the PM for review.

---

## 14. User stories and acceptance criteria

### Story 1: Set up the organisation structure
*As an Org Admin, I want to set up plants, divisions and sub-divisions in one place, so that users and instances can be placed correctly.*
- **AC1.** Plants & Divisions shows a collapsible tree with user and instance counts per row.
- **AC2.** Org Admins can add, rename and delete plants, divisions and sub-divisions anywhere.
- **AC3.** Plant Admins can do so only for divisions and sub-divisions in their own plant. Division Admins only for sub-divisions in their own division.
- **AC4.** A unit with child units, users or instances can't be deleted, and the message names what is left.
- **AC5.** The section is hidden for non-admins, and blocked on the server.

### Story 2: Add and edit users with placement and roles
*As an admin, I want to place a user in the structure and give them one or more roles, so that they get the right access.*
- **AC1.** Plant, Division and Sub-division are linked dropdowns, limited to the admin's scope.
- **AC2.** Several roles can be selected, grouped as Admin roles, Roles and Custom roles.
- **AC3.** Admin roles above the admin's own level are shown disabled.
- **AC4.** Choosing an admin role sets the lower placement levels to "All" and makes them optional.
- **AC5.** Admins can only see, edit and deactivate users in their scope (§6.2).
- **AC6.** Self-demotion, self-deactivation and removing the last Org Admin are blocked with a clear message.
- **AC7.** A failed save never deletes the user. It shows an error and keeps the user as they were.

### Story 3: Built-in and custom roles
*As an admin, I want ready-made roles and the option to create my own, so that I rarely have to build roles from scratch.*
- **AC1.** The 3 admin roles and 7 work roles exist in every company, show a lock, and can only be viewed.
- **AC2.** Admins can create custom roles using the Create / View / Edit / Delete / Download grid with View locked on.
- **AC3.** Save is greyed out until something changes. Duplicate names are refused with *"A role named X already exists."*
- **AC4.** Deleting a role held by anyone, active or deactivated, is blocked and names the holders. Otherwise it is deleted after confirmation, and the delete is confirmed by the server before "Role deleted" shows.

### Story 4: Choose who can access a project
*As a Project Management holder or admin, I want to pick the users who can access my project when I create it, so that only the right people see it.*
- **AC1.** Create Project ends with a Project Access section listing users with their placement and roles, filterable by plant and division and searchable.
- **AC2.** The creator is always included. Admins aren't listed and always have access.
- **AC3.** Non-admins see only projects they're a member of.
- **AC4.** Manage Access on the project card opens the same picker for admins, Project Management members and the creator.

### Story 5: Act according to my roles inside a project
*As a project member, I want to see everything in the project and use only the actions my roles allow.*
- **AC1.** Every member can view Recipe Management, Datasets & Annotations, AI Models and Instances.
- **AC2.** Each action button appears only for people allowed to use it (§8.4), and the server refuses the action for everyone else.
- **AC3.** A user with several roles gets all the actions of all their roles.

### Story 6: See only my instances
*As a line engineer, I want to see only the instances for my plant, division and line, so that I don't act on another site's machines.*
- **AC1.** Instances are tagged with plant (required), division and sub-division (optional), shown as columns.
- **AC2.** The Instances list follows the rule in §9.2, on screen and on the server, including downloads.
- **AC3.** When creating an instance, the plant/division/sub-division choices are limited to the creator's scope.

### Story 7: Menu and page access
*As any user, I want to see only the menus I can use.*
- **AC1.** Menu items follow §10.
- **AC2.** Opening a hidden page by its web address redirects to Projects, and the server refuses the data.

---

## 15. How we'll know it worked

| Measure | Target |
|---|---|
| Role edits needed when a new project is created | 0 |
| Time for an admin to add a user with placement and roles | Under 1 minute |
| Permissions shown on screen that the server also enforces | 100% |
| Access-related support tickets per month | Down from today's baseline |

---

## 16. Open questions

| # | Question | Prototype default |
|---|---|---|
| Q1 | Can a project creator add users from any plant, or only their own? | Any plant. Projects are organisation-wide and only instances are split by plant. |
| Q2 | Where can a non-admin with Instance Management create instances? | Only in their own sub-division. |
| Q3 | Can a Plant Admin make another Plant Admin for the same plant? | Yes. |
| Q4 | Can non-admins see the Users and Roles lists? | No, the section is hidden. |
| Q5 | Must non-admins have all three placement levels? | Yes. |
| Q6 | Who can edit or delete a custom role? It applies company-wide, but a Division Admin may create it. | Any admin. Alternative: only the creator and Org Admins. |
| Q7 | Can a user belong to more than one sub-division, e.g. a quality engineer covering two lines? | No, one placement per user. |
| Q8 | How do Frinks support staff enter customer companies, and is every action shown to the customer? | Out of scope for v2. Only Frinks admins can switch organisation. |
| Q9 | How long can someone stay logged in (idle timeout and maximum session)? | To be agreed. |

---

## 17. Appendix: prototype guide

- **Link:** [vbv-fr.github.io/vision-access-v2](https://vbv-fr.github.io/vision-access-v2/)
- **Switch user:** use the name box at the bottom-left of the sidebar.

| Person | Roles | Placement | Try this |
|---|---|---|---|
| Ananya Rao | Org Admin | All plants | Add a plant, add a user anywhere, view any role |
| Rahul Mehta | Plant Admin | Pune | Users list shows only Pune; Org Admin can't be assigned; add a division in Pune |
| Kavya Iyer | Division Admin | Chennai / Assembly | Instances show only Chennai and Assembly ones |
| Vikram Shah | Project Management, Recipe Management | Pune / Body Shop / Line A | Create a project and pick its users |
| Priya Nair | Annotation, Datasets Management | Pune / Body Shop / Line B | Sees 2 projects; Upload, Annotate and Auto-annotate actions |
| Arjun Das | Instance Management, License Download | Chennai / Assembly / Line 1 | Create Instance; FrinksEdge Download menu; Demo License button |
| Meera Joshi | AI Models | Chennai / Quality / Final Audit | Train and Edit Thresholds |
| Sanjay Kulkarni | Line Engineer (custom) | Pune / Paint Shop / Line C | Custom role mixing Recipe and Datasets |
| Deepa Menon | Annotation (invited) | Chennai / Assembly / Line 2 | Shows "Invited" status and Resend Invite |
