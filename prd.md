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

- Every user belongs to **one plant and one team**. Org Admins don't need a plant or team.
- **Everyone is a Viewer by default.** A user with no role can view every section of the projects they're added to.
- Roles add more on top of that, and a user can hold several. All built-in roles are locked:
  - **1 admin role:** Org Admin;
  - **3 organisation roles:** Manage Users – Organisation, Manage Users – Plant, Manage Plants & Teams;
  - **6 work roles:** Project Management, Recipe Management, Datasets Management, Annotation, AI Models, Instance Management.
- Org Admins can add custom roles.
- Three tools are switched on or off per user with toggles: **FrinksEdge Download, Download Demo License and Manual Assistant**.
- People see a project only when they've been **added to it**. Org Admins see every project.
- Inside a project, people see only the **instances of their own plant and team**.
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
2. Give every user a plant and a team, view access by default, and roles for anything more.
3. Provide ready-made, locked roles so most companies never need to build a role.
4. Let project creators choose exactly who can access a project.
5. Let trusted people manage users, plants and teams without making them admins.
6. Enforce every permission on the server, not just on screen.

**Not in this version**
- Divisions and sub-divisions (planned for later).
- Plant-level or division-level admins.
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
| **Viewer** | The default role everyone has. View access to every section of the projects they're on. |
| **Role** | A named set of permissions added on top of Viewer. A user can hold several. |
| **User manager** | Someone holding Manage Users – Organisation or Manage Users – Plant. |
| **Other access** | Three per-user toggles: FrinksEdge Download, Download Demo License, Manual Assistant. |
| **Project member** | A user who has been added to a project. |
| **Project creator** | The person who created a project. Always a member and can't be removed. |
| **Instance tag** | The plant and team an instance belongs to. |
| **Draft instance** | An instance with no plant or team yet, e.g. after its project moved to another company. |

---

## 5. Plants and Teams

- No change from today's setup: the **Teams & Plants** section keeps one fixed list of plants and one fixed list of teams. They are not linked to each other.
- **New plants and teams can be added. Existing ones can't be renamed or deleted.**
- **Org Admins and Manage Plants & Teams holders** can add them. Everyone else can only view the lists.
- Names must be unique within each list.
- Because the lists are fixed, an Org Admin can add users or create projects in any order.

---

## 6. Users

### 6.1 Fields

| Field | Required | Notes |
|---|---|---|
| Name | Yes | Accept real names, including accents, hyphens and apostrophes. |
| Email | Yes | Can't be changed after the invite. Accept all valid domains. |
| Phone | No | |
| Plant | Yes, except Org Admins | One plant per user. |
| Team | Yes, except Org Admins | One team per user. |
| Roles | No | Several allowed. Nothing is pre-selected. |
| Other access | — | Three toggles. See §6.4. |
| Status | — | Active or Inactive. New users show **Invited** until they set their password. |

**Note at the bottom of the Roles box:** *"If you don't assign any role, this user will have View access to all sections of Vision, in the projects they're added to."*

### 6.2 Who can manage users

| Action | Org Admin | Manage Users – Organisation | Manage Users – Plant | Everyone else |
|---|---|---|---|---|
| See the Users list | Yes | Yes | Yes | Yes, read-only |
| Add users | In any plant | In any plant | In own plant | No |
| Edit users: name, phone, team, status | Anyone | Anyone except Org Admins | Users in own plant, except Org Admins | No |
| Change a user's plant | Yes | Yes | No | No |
| Add or remove roles | Any role | Only roles they hold, plus Manage Users – Plant | Only roles they hold | No |
| Turn Other access toggles on or off | All three | Only ones they have on | Only ones they have on | No |
| Delete users | Anyone except the last Org Admin | Anyone except Org Admins | Users in own plant, except Org Admins | No |

Rows in the Users list show a ⋮ menu only to people who can act on that user.

### 6.3 Rules for user managers
- **Roles and toggles are the only limits.** User managers can edit everything about a user in their scope, but can add or remove only:
  - roles they hold themselves;
  - Other access toggles they have on themselves.
- Roles and toggles they can't give show greyed out, with the hover message *"You can only give access you have yourself."*
- Manage Users – Organisation holders can also give or remove **Manage Users – Plant**, because a plant sits inside the organisation.
- They can't change or delete **Org Admins**.
- Manage Users – Plant holders work only with users of their own plant, and can't move anyone to another plant.
- These roles give nothing inside projects. Holders see a project only if they've been added to it.

### 6.4 Other access (per-user toggles)
These are three on/off toggles in the Add/Edit User drawer.

| Toggle | What it unlocks | Default for a new user |
|---|---|---|
| **FrinksEdge Download** | The FrinksEdge Download page | Off |
| **Download Demo License** | The Download Demo License button on the Projects page | Off |
| **Manual Assistant** | The Manual Assistant chatbot and manuals | On |

- **Org Admins always have all three.** For them the toggles show as on and locked: *"Always on for Org Admins"*.
- The Users list shows each person's toggles in an **Other Access** column.

### 6.5 Status, delete and safety rules
- **Active or Inactive:** Org Admins and user managers switch a user between the two from the user drawer or the ⋮ menu. Inactive users are logged out immediately and can't log in. Their project memberships are kept, so making them Active again restores their access.
- **Delete:** permanent. A popup asks *"Delete Ravi Kumar? This can't be undone."* What happens:
  - the user is removed from every project, but **their work stays** (annotations, datasets, models and the rest);
  - projects they created show *"Name (deactivated)"* as the creator;
  - their email can be invited again later, as a new user.
- **Invited users** can be sent the invite again. They can't be added to projects until they've set their password.
- **No self-changes:** nobody can change their own plant, team, roles, toggles or status, or delete themselves.
- **Last Org Admin:** the last active Org Admin can't be deactivated, deleted or lose Org Admin. The message reads *"Make someone else an Org Admin first."*
- **Removing Org Admin** from someone:
  - they keep any other roles they have. If they have none, they become a Viewer;
  - no warning is shown;
  - they keep access to projects they created, and are added to other projects as needed.

---

## 7. Roles

All built-in roles are **locked**: they can't be edited or deleted.

### 7.1 Viewer (default for everyone)
- Every user is a Viewer. They don't need to be given this role.
- A Viewer can open **every section** of the projects they're added to and see everything in it. Every action is greyed out unless another role allows it.
- Other roles add actions on top of Viewer. For example, someone with Recipe Management but not AI Models can open the AI Models page and see all models and results, but Train, Edit Thresholds and Delete are greyed out.
- In the Users list, a user with no other role shows **Viewer** in the Role(s) column.

### 7.2 Admin role: Org Admin
- Sees **every project** and has **every permission** inside each one.
- Sees **all instances**, including drafts.
- The only person who can move an instance to another plant or team.
- Manages all users and can give any role (§6.2).
- The only person who creates, edits and deletes custom roles.
- Adds new plants and teams.
- Always has FrinksEdge Download, Download Demo License and Manual Assistant.
- Doesn't need a plant or team.

### 7.3 Organisation roles

| Role | What it adds |
|---|---|
| **Manage Users – Organisation** | Add, edit and delete users in any plant. Roles and toggles are limited as in §6.3. |
| **Manage Users – Plant** | The same, within their own plant. |
| **Manage Plants & Teams** | Add new plants and teams (§5). Existing ones can't be renamed or deleted. |

### 7.4 Work roles

| Role | What it adds on top of Viewer |
|---|---|
| **Project Management** | Create projects (name, description, inspection speed, product flow, cameras, variants and classes), edit them and delete them. Manage project access. |
| **Recipe Management** | Create, edit and delete recipes (captures), including their module configuration. |
| **Datasets Management** | Create datasets and upload images, edit and delete them, and download images. |
| **Annotation** | Annotate existing images, run auto-annotation, and edit and delete annotations. |
| **AI Models** | Train models, edit thresholds on the training results page, and delete models. |
| **Instance Management** | Create, edit and delete instances, and download instance packages, within their own plant and team. |

### 7.5 Several roles
A user with several roles gets everything those roles allow combined. For example, *Annotation + Datasets Management* can upload images, annotate them and download the images.

### 7.6 Custom roles
- Only **Org Admins** can create, edit and delete custom roles.
- The editor keeps today's two steps, **Basic Information → Permissions**. The Finalize step is removed.
- The Permissions step is one grid. View is always on, because everyone is a Viewer. A dash means the action doesn't exist for that area:

| Area | Create | View | Edit | Delete | Download |
|---|---|---|---|---|---|
| Project Details | ✓ | Always on | ✓ | ✓ | — |
| Recipe | ✓ | Always on | ✓ | ✓ | — |
| Datasets | ✓ | Always on | ✓ | ✓ | ✓ (images) |
| Annotations | ✓ | Always on | ✓ | ✓ | — |
| AI Models | ✓ | Always on | ✓ | ✓ | — |
| Instances | ✓ | Always on | ✓ | ✓ | ✓ (package) |

- **Annotations:** Create, Edit and Delete always go together. Ticking any one ticks all three, and unticking one unticks all three. Create includes auto-annotation.
- Custom roles cover project work only. Managing users, plants and teams stays with Org Admins and the organisation roles.
- **Save** stays greyed out until something changes.
- A custom role can't be deleted while anyone holds it, active or inactive. The message names the holders. If nobody holds it, it is deleted after a confirmation popup.
- Role names are unique, and names of deleted roles can be reused.

### 7.7 The Roles page
- **Columns:** Role Name, Type (Default / Admin / Organisation / Work / Custom), Description, Users Assigned.
  - Built-in roles show a lock.
  - Viewer shows **Everyone** under Users Assigned.
- **Everyone** can see the list.
- **Org Admins** get a ⋮ menu:
  - **View** on every role;
  - **Edit** and **Delete** on custom roles.

---

## 8. Projects

### 8.1 Who sees a project
- **Org Admins** see every project.
- **Everyone else** sees only projects they're a member of. This includes Project Management holders and user managers.
- If a non-admin is on no project, the Projects page says *"You have not been added to any project yet. Please contact your administrator."*

### 8.2 Creating a project
- **Who can create:** Org Admins and Project Management holders.
- The Create Project drawer keeps its fields: name, description, inspection speed, product flow, cameras, variants and classes.
- A new section at the end, **Project Access**, lists the users who can be added:
  - users from **any plant**, filtered by plant and team, and searchable;
  - each user's plant, team and roles are shown, with "Viewer" for users with no other role;
  - Org Admins aren't listed, because they always have access;
  - invited and inactive users aren't listed.
- The section header reads *"N users selected · Org Admins always have access."*
- A project can be created with nobody else added. Org Admins still see it, and members can be added later.

### 8.3 The project creator
- The creator is added automatically and **can't be removed** by anyone.
- When the project is opened again in the Edit Project drawer, a **greyed-out "Project Creator"** field shows who created it.
- If the creator is made Inactive or deleted, they show as *"Name (deactivated)"*.
- If the project moves to another company, the creator shows as *"Transferred"* (§10).

### 8.4 Changing access later
- **Manage Access** in the project card's ⋮ menu opens the same user picker.
- Available to Org Admins, and to Project Management holders who are members of that project.
- Project Management holders can remove other Project Management holders, but never the creator.
- When someone is removed, **their work stays**: annotations, datasets, models and the rest.

### 8.5 Deleting a project
Only Org Admins and Project Management holders can delete a project. The creator has no special right to delete.

### 8.6 Inside a project
Everyone on the project can open every section and see everything in it. Actions they can't use are **greyed out with a hover message**.

| Section | Everyone on the project | Needs this role |
|---|---|---|
| Project (card and drawer) | Open the project | Edit, Manage Access, Delete → Project Management |
| Recipe Management | View recipes | Add, Configure, Delete recipes → Recipe Management |
| Datasets & Annotations | View datasets and annotations | Create Dataset, Upload, Edit, Delete, Download images → Datasets Management; Annotate, Auto-annotate, Edit, Delete annotations → Annotation |
| AI Models | View models and results | Train, Edit Thresholds, Delete → AI Models |
| Instances | View instances of their plant and team | Create, Edit, Delete, Download package → Instance Management |

**Example hover messages:**
- *"You need Recipe Management to add recipes."*
- *"You need AI Models to edit thresholds."*
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
| Org Admin | Every plant and team, including drafts |
| Everyone else | Their own plant **and** their own team |

The same rule applies to:
- the Instances list and instance details;
- instance package downloads;
- the projects and instances offered in Download Demo License.

**Examples** (team names can repeat across plants, so the plant must match too)

| Instance tag | Priya (Pune / Line B) | Arjun (Chennai / Line B) | Ananya (Org Admin) |
|---|---|---|---|
| Pune / Line A | No | No | Yes |
| Pune / Line B | Yes | No | Yes |
| Chennai / Line B | No | Yes | Yes |
| Chennai / Final Audit | No | No | Yes |

### 9.3 Creating an instance
Plant and Team are both required.

| Who | Plant | Team |
|---|---|---|
| Org Admin | Any | Any |
| Instance Management holder | Own plant, locked | Own team, locked |

### 9.4 Moving an instance
- Only Org Admins can change an instance's plant or team.
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
6. The new company's Org Admins see the project straight away and add members.

---

## 11. Frinks staff

- Frinks Org Admins act as Org Admins inside customer companies.
- Their names don't appear in the customer's Users list.
- Only Frinks admins can switch between organisations.

---

## 12. Navigation and unavailable actions

| Menu item | Who can use it |
|---|---|
| Projects | Everyone |
| Teams & Plants | Everyone can view; Org Admins and Manage Plants & Teams holders can add new plants and teams |
| Users & Permissions → Users, Roles | Everyone can view; actions per §6 and §7.7 |
| FrinksEdge Download | Org Admins, and users with the toggle on. Others see it greyed out |
| Manual Assistant | Org Admins, and users with the toggle on. Others see it greyed out |
| Download Demo License button (Projects page) | Org Admins, and users with the toggle on. Others see it greyed out |

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
- A separate admin type whose only difference is creating projects would overlap with the Org Admin, who can already do this.

---

## 14. Server rules

These are required for v2 to be trustworthy.

1. **Every permission is checked on the server for every request,** including reads. A greyed-out button is never the only protection.
2. **Company wall:** the server confirms that every item belongs to the user's company.
3. **The server works out the project, plant, team and company from the item itself,** never from what the browser sends.
4. **Changes apply immediately:** a change to roles, toggles, status, placement or project access, or a delete, takes effect on the person's next action, with no re-login.
5. **Locked roles are locked on the server,** not just on screen.

---

## 15. Moving existing data over

| Today | v2 | How |
|---|---|---|
| Plants and teams | Plants and teams | Kept as they are. |
| User plant and team | User plant and team | Kept. Non-admins missing either are listed so an Org Admin can fill them in. |
| Org Admin role | Org Admin | Direct. |
| Other people with company-level rights, including people who manage users, plants or teams today | Decided one by one | They are listed. For each, decide Org Admin, an organisation role, or work roles only. |
| Standard User only (12 people today) | Viewer | They get no role, which makes them Viewers. |
| Custom roles (per-item ticks) | Built-in or custom roles | For each role, collect every permission type it has on any project and make that its standard. Match to built-in roles where they fit; keep the rest as custom roles. |
| Several roles per user | Several roles per user | Kept. |
| Access to a project | Project membership | Anyone with any permission on a project today becomes a member. |
| Project creator | Project creator | Kept. Creators who were deleted show as "Name (deactivated)". |
| Instance plant and team | Instance plant and team | Kept. Instances missing either are listed so an Org Admin can fill them in. |
| FrinksEdge Download, Demo License, Manual Assistant (visible to everyone today) | Other access toggles | Turned on for all existing users, so nobody loses access on the day. |

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
*As an Org Admin or Manage Plants & Teams holder, I want to add new plants and teams, so that users and instances can be placed correctly.*
- **AC1.** Teams & Plants shows two fixed lists: plants and teams.
- **AC2.** Only Org Admins and Manage Plants & Teams holders can add a plant or team. Everyone else sees the Add button greyed out with a hover message.
- **AC3.** Existing plants and teams can't be renamed or deleted. There are no Rename or Delete actions.
- **AC4.** A name already in the list is refused with *"A plant named X already exists."*

### Story 2: Add, edit and delete users
*As an Org Admin or user manager, I want to give a user a plant, a team, roles and other access, so that they get the right access.*
- **AC1.** Plant and Team are required for everyone except Org Admins.
- **AC2.** Roles are optional and several can be selected. Nothing is pre-selected.
- **AC3.** The note under the Roles box reads *"If you don't assign any role, this user will have View access to all sections of Vision, in the projects they're added to."* A user saved with no role shows as Viewer.
- **AC4.** User managers can edit any user in their scope except Org Admins. They can add or remove only roles they hold. The Organisation-level role can also give Manage Users – Plant. Other roles show greyed out with a hover message.
- **AC5.** Manage Users – Plant holders can add and edit only users of their own plant, and the Plant field is locked for them.
- **AC6.** The three Other access toggles appear with defaults off / off / on. For Org Admins they show on and locked. User managers can turn on or off only the ones they have on themselves.
- **AC7.** Status can be set to Active or Inactive. Inactive users are logged out at once and keep their project memberships.
- **AC8.** Delete is permanent and asks for confirmation. The user is removed from all projects, their work stays, projects they created show "Name (deactivated)", and the email can be invited again.
- **AC9.** Changing your own plant, team, roles, toggles or status, or deleting yourself, is blocked. The last active Org Admin is protected.
- **AC10.** A failed save never deletes the user. It shows an error and keeps the user as they were.
- **AC11.** Everyone can see the Users list. The ⋮ menu shows only on rows the person can act on. Frinks staff aren't listed.

### Story 3: Roles
*As an Org Admin, I want ready-made roles and the option to create my own, so that I rarely have to build roles from scratch.*
- **AC1.** The 11 built-in roles exist in every company, show a lock and can only be viewed: Viewer, Org Admin, Manage Users – Organisation, Manage Users – Plant, Manage Plants & Teams, Project Management, Recipe Management, Datasets Management, Annotation, AI Models, Instance Management.
- **AC2.** Only Org Admins can create, edit or delete custom roles, using the grid in §7.6.
- **AC3.** In the grid, Annotations' Create, Edit and Delete always tick and untick together.
- **AC4.** Save is greyed out until something changes. Duplicate names are refused with *"A role named X already exists."*
- **AC5.** A role held by anyone, active or inactive, can't be deleted, and the message names the holders.

### Story 4: Create a project and choose who can access it
*As a Project Management holder or Org Admin, I want to choose who can access my project, so that only the right people see it.*
- **AC1.** Create Project ends with a Project Access section, as described in §8.2.
- **AC2.** The creator is added automatically and can't be removed. Edit Project shows a greyed-out "Project Creator" field.
- **AC3.** A project can be saved with nobody else added.
- **AC4.** Manage Access on the project card opens the same picker, for Org Admins and for Project Management members.
- **AC5.** Non-admins see only projects they're a member of.

### Story 5: Work inside a project
*As a project member, I want to see everything in the project and use only the actions my roles allow.*
- **AC1.** Every member can open every section and see everything in it.
- **AC2.** Actions the person can't use are greyed out with a hover message naming the role needed (§8.6). The server refuses them too. For example, a Recipe Management holder without AI Models can open the AI Models page, but Train, Edit Thresholds and Delete are greyed out.
- **AC3.** Classes can be added only in the project drawer. Module configuration and the annotation job no longer offer it.
- **AC4.** Dataset download offers images only. There is no annotation download.
- **AC5.** A user with several roles gets all the actions of all their roles.

### Story 6: See only my instances
*As a line engineer, I want to see only the instances of my plant and team, so that I don't act on another line's machines.*
- **AC1.** Every instance has a plant and a team.
- **AC2.** The Instances list, instance details, package downloads and the Download Demo License lists follow §9.2, on screen and on the server.
- **AC3.** Create Instance requires plant and team, filled in and locked as in §9.3.
- **AC4.** Only Org Admins can change an instance's plant or team.

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

## 18. Decisions


| # | Decision |
|---|---|
| D1 | Plants and teams only; divisions and sub-divisions come later. One plant and one team per user. Org Admins need neither. |
| D2 | One admin role: Org Admin. No plant-level or division-level admins. |
| D3 | Everyone is a Viewer by default. Roles are optional. A note under the Roles box explains that no role means View access. |
| D4 | Two user-manager roles, Manage Users – Organisation and Manage Users – Plant. They add, edit and delete users in their scope. They change roles and Other access toggles only within what they hold themselves. The Organisation-level role can also give Manage Users – Plant. |
| D5 | User managers can't change or delete Org Admins. Manage Users – Plant holders can't move users to another plant. |
| D6 | Plants and teams stay fixed flat lists, as today. New ones can be added; existing ones can't be renamed or deleted. Org Admins and the new Manage Plants & Teams role can add them. |
| D7 | Users can be deleted permanently, as well as set Inactive. Inactive users keep their project memberships. A deleted user's email can be invited again. |
| D8 | Project Management adds cameras, variants and classes. Recipe Management adds recipes (captures). |
| D9 | Annotations: View plus Create, with Edit and Delete tied to Create. Auto-annotation is part of it. No annotation download. |
| D10 | Dataset download is images only, and needs Datasets Management. |
| D11 | The project creator is added automatically, can't be removed, and has no special rights. Edit Project shows a greyed-out "Project Creator" field. |
| D12 | Project creators can add users from any plant. Invited users can't be added. |
| D13 | Unavailable actions are greyed out with a hover message. This includes menu items for toggles that are off. |
| D14 | Everyone can see the Users and Roles lists. ⋮ menus only for people who can act. |
| D15 | Only Org Admins create, edit and delete custom roles. |
| D16 | Instances need a plant and team. Visibility, details, downloads and the demo licence follow the same rule. Only Org Admins move instances. |
| D17 | Change Org: members removed, creator "Transferred", all instances become drafts, the new Org Admin assigns plant and team. |
| D18 | Frinks Org Admins act as Org Admins in customer companies and are hidden from their Users list. |
| D19 | Someone who loses Org Admin keeps access to projects they created. |
| D20 | Inferencing Customizations and keygen: code commented out, nothing in the UI. |
| D21 | Removing Org Admin from someone with no other role leaves them as a Viewer. |
