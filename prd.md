---
title: Vision Access v2 PRD
---

# PRD: Frinks Vision Access v2 (Plants & Teams, Roles and Project Access)

**Product:** Frinks Vision  
**Status:** Draft for review  
**Date:** 19 October 2026  
**Prototype:** [vbv-fr.github.io/vision-access-v2](https://vbv-fr.github.io/vision-access-v2/) (being updated to match this draft)  
**Replaces:** the current Roles & Permissions model (FP-203 and its sub-tasks)

---

## 1. Summary

- Every user belongs to **one plant and one team** and holds **one or more roles**. Org Admins need neither, and Plant Admins need only a plant.
- Roles are fixed and locked:
  - **2 admin roles** (Org Admin, Plant Admin);
  - **2 user-management roles** (Manage Users – Organisation, Manage Users – Plant);
  - **7 work roles**, including a view-only **Viewer**.
- Org Admins can add custom roles.
- Three tools are switched on or off per user: **FrinksEdge Download, Download Demo License and Manual Assistant**.
- People see a project only when they've been **added to it**. Admins see every project.
- Inside a project, people see only the **instances of their own plant and team**.
- Everyone added to a project can view everything in it. Their roles decide what else they can do.
- Actions a person can't use are shown **greyed out with a hover message** saying what's needed.

---

## 2. Why we are changing it

**What goes wrong today**
- Permissions are ticked item by item (every project, variant, camera, capture and dataset) inside each role. Every new project or dataset means editing roles, roles grow very large, and access quietly drifts.
- Who can see a project and what they can do on it are mixed in one place, so neither can be checked at a glance.
- Plants and teams are only labels. They don't limit what anyone sees.

**What the new model fixes**
- Two separate questions, answered in two places: the **project** decides who is on it, and the **role** decides what that person can do.
- Roles are a short, fixed list. A new project never requires editing a role.
- Instance visibility follows each person's plant and team automatically.

---

## 3. Goals and non-goals

**Goals**
1. Use the existing Plants and Teams lists to decide which instances each person sees.
2. Give every user a plant, a team and one or more roles.
3. Provide ready-made, locked roles, including a view-only Viewer role.
4. Let project creators choose exactly who can access a project.
5. Let trusted people manage users without making them admins.
6. Enforce every permission on the server, not just on screen.

**Not in this version**
- Divisions and sub-divisions (planned for later).
- Permissions on individual datasets, captures or cameras.
- A different role for the same person on different projects.
- A separate "Project Admin" role (see §13).
- Single sign-on, or one person belonging to more than one company.
- Notifications when people are added to or removed from projects (later).
- An activity log (next version).
- Blocking the deletion of items that other items depend on (separate task).
- Paging for very large user lists (not needed at today's scale).

---

## 4. Key concepts

| Term | Meaning |
|---|---|
| **Organisation** | One customer company. |
| **Plant** | A factory site, e.g. *Pune*. |
| **Team** | A group of people, e.g. *Line B*. Plants and teams are two separate lists; a team is not inside a plant. |
| **Placement** | A user's plant and team. |
| **Role** | A named set of permissions. A user can hold several. |
| **Other access** | Three per-user switches: FrinksEdge Download, Download Demo License, Manual Assistant. |
| **Project member** | A user who has been added to a project. |
| **Project creator** | The person who created a project. Always a member and can't be removed. |
| **Instance tag** | The plant and team an instance belongs to. |
| **Draft instance** | An instance with no plant or team yet, e.g. after its project moved to another company. |

---

## 5. Plants and Teams

- The existing **Teams & Plants** section stays: one list of plants and one list of teams. They are not linked to each other.
- The lists rarely change, so an Org Admin can add users or create projects in any order.
- **Org Admins** add, rename and delete plants and teams. Everyone else can only view the lists.
- A plant or team **can't be deleted** while users or instances still use it. The message names what is left, e.g. *"Line B is still used by 3 users and 1 instance. Move them first."*
- Names must be unique within each list.

---

## 6. Users

### 6.1 Fields

| Field | Required | Notes |
|---|---|---|
| Name | Yes | Accept real names, including accents, hyphens and apostrophes. |
| Email | Yes | Can't be changed after the invite. Accept all valid domains. |
| Phone | No | |
| Plant | Yes, except Org Admins | One plant per user. |
| Team | Yes, except Org Admins and Plant Admins | One team per user. Plant Admins show "All teams". |
| Roles | At least one | Several allowed. **Viewer** is pre-selected for new users. |
| Other access | — | Three switches. See §6.4. |
| Status | — | Active or Inactive. New users show **Invited** until they set their password. |

### 6.2 Who can manage users

| Action | Org Admin | Plant Admin | Manage Users – Organisation | Manage Users – Plant | Everyone else |
|---|---|---|---|---|---|
| See the Users list | Yes | Yes | Yes | Yes | Yes, read-only |
| Add users | In any plant | In own plant | In any plant | In own plant | No |
| Edit or deactivate users | Anyone | Users in own plant, except Org Admins | Users in any plant (§6.3) | Users in own plant (§6.3) | No |
| Move a user to another plant | Yes | No | Yes | No | No |
| Roles they can give | Any | Any except Org Admin and Manage Users – Organisation | Only roles they hold (§6.3) | Only roles they hold (§6.3) | — |
| Other access switches they can set | All three | All three | Only ones they have on | Only ones they have on | — |

Rows in the Users list show a ⋮ menu only to people who can act on that user.

### 6.3 Rules for the two Manage Users roles
- They can give **only the roles they hold themselves.**
  - **Viewer** can always be given, because it adds nothing.
  - A Manage Users – Organisation holder can also give Manage Users – Plant, the narrower version of their own role.
- They can edit or deactivate a user **only if they hold every role that user has.** So they can never change an admin, or anyone with more access than themselves.
- "Delete" means setting the user to **Inactive**. There is no permanent delete.
- They can switch on only the Other access switches they have on themselves.
- These roles give nothing inside projects. Holders see a project only if they've been added to it.

### 6.4 Other access (per-user switches)

| Switch | What it unlocks | Default for a new user |
|---|---|---|
| **FrinksEdge Download** | The FrinksEdge Download page | Off |
| **Download Demo License** | The Download Demo License button on the Projects page | Off |
| **Manual Assistant** | The Manual Assistant chatbot and manuals | On |

- **Admins always have all three.** For an admin, the switches show as on and locked: *"Always on for admins"*.
- The Users list shows each person's switches in an **Other Access** column.

### 6.5 Status and safety rules
- **Activate or deactivate:** admins and user managers switch a user between Active and Inactive from the user drawer or the ⋮ menu.
- **Inactive users** are logged out immediately and can't log in. Their project memberships are kept, so making them Active again restores their access.
- **Invited users** can be sent the invite again. They can't be added to projects until they've set their password.
- **No self-changes:** nobody can change their own plant, team, roles, switches or status.
- **Last Org Admin:** the last active Org Admin can't be deactivated or lose Org Admin. The message reads *"Make someone else an Org Admin first."*
- **Removing an admin role** needs at least one other role on that person. No warning is shown. Afterwards they see only projects they created, and an admin adds them to other projects as needed.

---

## 7. Roles

All built-in roles are **locked**: they can't be edited or deleted.

### 7.1 Admin roles

| | Org Admin | Plant Admin |
|---|---|---|
| Placement | None | Plant only |
| Projects | Every project, every permission | Every project, every permission |
| Instances | All, including drafts | Own plant |
| Users | See §6.2 | See §6.2 |
| Custom roles | Create, edit, delete | View |
| Plants & Teams lists | Change | View |
| Move instances | To any plant and team | Between teams, within own plant |
| Other access switches | Always on | Always on |

A Plant Admin can make another Plant Admin for their own plant, but never an Org Admin.

### 7.2 User-management roles

| Role | What it adds |
|---|---|
| **Manage Users – Organisation** | Add, edit and deactivate users in any plant, giving only the roles they hold themselves (§6.3) |
| **Manage Users – Plant** | The same, within their own plant |

### 7.3 Work roles

| Role | What it adds on top of View |
|---|---|
| **Viewer** | Nothing. For people who only look. Pre-selected for new users. |
| **Project Management** | Create projects (name, description, inspection speed, product flow, cameras, variants and classes), edit them and delete them. Manage project access. |
| **Recipe Management** | Create, edit and delete recipes (captures), including their module configuration. |
| **Datasets Management** | Create datasets and upload images, edit and delete them, and download images. |
| **Annotation** | Annotate existing images, run auto-annotation, and edit and delete annotations. |
| **AI Models** | Train models, edit thresholds on the training results page, and delete models. |
| **Instance Management** | Create, edit and delete instances, and download instance packages, within their own plant and team. |

### 7.4 What every project member gets
**View on everything** inside any project they're a member of. This is never switched off.

### 7.5 Several roles
A user with several roles gets everything those roles allow combined. For example, *Annotation + Datasets Management* can upload images, annotate them and download the images.

### 7.6 Custom roles
- Only **Org Admins** can create, edit and delete custom roles.
- The editor keeps today's two steps, **Basic Information → Permissions**. The Finalize step is removed.
- The Permissions step is one grid. A dash means the action doesn't exist for that area:

| Area | Create | View | Edit | Delete | Download |
|---|---|---|---|---|---|
| Project Details | ✓ | Always on | ✓ | ✓ | — |
| Recipe | ✓ | Always on | ✓ | ✓ | — |
| Datasets | ✓ | Always on | ✓ | ✓ | ✓ (images) |
| Annotations | ✓ | Always on | ✓ | ✓ | — |
| AI Models | ✓ | Always on | ✓ | ✓ | — |
| Instances | ✓ | Always on | ✓ | ✓ | ✓ (package) |

- **Annotations:** Create, Edit and Delete always go together. Ticking any one ticks all three, and unticking one unticks all three. Create includes auto-annotation.
- Custom roles cover project work only. Managing users stays with the admin and Manage Users roles.
- **Save** stays greyed out until something changes.
- A custom role can't be deleted while anyone holds it, active or inactive. The message names the holders. If nobody holds it, it is deleted after a confirmation popup.
- Role names are unique, and names of deleted roles can be reused.

### 7.7 The Roles page
- **Columns:** Role Name, Type (Admin / User management / Built-in / Custom), Description, Users Assigned.
- **Everyone** can see the list.
- **Admins** get a ⋮ menu:
  - **View** on every role;
  - **Edit** and **Delete** on custom roles, for Org Admins only.

---

## 8. Projects

### 8.1 Who sees a project
- **Org Admins and Plant Admins** see every project.
- **Everyone else** sees only projects they're a member of. This includes Project Management holders, who must also be added to a project to see it.
- If a non-admin is on no project, the Projects page says *"You have not been added to any project yet. Please contact your administrator."*

### 8.2 Creating a project
- **Who can create:** admins and Project Management holders.
- The Create Project drawer keeps its fields: name, description, inspection speed, product flow, cameras, variants and classes.
- A new section at the end, **Project Access**, lists the users who can be added:
  - users from **any plant**, filtered by plant and team, and searchable;
  - each user's plant, team and roles are shown;
  - admins aren't listed, because they always have access;
  - invited and inactive users aren't listed.
- The section header reads *"N users selected · Org and Plant Admins always have access."*
- A project can be created with nobody else added. Admins still see it, and members can be added later.

### 8.3 The project creator
- The creator is added automatically and **can't be removed** by anyone.
- When the project is opened again in the Edit Project drawer, a **greyed-out "Project Creator"** field shows who created it.
- If the creator is deactivated, they show as *"Name (deactivated)"*.
- If the project moves to another company, the creator shows as *"Transferred"* (§10).

### 8.4 Changing access later
- **Manage Access** in the project card's ⋮ menu opens the same user picker.
- Available to admins, and to Project Management holders who are members of that project.
- Project Management holders can remove other Project Management holders, but never the creator.
- When someone is removed, **their work stays**: annotations, datasets, models and the rest.

### 8.5 Deleting a project
Only admins and holders of Project Management can delete a project. The creator has no special right to delete.

### 8.6 Inside a project
Everyone on the project sees every section. Actions they can't use are **greyed out with a hover message**.

| Section | Everyone on the project | Needs this role |
|---|---|---|
| Project (card and drawer) | Open the project | Edit, Manage Access, Delete → Project Management |
| Recipe Management | View recipes | Add, Configure, Delete recipes → Recipe Management |
| Datasets & Annotations | View datasets and annotations | Create Dataset, Upload, Edit, Delete, Download images → Datasets Management; Annotate, Auto-annotate, Edit, Delete annotations → Annotation |
| AI Models | View models and results | Train, Edit Thresholds, Delete → AI Models |
| Instances | View instances of their plant and team | Create, Edit, Delete, Download package → Instance Management |

**Example hover messages:**
- *"You need Recipe Management to add recipes."*
- *"You need Datasets Management to download images."*
- *"The project creator can't be removed."*

### 8.7 Product changes that come with this
- **Classes** are added only in the Create/Edit Project drawer (Project Management). The two other places that add classes today, module configuration and the annotation job, are removed.
- **Dataset download** has one option: images. Annotations can't be downloaded.

---

## 9. Instances

### 9.1 Tagging
Every instance has a **plant and a team**, and both are required. The Instances table keeps its Plant and Team columns.

### 9.2 Who sees an instance

| Person | Sees instances of |
|---|---|
| Org Admin | Every plant, including drafts |
| Plant Admin | Their own plant |
| Everyone else | Their own plant **and** their own team |

The same rule applies to the Instances list, instance details, instance package downloads, and the instances offered in Download Demo License (which also lists only projects the person is on).

**Examples** (team names can repeat across plants, so the plant must match too)

| Instance tag | Priya (Pune / Line B) | Rahul (Plant Admin, Pune) | Arjun (Chennai / Line B) |
|---|---|---|---|
| Pune / Line A | No | Yes | No |
| Pune / Line B | Yes | Yes | No |
| Chennai / Line B | No | No | Yes |
| Chennai / Final Audit | No | No | No |

### 9.3 Creating an instance
Plant and Team are required:

| Who | Plant | Team |
|---|---|---|
| Org Admin | Any | Any |
| Plant Admin | Own plant, locked | Any |
| Instance Management holder | Own plant, locked | Own team, locked |

### 9.4 Moving an instance
- Only admins can change an instance's plant or team.
- Plant Admins can move instances only between teams, within their own plant.
- Instance Management's Edit doesn't include changing plant or team.

### 9.5 Draft instances
- An instance without a plant and team is a **draft**.
- Only Org Admins see drafts.
- A draft can't be downloaded until it has a plant and team.

---

## 10. Moving a project to another company (Change Org)

When Frinks staff move a project to another company:

1. All project members are removed.
2. The creator shows as *"Transferred"*. No user is linked.
3. All instances become **drafts**. Plants and teams aren't carried over, because each company has its own lists.
4. The new company's Org Admin assigns a plant and team to each instance.
5. Once assigned, the instance's plant and team IDs are updated everywhere they're used, including Edge, following the existing org-change rule.
6. The new company's admins see the project straight away and add members.

---

## 11. Frinks staff

- Frinks Org Admins act as admins inside customer companies.
- Their names don't appear in the customer's Users list.
- Only Frinks admins can switch between organisations.

---

## 12. Navigation and unavailable actions

| Menu item | Who can use it |
|---|---|
| Projects | Everyone |
| Teams & Plants | Everyone can view; only Org Admins can change |
| Users & Permissions → Users, Roles | Everyone can view; actions per §6 and §7.7 |
| FrinksEdge Download | Admins, and users with the switch on. Others see it greyed out |
| Manual Assistant | Admins, and users with the switch on. Others see it greyed out |
| Download Demo License button (Projects page) | Admins, and users with the switch on. Others see it greyed out |

**Greyed out vs. hidden**
- Actions a person can't use are **greyed out with a hover message** saying what's needed.
- **Hidden instead:**
  - projects the person isn't on;
  - instances outside their plant and team;
  - ⋮ menus on Users and Roles rows they can't act on;
  - Frinks staff in the Users list.

---

## 13. Decision: no separate "Project Admin" role

A "Project Admin" who can create projects was suggested. **Recommendation: don't add it.**
- **Project Management** already lets a person create projects, edit them and choose who can access them.
- A separate admin type whose only difference is creating projects would overlap with Org and Plant Admins, who can already do this.

---

## 14. Server rules

These are required for v2 to be trustworthy.

1. **Every permission is checked on the server for every request,** including reads. A greyed-out button is never the only protection.
2. **Company wall:** the server confirms that every item belongs to the user's company.
3. **The server works out the project, plant, team and company from the item itself,** never from what the browser sends.
4. **Changes apply immediately:** a change to roles, switches, status, placement or project access takes effect on the person's next action, with no re-login.
5. **Locked roles are locked on the server,** not just on screen.

---

## 15. Moving existing data over

| Today | v2 | How |
|---|---|---|
| Plants and teams | Plants and teams | Kept as they are. |
| User plant and team | User plant and team | Kept. Non-admins missing either are listed so an admin can fill them in. |
| Org Admin role | Org Admin | Direct. |
| Other people with company-level rights | Decided one by one | They are listed. For each, decide Org Admin, Plant Admin, a Manage Users role, or work roles only. |
| Standard User only (12 people today) | Viewer | Direct. |
| Custom roles (per-item ticks) | Built-in or custom roles | For each role, collect every permission type it has on any project and make that its standard. Match to built-in roles where they fit; keep the rest as custom roles. |
| Several roles per user | Several roles per user | Kept. |
| Access to a project | Project membership | Anyone with any permission on a project today becomes a member. |
| Project creator | Project creator | Kept. Creators who were deleted show as "Name (deactivated)". |
| Instance plant and team | Instance plant and team | Kept. Instances missing either are listed so an Org Admin can fill them in. |
| FrinksEdge Download, Demo License, Manual Assistant (visible to everyone today) | Other access switches | Switched on for all existing users, so nobody loses access on the day. |

**Review step:** Deb prepares a report per company before the switch-over, and the PM reviews it. It lists:
- users and instances missing a plant or team;
- people with company-level rights who need a decision;
- people moving from Standard User to Viewer;
- the project memberships being created.

**Clean-up in the same release**
- **Inferencing Customizations and instance key generation (keygen):**
  - keep the code but comment it out;
  - make sure nothing about them is visible anywhere in the UI;
  - Edge has its own copy of key generation, which is not affected. Before switching off Vision's copy, confirm no installer or script calls it. None was found in the Vision app or the Edge code.
- **Adding classes** from module configuration and from the annotation job is removed (§8.7).
- **Dataset download** keeps a single option: images (§8.7).

---

## 16. User stories and acceptance criteria

### Story 1: Plants and Teams lists
*As an Org Admin, I want to keep the plants and teams lists up to date, so that users and instances can be placed correctly.*
- **AC1.** Teams & Plants shows two lists: plants and teams.
- **AC2.** Only Org Admins can add, rename or delete. Everyone else sees the lists read-only, with the actions greyed out.
- **AC3.** A plant or team used by users or instances can't be deleted, and the message names what still uses it.

### Story 2: Add and edit users
*As an admin or user manager, I want to give a user a plant, a team, roles and other access, so that they get the right access.*
- **AC1.** Plant and Team are required for non-admins. Plant Admins need a plant only. Org Admins need neither.
- **AC2.** Several roles can be selected. Viewer is pre-selected for new users.
- **AC3.** Roles the person can't give are shown greyed out, with a hover message (§6.2, §6.3).
- **AC4.** Manage Users holders can add users only in their scope, give only roles they hold (plus Viewer), and edit only users whose roles they all hold.
- **AC5.** The three Other access switches appear with defaults off / off / on. For admins they show on and locked.
- **AC6.** Status can be set to Active or Inactive. Inactive users are logged out at once.
- **AC7.** Changing your own plant, team, roles, switches or status is blocked. The last active Org Admin is protected.
- **AC8.** Removing someone's admin role requires at least one other role.
- **AC9.** A failed save never deletes the user. It shows an error and keeps the user as they were.
- **AC10.** Everyone can see the Users list. The ⋮ menu shows only on rows the person can act on. Frinks staff aren't listed.

### Story 3: Roles
*As an Org Admin, I want ready-made roles and the option to create my own, so that I rarely have to build roles from scratch.*
- **AC1.** The 11 built-in roles exist in every company, show a lock and can only be viewed: Org Admin, Plant Admin, Manage Users – Organisation, Manage Users – Plant, Viewer, Project Management, Recipe Management, Datasets Management, Annotation, AI Models, Instance Management.
- **AC2.** Only Org Admins can create, edit or delete custom roles, using the grid in §7.6.
- **AC3.** In the grid, Annotations' Create, Edit and Delete always tick and untick together.
- **AC4.** Save is greyed out until something changes. Duplicate names are refused with *"A role named X already exists."*
- **AC5.** A role held by anyone, active or inactive, can't be deleted, and the message names the holders.

### Story 4: Create a project and choose who can access it
*As a Project Management holder or admin, I want to choose who can access my project, so that only the right people see it.*
- **AC1.** Create Project ends with a Project Access section, as described in §8.2.
- **AC2.** The creator is added automatically and can't be removed. Edit Project shows a greyed-out "Project Creator" field.
- **AC3.** A project can be saved with nobody else added.
- **AC4.** Manage Access on the project card opens the same picker, for admins and for Project Management members.
- **AC5.** Non-admins see only projects they're a member of.

### Story 5: Work inside a project
*As a project member, I want to see everything in the project and use only the actions my roles allow.*
- **AC1.** Every member can view every section.
- **AC2.** Actions the person can't use are greyed out with a hover message naming the role needed (§8.6). The server refuses them too.
- **AC3.** Classes can be added only in the project drawer. Module configuration and the annotation job no longer offer it.
- **AC4.** Dataset download offers images only. There is no annotation download.
- **AC5.** A user with several roles gets all the actions of all their roles.

### Story 6: See only my instances
*As a line engineer, I want to see only the instances of my plant and team, so that I don't act on another line's machines.*
- **AC1.** Every instance has a plant and a team.
- **AC2.** The Instances list, instance details, package downloads and the Download Demo License instance list follow §9.2, on screen and on the server.
- **AC3.** Create Instance requires plant and team, filled in and locked as in §9.3.
- **AC4.** Only admins can change an instance's plant or team (§9.4).

### Story 7: Move a project to another company
*As Frinks staff, I want a moved project to arrive clean in the new company, so that no old access carries over.*
- **AC1.** Members are removed, the creator shows "Transferred", and all instances become drafts.
- **AC2.** Only the new company's Org Admins see the drafts and can assign a plant and team.
- **AC3.** After assignment, plant and team IDs update everywhere, including Edge.

### Story 8: Menus and other access
*As any user, I want menus to show me what I can and can't use.*
- **AC1.** Menu items and the Download Demo License button follow §12.
- **AC2.** Opening a page the person can't use, by typing its web address, redirects to Projects, and the server refuses the data.

---

## 17. How we'll know it worked

| Measure | Target |
|---|---|
| Role edits needed when a new project is created | 0 |
| Time for an admin to add a user with plant, team and roles | Under 1 minute |
| Access-related support tickets per month | Down from today's level |

---

## 18. Decisions and open questions

### 18.1 Decided

| # | Decision |
|---|---|
| D1 | Plants and teams only; divisions and sub-divisions come later. One plant and one team per user. Org Admins need neither. |
| D2 | Admin roles: Org Admin and Plant Admin. A Plant Admin can make Plant Admins for their own plant, never an Org Admin. |
| D3 | New roles: Manage Users – Organisation and Manage Users – Plant. They add, edit and deactivate users in their scope and give only roles they hold. |
| D4 | New Viewer role, pre-selected for new users. Standard-User-only people move to Viewer. |
| D5 | Project Management adds cameras, variants and classes. Recipe Management adds recipes (captures). |
| D6 | Annotations: View plus Create, with Edit and Delete tied to Create. Auto-annotation is part of it. No annotation download. |
| D7 | Dataset download is images only, and needs Datasets Management. |
| D8 | The project creator is added automatically, can't be removed, and has no special rights. |
| D9 | Project creators can add users from any plant. Invited users can't be added. |
| D10 | Unavailable actions are greyed out with a hover message. |
| D11 | Everyone can see the Users and Roles lists. ⋮ menus only for people who can act. |
| D12 | Only Org Admins edit or delete custom roles. |
| D13 | Instances need a plant and team. Visibility, downloads, demo licence and details follow the same rule. Only admins move instances. |
| D14 | Change Org: members removed, creator "Transferred", instances become drafts, the new Org Admin assigns plant and team. |
| D15 | Frinks Org Admins act as admins in customer companies and are hidden from their Users list. |
| D16 | Inferencing Customizations and keygen: code commented out, nothing in the UI. |

### 18.2 Defaults to confirm

| # | Default used in this draft |
|---|---|
| C1 | Plant Admins need a plant but no team. |
| C2 | Only Org Admins **create** custom roles too, not just edit and delete them. |
| C3 | Only Org Admins change the Plants and Teams lists. Everyone else can view them. |
| C4 | Plant Admins move instances only between teams in their own plant. Only Org Admins move instances or users to another plant. |
| C5 | Plant Admins have every permission in every project, including deleting a project that has other plants' instances. |
| C6 | For the Manage Users roles, "delete" means setting the user to Inactive. There is no permanent delete. |
| C7 | Manage Users holders can edit only users whose roles they all hold, so never admins. |
| C8 | Manage Users holders can always give Viewer. The Organisation-level role can also give Manage Users – Plant. |
| C9 | Plant Admins can give Manage Users – Plant but not Manage Users – Organisation. |
| C10 | FrinksEdge Download and Manual Assistant menu items are greyed out, not hidden, when the switch is off. |
| C11 | Inactive users keep their project memberships. Re-activating restores access. |
| C12 | Someone who loses an admin role keeps access to projects they created. |

### 18.3 Still open

| # | Question |
|---|---|
| Q1 | How long can someone stay logged in (idle timeout and maximum session)? |
| Q2 | Button-by-button permission list: to follow from the PM. |
